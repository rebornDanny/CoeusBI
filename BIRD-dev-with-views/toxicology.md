9 views

```sql
CREATE VIEW IF NOT EXISTS v_atom_degree AS
SELECT id AS atom_id, COUNT(*) AS degree
FROM (
SELECT atom_id AS id FROM connected
UNION ALL
SELECT atom_id2 AS id FROM connected
)
GROUP BY id;

CREATE VIEW IF NOT EXISTS v_atom_detail AS
SELECT
a.atom_id,
a.molecule_id,
m.label,
a.element,
COALESCE(d.degree, 0) AS degree
FROM atom a
LEFT JOIN molecule m ON m.molecule_id = a.molecule_id
LEFT JOIN v_atom_degree d ON d.atom_id = a.atom_id;

CREATE VIEW IF NOT EXISTS v_bond_detail AS
SELECT
b.bond_id,
b.molecule_id,
m.label,
b.bond_type,
c.atom_id AS atom_id1,
a1.element AS element1,
c.atom_id2 AS atom_id2,
a2.element AS element2
FROM bond b
LEFT JOIN molecule m ON m.molecule_id = b.molecule_id
LEFT JOIN connected c ON c.bond_id = b.bond_id
LEFT JOIN atom a1 ON a1.atom_id = c.atom_id
LEFT JOIN atom a2 ON a2.atom_id = c.atom_id2;

CREATE VIEW IF NOT EXISTS v_connected_detail AS
SELECT
m.molecule_id,
m.label,
c.bond_id,
b.bond_type,
c.atom_id AS atom_id1,
a1.element AS element1,
c.atom_id2 AS atom_id2,
a2.element AS element2
FROM atom a1
LEFT JOIN molecule m ON m.molecule_id = a1.molecule_id
LEFT JOIN connected c ON c.atom_id = a1.atom_id
LEFT JOIN atom a2 ON a2.atom_id = c.atom_id2
LEFT JOIN bond b ON b.bond_id = c.bond_id;

CREATE VIEW IF NOT EXISTS v_molecule_unified AS
SELECT
m.molecule_id,
m.label,
a.atom_id,
a.element,
c.bond_id,
b.bond_type,
c.atom_id2 AS neighbor_atom_id,
a2.element AS neighbor_element
FROM molecule m
LEFT JOIN atom a ON a.molecule_id = m.molecule_id
LEFT JOIN connected c ON c.atom_id = a.atom_id
LEFT JOIN atom a2 ON a2.atom_id = c.atom_id2
LEFT JOIN bond b ON b.bond_id = c.bond_id;

CREATE VIEW IF NOT EXISTS v_molecule_element_counts AS
SELECT
m.molecule_id,
m.label,
COUNT(a.atom_id) AS atom_count,
SUM(CASE WHEN lower(a.element) = 'c' THEN 1 ELSE 0 END) AS c_count,
SUM(CASE WHEN lower(a.element) = 'h' THEN 1 ELSE 0 END) AS h_count,
SUM(CASE WHEN lower(a.element) = 'o' THEN 1 ELSE 0 END) AS o_count,
SUM(CASE WHEN lower(a.element) = 'n' THEN 1 ELSE 0 END) AS n_count,
SUM(CASE WHEN lower(a.element) = 's' THEN 1 ELSE 0 END) AS s_count,
SUM(CASE WHEN lower(a.element) = 'p' THEN 1 ELSE 0 END) AS p_count,
SUM(CASE WHEN lower(a.element) = 'cl' THEN 1 ELSE 0 END) AS cl_count,
SUM(CASE WHEN lower(a.element) = 'br' THEN 1 ELSE 0 END) AS br_count,
SUM(CASE WHEN lower(a.element) = 'f' THEN 1 ELSE 0 END) AS f_count,
SUM(CASE WHEN lower(a.element) = 'i' THEN 1 ELSE 0 END) AS i_count,
SUM(CASE WHEN lower(a.element) = 'na' THEN 1 ELSE 0 END) AS na_count,
SUM(CASE WHEN lower(a.element) = 'sn' THEN 1 ELSE 0 END) AS sn_count,
SUM(CASE WHEN lower(a.element) = 'pb' THEN 1 ELSE 0 END) AS pb_count,
SUM(CASE WHEN lower(a.element) = 'te' THEN 1 ELSE 0 END) AS te_count,
SUM(CASE WHEN lower(a.element) = 'ca' THEN 1 ELSE 0 END) AS ca_count
FROM molecule m
LEFT JOIN atom a ON a.molecule_id = m.molecule_id
GROUP BY m.molecule_id, m.label;

CREATE VIEW IF NOT EXISTS v_molecule_bond_type_counts AS
SELECT
m.molecule_id,
m.label,
COUNT(b.bond_id) AS bond_count,
SUM(CASE WHEN b.bond_type = '-' THEN 1 ELSE 0 END) AS single_bond_count,
SUM(CASE WHEN b.bond_type = '=' THEN 1 ELSE 0 END) AS double_bond_count,
SUM(CASE WHEN b.bond_type = '#' THEN 1 ELSE 0 END) AS triple_bond_count
FROM molecule m
LEFT JOIN bond b ON b.molecule_id = m.molecule_id
GROUP BY m.molecule_id, m.label;

CREATE VIEW IF NOT EXISTS v_molecule_summary AS
SELECT
m.molecule_id,
m.label,
COALESCE(ec.atom_count, 0) AS atom_count,
COALESCE(bc.bond_count, 0) AS bond_count,
COALESCE(bc.single_bond_count, 0) AS single_bond_count,
COALESCE(bc.double_bond_count, 0) AS double_bond_count,
COALESCE(bc.triple_bond_count, 0) AS triple_bond_count,
COALESCE(ec.c_count, 0) AS c_count,
COALESCE(ec.h_count, 0) AS h_count,
COALESCE(ec.o_count, 0) AS o_count,
COALESCE(ec.n_count, 0) AS n_count,
COALESCE(ec.s_count, 0) AS s_count,
COALESCE(ec.p_count, 0) AS p_count,
COALESCE(ec.cl_count, 0) AS cl_count,
COALESCE(ec.br_count, 0) AS br_count,
COALESCE(ec.f_count, 0) AS f_count,
COALESCE(ec.i_count, 0) AS i_count,
COALESCE(ec.na_count, 0) AS na_count,
COALESCE(ec.sn_count, 0) AS sn_count,
COALESCE(ec.pb_count, 0) AS pb_count,
COALESCE(ec.te_count, 0) AS te_count,
COALESCE(ec.ca_count, 0) AS ca_count,
CASE WHEN COALESCE(ec.cl_count, 0) > 0 THEN 1 ELSE 0 END AS has_cl,
CASE WHEN COALESCE(ec.br_count, 0) > 0 THEN 1 ELSE 0 END AS has_br,
CASE WHEN COALESCE(ec.f_count, 0) > 0 THEN 1 ELSE 0 END AS has_f,
CASE WHEN COALESCE(ec.i_count, 0) > 0 THEN 1 ELSE 0 END AS has_i,
CASE WHEN COALESCE(bc.double_bond_count, 0) > 0 THEN 1 ELSE 0 END AS has_double_bond,
CASE WHEN COALESCE(bc.triple_bond_count, 0) > 0 THEN 1 ELSE 0 END AS has_triple_bond
FROM molecule m
LEFT JOIN v_molecule_element_counts ec ON ec.molecule_id = m.molecule_id
LEFT JOIN v_molecule_bond_type_counts bc ON bc.molecule_id = m.molecule_id;

CREATE VIEW IF NOT EXISTS v_atom_neighbors AS
SELECT
a.atom_id,
a.molecule_id,
m.label,
a.element,
COALESCE(d.degree, 0) AS degree,
group_concat(DISTINCT a2.atom_id) AS neighbor_atom_ids,
group_concat(DISTINCT a2.element) AS neighbor_elements
FROM atom a
LEFT JOIN molecule m ON m.molecule_id = a.molecule_id
LEFT JOIN connected c ON c.atom_id = a.atom_id
LEFT JOIN atom a2 ON a2.atom_id = c.atom_id2
LEFT JOIN v_atom_degree d ON d.atom_id = a.atom_id
GROUP BY a.atom_id, a.molecule_id, m.label, a.element, d.degree;
```
