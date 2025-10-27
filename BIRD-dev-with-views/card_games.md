6 views

```sql
CREATE VIEW IF NOT EXISTS "v_cards_core" AS
SELECT
c."uuid" AS card_uuid,
c."id" AS card_id,
c."name" AS card_name,
c."asciiName" AS card_ascii_name,
c."manaCost" AS mana_cost,
c."convertedManaCost" AS mana_value,
c."colorIdentity" AS color_identity,
c."colors" AS colors,
c."type" AS card_type_line,
c."types" AS card_types,
c."supertypes" AS card_supertypes,
c."subtypes" AS card_subtypes,
c."power" AS power,
c."toughness" AS toughness,
c."loyalty" AS loyalty,
c."text" AS rules_text,
c."flavorText" AS flavor_text,
c."rarity" AS rarity,
c."layout" AS layout,
c."frameEffects" AS frame_effects,
c."frameVersion" AS frame_version,
c."isFullArt" AS is_full_art,
c."isPromo" AS is_promo,
c."isReprint" AS is_reprint,
c."hasFoil" AS has_foil,
c."hasNonFoil" AS has_nonfoil,
c."availability" AS availability,
c."artist" AS artist,
c."number" AS collector_number,
c."setCode" AS set_code,
s."name" AS set_name,
s."type" AS set_type,
s."releaseDate" AS set_release_date,
s."baseSetSize" AS set_base_size,
s."totalSetSize" AS set_total_size,
s."block" AS set_block,
s."isOnlineOnly" AS set_is_online_only,
s."isFoilOnly" AS set_is_foil_only
FROM "cards" c
LEFT JOIN "sets" s ON s."code" = c."setCode";

CREATE VIEW IF NOT EXISTS "v_cards_legalities" AS
SELECT
c."uuid" AS card_uuid,
c."name" AS card_name,
c."setCode" AS set_code,
s."name" AS set_name,
l."format" AS format,
l."status" AS legality_status
FROM "cards" c
LEFT JOIN "legalities" l ON l."uuid" = c."uuid"
LEFT JOIN "sets" s ON s."code" = c."setCode";

CREATE VIEW IF NOT EXISTS "v_cards_translations" AS
SELECT
c."uuid" AS card_uuid,
c."name" AS card_name_en,
fd."language" AS card_language,
fd."name" AS card_name_local,
fd."type" AS card_type_local,
fd."text" AS card_text_local,
fd."flavorText" AS card_flavor_text_local,
c."setCode" AS set_code,
s."name" AS set_name_en,
st."language" AS set_language,
st."translation" AS set_name_local
FROM "cards" c
LEFT JOIN "foreign_data" fd ON fd."uuid" = c."uuid"
LEFT JOIN "sets" s ON s."code" = c."setCode"
LEFT JOIN "set_translations" st ON st."setCode" = c."setCode";

CREATE VIEW IF NOT EXISTS "v_cards_rulings" AS
SELECT
c."uuid" AS card_uuid,
c."name" AS card_name,
r."id" AS ruling_id,
r."date" AS ruling_date,
r."text" AS ruling_text,
c."setCode" AS set_code,
s."name" AS set_name
FROM "cards" c
LEFT JOIN "rulings" r ON r."uuid" = c."uuid"
LEFT JOIN "sets" s ON s."code" = c."setCode";

CREATE VIEW IF NOT EXISTS "v_sets_with_translations" AS
SELECT
s."code" AS set_code,
s."name" AS set_name_en,
s."type" AS set_type,
s."releaseDate" AS set_release_date,
s."baseSetSize" AS set_base_size,
s."totalSetSize" AS set_total_size,
s."block" AS set_block,
s."parentCode" AS parent_set_code,
s."isOnlineOnly" AS is_online_only,
s."isFoilOnly" AS is_foil_only,
st."language" AS language,
st."translation" AS set_name_local
FROM "sets" s
LEFT JOIN "set_translations" st ON st."setCode" = s."code";

CREATE VIEW IF NOT EXISTS "v_cards_allfacts" AS
SELECT
c."uuid" AS card_uuid,
c."id" AS card_id,
c."name" AS card_name,
c."asciiName" AS card_ascii_name,
c."manaCost" AS mana_cost,
c."convertedManaCost" AS mana_value,
c."colorIdentity" AS color_identity,
c."colors" AS colors,
c."type" AS card_type_line,
c."types" AS card_types,
c."supertypes" AS card_supertypes,
c."subtypes" AS card_subtypes,
c."power" AS power,
c."toughness" AS toughness,
c."loyalty" AS loyalty,
c."text" AS rules_text,
c."flavorText" AS flavor_text,
c."rarity" AS rarity,
c."layout" AS layout,
c."frameEffects" AS frame_effects,
c."frameVersion" AS frame_version,
c."isFullArt" AS is_full_art,
c."isPromo" AS is_promo,
c."isReprint" AS is_reprint,
c."hasFoil" AS has_foil,
c."hasNonFoil" AS has_nonfoil,
c."availability" AS availability,
c."artist" AS artist,
c."number" AS collector_number,
c."setCode" AS set_code,
s."name" AS set_name,
s."type" AS set_type,
s."releaseDate" AS set_release_date,
s."baseSetSize" AS set_base_size,
s."totalSetSize" AS set_total_size,
s."block" AS set_block,
s."isOnlineOnly" AS set_is_online_only,
s."isFoilOnly" AS set_is_foil_only,
l."format" AS legality_format,
l."status" AS legality_status,
fd."language" AS card_language,
fd."name" AS card_name_local,
fd."type" AS card_type_local,
fd."text" AS card_text_local,
fd."flavorText" AS card_flavor_text_local,
st."language" AS set_language,
st."translation" AS set_name_local,
r."id" AS ruling_id,
r."date" AS ruling_date,
r."text" AS ruling_text
FROM "cards" c
LEFT JOIN "sets" s ON s."code" = c."setCode"
LEFT JOIN "legalities" l ON l."uuid" = c."uuid"
LEFT JOIN "foreign_data" fd ON fd."uuid" = c."uuid"
LEFT JOIN "set_translations" st ON st."setCode" = c."setCode"
LEFT JOIN "rulings" r ON r."uuid" = c."uuid";


```

