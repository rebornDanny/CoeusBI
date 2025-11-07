12 views

```sql
CREATE VIEW IF NOT EXISTS v_post_comment_stats AS
SELECT
c.PostId,
COUNT(*) AS comment_count_calc,
SUM(CASE WHEN c.Score > 60 THEN 1 ELSE 0 END) AS positive_comment_count,
SUM(CASE WHEN c.Score < 60 THEN 1 ELSE 0 END) AS negative_comment_count,
AVG(c.Score) AS avg_comment_score,
MAX(c.CreationDate) AS last_comment_date
FROM comments c
GROUP BY c.PostId;

CREATE VIEW IF NOT EXISTS v_post_vote_stats AS
SELECT
v.PostId,
COUNT(*) AS vote_count,
SUM(CASE WHEN v.BountyAmount IS NOT NULL THEN v.BountyAmount ELSE 0 END) AS total_bounty_amount,
MIN(v.CreationDate) AS first_vote_date,
MAX(v.CreationDate) AS last_vote_date
FROM votes v
GROUP BY v.PostId;

CREATE VIEW IF NOT EXISTS v_posts_enriched AS
SELECT
p.Id AS PostId,
p.PostTypeId,
p.AcceptedAnswerId,
p.CreaionDate,
p.Score,
p.ViewCount,
p.Body,
p.OwnerUserId,
p.LasActivityDate,
p.Title,
p.Tags,
p.AnswerCount,
p.CommentCount,
p.FavoriteCount,
p.LastEditorUserId,
p.LastEditDate,
p.CommunityOwnedDate,
p.ParentId,
p.ClosedDate,
p.OwnerDisplayName,
p.LastEditorDisplayName,
owner.DisplayName AS OwnerUser_DisplayName,
owner.Reputation AS OwnerUser_Reputation,
owner.Location AS OwnerUser_Location,
lasteditor.DisplayName AS LastEditorUser_DisplayName,
accepted.Title AS AcceptedAnswer_Title,
parent.Title AS Parent_Title,
pcs.comment_count_calc AS CalcCommentCount,
pcs.positive_comment_count,
pcs.negative_comment_count,
pcs.avg_comment_score,
pcs.last_comment_date,
pvs.vote_count,
pvs.total_bounty_amount,
pvs.first_vote_date,
pvs.last_vote_date
FROM posts p
LEFT JOIN users owner ON owner.Id = p.OwnerUserId
LEFT JOIN users lasteditor ON lasteditor.Id = p.LastEditorUserId
LEFT JOIN posts accepted ON accepted.Id = p.AcceptedAnswerId
LEFT JOIN posts parent ON parent.Id = p.ParentId
LEFT JOIN v_post_comment_stats pcs ON pcs.PostId = p.Id
LEFT JOIN v_post_vote_stats pvs ON pvs.PostId = p.Id;

CREATE VIEW IF NOT EXISTS v_post_tags_map AS
SELECT
p.Id AS PostId,
p.Title AS PostTitle,
t.Id AS TagId,
t.TagName,
t.Count AS TagGlobalCount
FROM posts p
LEFT JOIN tags t ON p.Tags LIKE '%<' || t.TagName || '>%';

CREATE VIEW IF NOT EXISTS v_posts_with_tags AS
SELECT
pe.*,
pt.TagId,
pt.TagName,
pt.TagGlobalCount
FROM v_posts_enriched pe
LEFT JOIN v_post_tags_map pt ON pt.PostId = pe.PostId;

CREATE VIEW IF NOT EXISTS v_comments_enriched AS
SELECT
c.Id AS CommentId,
c.PostId,
p.Title AS PostTitle,
p.PostTypeId AS PostTypeId,
c.Score,
c.Text,
c.CreationDate,
c.UserId AS CommenterUserId,
c.UserDisplayName AS CommenterDisplayName_FromComment,
u.DisplayName AS CommenterDisplayName_FromUser,
u.Reputation AS CommenterReputation,
CASE
WHEN c.Score > 60 THEN 'positive'
WHEN c.Score < 60 THEN 'negative'
ELSE 'neutral'
END AS CommentSentiment,
p.OwnerUserId AS PostOwnerUserId,
po.DisplayName AS PostOwnerDisplayName
FROM comments c
LEFT JOIN posts p ON p.Id = c.PostId
LEFT JOIN users u ON u.Id = c.UserId
LEFT JOIN users po ON po.Id = p.OwnerUserId;

CREATE VIEW IF NOT EXISTS v_votes_enriched AS
SELECT
v.Id AS VoteId,
v.PostId,
p.Title AS PostTitle,
v.VoteTypeId,
v.CreationDate,
v.UserId AS VoterUserId,
u.DisplayName AS VoterDisplayName,
u.Reputation AS VoterReputation,
v.BountyAmount
FROM votes v
LEFT JOIN posts p ON p.Id = v.PostId
LEFT JOIN users u ON u.Id = v.UserId;

CREATE VIEW IF NOT EXISTS v_badges_enriched AS
SELECT
b.Id AS BadgeId,
b.Name AS BadgeName,
b.Date AS BadgeDate,
b.UserId,
u.DisplayName AS UserDisplayName,
u.Reputation AS UserReputation,
u.Location AS UserLocation,
u.CreationDate AS UserCreationDate
FROM badges b
LEFT JOIN users u ON u.Id = b.UserId;

CREATE VIEW IF NOT EXISTS v_postlinks_enriched AS
SELECT
pl.Id AS LinkId,
pl.CreationDate,
pl.PostId,
p.Title AS PostTitle,
pl.RelatedPostId,
rp.Title AS RelatedPostTitle,
pl.LinkTypeId
FROM postLinks pl
LEFT JOIN posts p ON p.Id = pl.PostId
LEFT JOIN posts rp ON rp.Id = pl.RelatedPostId;

CREATE VIEW IF NOT EXISTS v_posthistory_enriched AS
SELECT
ph.Id AS PostHistoryId,
ph.PostHistoryTypeId,
ph.PostId,
p.Title AS PostTitle,
ph.RevisionGUID,
ph.CreationDate,
ph.UserId AS EditorUserId,
ph.UserDisplayName AS EditorDisplayName_FromHistory,
u.DisplayName AS EditorDisplayName_FromUser,
ph.Text,
ph.Comment
FROM postHistory ph
LEFT JOIN posts p ON p.Id = ph.PostId
LEFT JOIN users u ON u.Id = ph.UserId;

CREATE VIEW IF NOT EXISTS v_tags_enriched AS
SELECT
t.Id AS TagId,
t.TagName,
t.Count AS TagCount,
t.ExcerptPostId,
ep.Title AS ExcerptPostTitle,
t.WikiPostId,
wp.Title AS WikiPostTitle
FROM tags t
LEFT JOIN posts ep ON ep.Id = t.ExcerptPostId
LEFT JOIN posts wp ON wp.Id = t.WikiPostId;

CREATE VIEW IF NOT EXISTS v_user_activity_stats AS
SELECT
u.Id AS UserId,
u.DisplayName,
u.Reputation,
u.Location,
u.CreationDate,
u.LastAccessDate,
u.WebsiteUrl,
u.Views,
u.UpVotes,
u.DownVotes,
u.Age,
u.ProfileImageUrl,
COALESCE(ps.post_count, 0) AS post_count,
COALESCE(ps.question_count, 0) AS question_count,
COALESCE(ps.answer_count, 0) AS answer_count,
COALESCE(cs.comment_count, 0) AS comment_count,
COALESCE(vs.votes_cast, 0) AS votes_cast,
COALESCE(bs.badge_count, 0) AS badge_count,
phs.last_edit_date AS last_edit_date
FROM users u
LEFT JOIN (
SELECT
p.OwnerUserId AS UserId,
COUNT(*) AS post_count,
SUM(CASE WHEN p.ParentId IS NULL THEN 1 ELSE 0 END) AS question_count,
SUM(CASE WHEN p.ParentId IS NOT NULL THEN 1 ELSE 0 END) AS answer_count
FROM posts p
GROUP BY p.OwnerUserId
) ps ON ps.UserId = u.Id
LEFT JOIN (
SELECT c.UserId AS UserId, COUNT(*) AS comment_count
FROM comments c
GROUP BY c.UserId
) cs ON cs.UserId = u.Id
LEFT JOIN (
SELECT v.UserId AS UserId, COUNT(*) AS votes_cast
FROM votes v
GROUP BY v.UserId
) vs ON vs.UserId = u.Id
LEFT JOIN (
SELECT b.UserId AS UserId, COUNT(*) AS badge_count
FROM badges b
GROUP BY b.UserId
) bs ON bs.UserId = u.Id
LEFT JOIN (
SELECT ph.UserId AS UserId, MAX(ph.CreationDate) AS last_edit_date
FROM postHistory ph
GROUP BY ph.UserId
) phs ON phs.UserId = u.Id;
```
