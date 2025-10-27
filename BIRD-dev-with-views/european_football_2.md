8 views

```sql
CREATE VIEW IF NOT EXISTS vw_countries AS
SELECT
c.id AS country_id,
c.name AS country_name
FROM Country c;

CREATE VIEW IF NOT EXISTS vw_leagues AS
SELECT
l.id AS league_id,
l.name AS league_name,
l.country_id,
c.name AS country_name
FROM League l
LEFT JOIN Country c ON l.country_id = c.id;

CREATE VIEW IF NOT EXISTS vw_teams AS
SELECT
t.id AS team_row_id,
t.team_api_id,
t.team_fifa_api_id,
t.team_long_name,
t.team_short_name
FROM Team t;

CREATE VIEW IF NOT EXISTS vw_team_attributes AS
SELECT
ta.id AS team_attributes_id,
ta.team_api_id,
ta.team_fifa_api_id,
ta.date,
ta.buildUpPlaySpeed,
ta.buildUpPlaySpeedClass,
ta.buildUpPlayDribbling,
ta.buildUpPlayDribblingClass,
ta.buildUpPlayPassing,
ta.buildUpPlayPassingClass,
ta.buildUpPlayPositioningClass,
ta.chanceCreationPassing,
ta.chanceCreationPassingClass,
ta.chanceCreationCrossing,
ta.chanceCreationCrossingClass,
ta.chanceCreationShooting,
ta.chanceCreationShootingClass,
ta.chanceCreationPositioningClass,
ta.defencePressure,
ta.defencePressureClass,
ta.defenceAggression,
ta.defenceAggressionClass,
ta.defenceTeamWidth,
ta.defenceTeamWidthClass,
ta.defenceDefenderLineClass,
t.team_long_name,
t.team_short_name
FROM Team_Attributes ta
LEFT JOIN Team t ON ta.team_api_id = t.team_api_id;

CREATE VIEW IF NOT EXISTS vw_players AS
SELECT
p.id AS player_row_id,
p.player_api_id,
p.player_fifa_api_id,
p.player_name,
p.birthday,
p.height,
p.weight
FROM Player p;

CREATE VIEW IF NOT EXISTS vw_player_attributes AS
SELECT
pa.id AS player_attributes_id,
pa.player_api_id,
pa.player_fifa_api_id,
pa.date,
pa.overall_rating,
pa.potential,
pa.preferred_foot,
pa.attacking_work_rate,
pa.defensive_work_rate,
pa.crossing,
pa.finishing,
pa.heading_accuracy,
pa.short_passing,
pa.volleys,
pa.dribbling,
pa.curve,
pa.free_kick_accuracy,
pa.long_passing,
pa.ball_control,
pa.acceleration,
pa.sprint_speed,
pa.agility,
pa.reactions,
pa.balance,
pa.shot_power,
pa.jumping,
pa.stamina,
pa.strength,
pa.long_shots,
pa.aggression,
pa.interceptions,
pa.positioning,
pa.vision,
pa.penalties,
pa.marking,
pa.standing_tackle,
pa.sliding_tackle,
pa.gk_diving,
pa.gk_handling,
pa.gk_kicking,
pa.gk_positioning,
pa.gk_reflexes,
p.player_name,
p.birthday,
p.height,
p.weight
FROM Player_Attributes pa
LEFT JOIN Player p ON pa.player_api_id = p.player_api_id;

CREATE VIEW IF NOT EXISTS vw_matches AS
SELECT
m.id AS match_id,
m.match_api_id,
m.country_id,
c.name AS country_name,
m.league_id,
l.name AS league_name,
m.season,
m.stage,
m.date,
m.home_team_api_id,
ht.team_long_name AS home_team_long_name,
ht.team_short_name AS home_team_short_name,
m.away_team_api_id,
at.team_long_name AS away_team_long_name,
at.team_short_name AS away_team_short_name,
m.home_team_goal,
m.away_team_goal,
m.home_player_X1,
m.home_player_X2,
m.home_player_X3,
m.home_player_X4,
m.home_player_X5,
m.home_player_X6,
m.home_player_X7,
m.home_player_X8,
m.home_player_X9,
m.home_player_X10,
m.home_player_X11,
m.away_player_X1,
m.away_player_X2,
m.away_player_X3,
m.away_player_X4,
m.away_player_X5,
m.away_player_X6,
m.away_player_X7,
m.away_player_X8,
m.away_player_X9,
m.away_player_X10,
m.away_player_X11,
m.home_player_Y1,
m.home_player_Y2,
m.home_player_Y3,
m.home_player_Y4,
m.home_player_Y5,
m.home_player_Y6,
m.home_player_Y7,
m.home_player_Y8,
m.home_player_Y9,
m.home_player_Y10,
m.home_player_Y11,
m.away_player_Y1,
m.away_player_Y2,
m.away_player_Y3,
m.away_player_Y4,
m.away_player_Y5,
m.away_player_Y6,
m.away_player_Y7,
m.away_player_Y8,
m.away_player_Y9,
m.away_player_Y10,
m.away_player_Y11,
m.home_player_1,
m.home_player_2,
m.home_player_3,
m.home_player_4,
m.home_player_5,
m.home_player_6,
m.home_player_7,
m.home_player_8,
m.home_player_9,
m.home_player_10,
m.home_player_11,
m.away_player_1,
m.away_player_2,
m.away_player_3,
m.away_player_4,
m.away_player_5,
m.away_player_6,
m.away_player_7,
m.away_player_8,
m.away_player_9,
m.away_player_10,
m.away_player_11,
m.goal,
m.shoton,
m.shotoff,
m.foulcommit,
m.card,
m."cross",
m.corner,
m.possession,
m.B365H, m.B365D, m.B365A,
m.BWH, m.BWD, m.BWA,
m.IWH, m.IWD, m.IWA,
m.LBH, m.LBD, m.LBA,
m.PSH, m.PSD, m.PSA,
m.WHH, m.WHD, m.WHA,
m.SJH, m.SJD, m.SJA,
m.VCH, m.VCD, m.VCA,
m.GBH, m.GBD, m.GBA,
m.BSH, m.BSD, m.BSA
FROM "Match" m
LEFT JOIN Country c ON m.country_id = c.id
LEFT JOIN League l ON m.league_id = l.id
LEFT JOIN Team ht ON m.home_team_api_id = ht.team_api_id
LEFT JOIN Team at ON m.away_team_api_id = at.team_api_id;

CREATE VIEW IF NOT EXISTS vw_match_lineups AS
SELECT
m.id AS match_id,
m.match_api_id,
m.season,
m.stage,
m.date,
m.league_id,
l.name AS league_name,
m.country_id,
c.name AS country_name,
m.home_team_api_id,
ht.team_long_name AS home_team_long_name,
m.away_team_api_id,
at.team_long_name AS away_team_long_name,
m.home_team_goal,
m.away_team_goal,
m.home_player_1,
p_h1.player_name AS home_player_1_name,
m.home_player_2,
p_h2.player_name AS home_player_2_name,
m.home_player_3,
p_h3.player_name AS home_player_3_name,
m.home_player_4,
p_h4.player_name AS home_player_4_name,
m.home_player_5,
p_h5.player_name AS home_player_5_name,
m.home_player_6,
p_h6.player_name AS home_player_6_name,
m.home_player_7,
p_h7.player_name AS home_player_7_name,
m.home_player_8,
p_h8.player_name AS home_player_8_name,
m.home_player_9,
p_h9.player_name AS home_player_9_name,
m.home_player_10,
p_h10.player_name AS home_player_10_name,
m.home_player_11,
p_h11.player_name AS home_player_11_name,
m.away_player_1,
p_a1.player_name AS away_player_1_name,
m.away_player_2,
p_a2.player_name AS away_player_2_name,
m.away_player_3,
p_a3.player_name AS away_player_3_name,
m.away_player_4,
p_a4.player_name AS away_player_4_name,
m.away_player_5,
p_a5.player_name AS away_player_5_name,
m.away_player_6,
p_a6.player_name AS away_player_6_name,
m.away_player_7,
p_a7.player_name AS away_player_7_name,
m.away_player_8,
p_a8.player_name AS away_player_8_name,
m.away_player_9,
p_a9.player_name AS away_player_9_name,
m.away_player_10,
p_a10.player_name AS away_player_10_name,
m.away_player_11,
p_a11.player_name AS away_player_11_name
FROM "Match" m
LEFT JOIN League l ON m.league_id = l.id
LEFT JOIN Country c ON m.country_id = c.id
LEFT JOIN Team ht ON m.home_team_api_id = ht.team_api_id
LEFT JOIN Team at ON m.away_team_api_id = at.team_api_id
LEFT JOIN Player p_h1 ON m.home_player_1 = p_h1.player_api_id
LEFT JOIN Player p_h2 ON m.home_player_2 = p_h2.player_api_id
LEFT JOIN Player p_h3 ON m.home_player_3 = p_h3.player_api_id
LEFT JOIN Player p_h4 ON m.home_player_4 = p_h4.player_api_id
LEFT JOIN Player p_h5 ON m.home_player_5 = p_h5.player_api_id
LEFT JOIN Player p_h6 ON m.home_player_6 = p_h6.player_api_id
LEFT JOIN Player p_h7 ON m.home_player_7 = p_h7.player_api_id
LEFT JOIN Player p_h8 ON m.home_player_8 = p_h8.player_api_id
LEFT JOIN Player p_h9 ON m.home_player_9 = p_h9.player_api_id
LEFT JOIN Player p_h10 ON m.home_player_10 = p_h10.player_api_id
LEFT JOIN Player p_h11 ON m.home_player_11 = p_h11.player_api_id
LEFT JOIN Player p_a1 ON m.away_player_1 = p_a1.player_api_id
LEFT JOIN Player p_a2 ON m.away_player_2 = p_a2.player_api_id
LEFT JOIN Player p_a3 ON m.away_player_3 = p_a3.player_api_id
LEFT JOIN Player p_a4 ON m.away_player_4 = p_a4.player_api_id
LEFT JOIN Player p_a5 ON m.away_player_5 = p_a5.player_api_id
LEFT JOIN Player p_a6 ON m.away_player_6 = p_a6.player_api_id
LEFT JOIN Player p_a7 ON m.away_player_7 = p_a7.player_api_id
LEFT JOIN Player p_a8 ON m.away_player_8 = p_a8.player_api_id
LEFT JOIN Player p_a9 ON m.away_player_9 = p_a9.player_api_id
LEFT JOIN Player p_a10 ON m.away_player_10 = p_a10.player_api_id
LEFT JOIN Player p_a11 ON m.away_player_11 = p_a11.player_api_id;


```
