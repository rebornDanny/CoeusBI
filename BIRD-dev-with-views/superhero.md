6 views

```sql
CREATE VIEW IF NOT EXISTS v_superhero_enriched AS
SELECT
s.id AS hero_id,
s.superhero_name,
s.full_name,
g.gender,
ec.colour AS eye_colour,
hc.colour AS hair_colour,
sc.colour AS skin_colour,
r.race,
p.publisher_name,
al.alignment,
s.height_cm,
s.weight_kg,
COUNT(DISTINCT hp.power_id) AS power_count,
GROUP_CONCAT(DISTINCT sp.power_name) AS power_list,
COUNT(DISTINCT ha.attribute_id) AS attribute_count,
GROUP_CONCAT(DISTINCT (attr.attribute_name || ':' || COALESCE(ha.attribute_value, ''))) AS attributes_list
FROM superhero s
LEFT JOIN gender g ON g.id = s.gender_id
LEFT JOIN colour ec ON ec.id = s.eye_colour_id
LEFT JOIN colour hc ON hc.id = s.hair_colour_id
LEFT JOIN colour sc ON sc.id = s.skin_colour_id
LEFT JOIN race r ON r.id = s.race_id
LEFT JOIN publisher p ON p.id = s.publisher_id
LEFT JOIN alignment al ON al.id = s.alignment_id
LEFT JOIN hero_power hp ON hp.hero_id = s.id
LEFT JOIN superpower sp ON sp.id = hp.power_id
LEFT JOIN hero_attribute ha ON ha.hero_id = s.id
LEFT JOIN attribute attr ON attr.id = ha.attribute_id
GROUP BY
s.id, s.superhero_name, s.full_name, g.gender,
ec.colour, hc.colour, sc.colour, r.race,
p.publisher_name, al.alignment, s.height_cm, s.weight_kg;

CREATE VIEW IF NOT EXISTS v_hero_power AS
SELECT
s.id AS hero_id,
s.superhero_name,
s.full_name,
p.publisher_name,
al.alignment,
sp.id AS power_id,
sp.power_name
FROM superhero s
LEFT JOIN publisher p ON p.id = s.publisher_id
LEFT JOIN alignment al ON al.id = s.alignment_id
LEFT JOIN hero_power hp ON hp.hero_id = s.id
LEFT JOIN superpower sp ON sp.id = hp.power_id;

CREATE VIEW IF NOT EXISTS v_hero_attribute AS
SELECT
s.id AS hero_id,
s.superhero_name,
s.full_name,
p.publisher_name,
al.alignment,
attr.id AS attribute_id,
attr.attribute_name,
ha.attribute_value
FROM superhero s
LEFT JOIN publisher p ON p.id = s.publisher_id
LEFT JOIN alignment al ON al.id = s.alignment_id
LEFT JOIN hero_attribute ha ON ha.hero_id = s.id
LEFT JOIN attribute attr ON attr.id = ha.attribute_id;

CREATE VIEW IF NOT EXISTS v_power_popularity AS
SELECT
sp.id AS power_id,
sp.power_name,
COUNT(DISTINCT hp.hero_id) AS hero_count
FROM superpower sp
LEFT JOIN hero_power hp ON hp.power_id = sp.id
GROUP BY sp.id, sp.power_name;

CREATE VIEW IF NOT EXISTS v_alignment_summary AS
SELECT
al.alignment,
COUNT(DISTINCT s.id) AS hero_count
FROM alignment al
LEFT JOIN superhero s ON s.alignment_id = al.id
GROUP BY al.alignment;

CREATE VIEW IF NOT EXISTS v_publisher_summary AS
SELECT
p.publisher_name,
COUNT(DISTINCT s.id) AS hero_count,
AVG(NULLIF(s.height_cm, 0)) AS avg_height_cm,
AVG(NULLIF(s.weight_kg, 0)) AS avg_weight_kg,
(1.0 * COUNT(DISTINCT CASE WHEN hp.power_id IS NOT NULL THEN s.id || '-' || hp.power_id END)
/ NULLIF(COUNT(DISTINCT s.id), 0)) AS avg_powers_per_hero
FROM publisher p
LEFT JOIN superhero s ON s.publisher_id = p.id
LEFT JOIN hero_power hp ON hp.hero_id = s.id
GROUP BY p.publisher_name;
```
