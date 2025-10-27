9 views

```sql
CREATE VIEW vw_account_dim AS
SELECT
a.account_id,
a.district_id,
d.A2 AS district_name,
d.A3 AS region,
a.frequency,
a.date AS account_open_date,
d.A10 AS urban_ratio,
d.A11 AS average_salary,
d.A12 AS unemployment_rate_1995,
d.A13 AS unemployment_rate_1996,
d.A14 AS entrepreneurs_per_1000,
d.A15 AS crimes_1995,
d.A16 AS crimes_1996
FROM account a
LEFT JOIN district d ON a.district_id = d.district_id;

CREATE VIEW vw_account_client_card AS
SELECT
a.account_id,
a.frequency,
a.date AS account_open_date,
a.district_id AS account_district_id,
ad.A2 AS account_district_name,
ad.A3 AS account_region,
disp.disp_id,
disp.type AS disp_type,
c.client_id,
c.gender,
c.birth_date,
c.district_id AS client_district_id,
cd.A2 AS client_district_name,
cd.A3 AS client_region,
card.card_id,
card.type AS card_type,
card.issued AS card_issued_date
FROM account a
LEFT JOIN district ad ON a.district_id = ad.district_id
LEFT JOIN disp ON disp.account_id = a.account_id
LEFT JOIN client c ON c.client_id = disp.client_id
LEFT JOIN district cd ON c.district_id = cd.district_id
LEFT JOIN card ON card.disp_id = disp.disp_id;

CREATE VIEW vw_account_loans AS
SELECT
a.account_id,
a.frequency,
a.date AS account_open_date,
a.district_id,
d.A2 AS account_district_name,
d.A3 AS account_region,
l.loan_id,
l.date AS loan_date,
l.amount AS loan_amount,
l.duration AS loan_duration,
l.payments AS loan_monthly_payment,
l.status AS loan_status
FROM account a
LEFT JOIN district d ON a.district_id = d.district_id
LEFT JOIN loan l ON l.account_id = a.account_id;

CREATE VIEW vw_account_orders AS
SELECT
a.account_id,
a.frequency,
a.date AS account_open_date,
a.district_id,
d.A2 AS account_district_name,
d.A3 AS account_region,
o.order_id,
o.bank_to,
o.account_to,
o.amount AS order_amount,
o.k_symbol AS order_k_symbol
FROM account a
LEFT JOIN district d ON a.district_id = d.district_id
LEFT JOIN "order" o ON o.account_id = a.account_id;

CREATE VIEW vw_account_transactions AS
SELECT
a.account_id,
a.frequency,
a.date AS account_open_date,
a.district_id,
d.A2 AS account_district_name,
d.A3 AS account_region,
t.trans_id,
t.date AS trans_date,
t.type AS trans_type,
t.operation,
t.amount AS trans_amount,
t.balance AS trans_balance,
t.k_symbol AS trans_k_symbol,
t.bank AS partner_bank,
t.account AS partner_account
FROM account a
LEFT JOIN district d ON a.district_id = d.district_id
LEFT JOIN trans t ON t.account_id = a.account_id;

CREATE VIEW vw_client_accounts AS
SELECT
c.client_id,
c.gender,
c.birth_date,
c.district_id AS client_district_id,
cd.A2 AS client_district_name,
cd.A3 AS client_region,
disp.disp_id,
disp.type AS disp_type,
a.account_id,
a.frequency,
a.date AS account_open_date,
a.district_id AS account_district_id,
ad.A2 AS account_district_name,
ad.A3 AS account_region
FROM client c
LEFT JOIN district cd ON c.district_id = cd.district_id
LEFT JOIN disp ON disp.client_id = c.client_id
LEFT JOIN account a ON a.account_id = disp.account_id
LEFT JOIN district ad ON a.district_id = ad.district_id;

CREATE VIEW vw_card_holders AS
SELECT
card.card_id,
card.type AS card_type,
card.issued AS card_issued_date,
disp.disp_id,
disp.type AS disp_type,
c.client_id,
c.gender,
c.birth_date,
c.district_id AS client_district_id,
cd.A2 AS client_district_name,
cd.A3 AS client_region,
a.account_id,
a.frequency,
a.date AS account_open_date,
a.district_id AS account_district_id,
ad.A2 AS account_district_name,
ad.A3 AS account_region
FROM card
LEFT JOIN disp ON card.disp_id = disp.disp_id
LEFT JOIN client c ON disp.client_id = c.client_id
LEFT JOIN district cd ON c.district_id = cd.district_id
LEFT JOIN account a ON disp.account_id = a.account_id
LEFT JOIN district ad ON a.district_id = ad.district_id;

CREATE VIEW vw_district_dim AS
SELECT
district_id,
A2 AS district_name,
A3 AS region,
A4 AS inhabitants,
A5 AS municipalities_lt_499,
A6 AS municipalities_500_1999,
A7 AS municipalities_2000_9999,
A8 AS municipalities_gt_10000,
A9 AS aux_A9,
A10 AS urban_ratio,
A11 AS average_salary,
A12 AS unemployment_rate_1995,
A13 AS unemployment_rate_1996,
A14 AS entrepreneurs_per_1000,
A15 AS crimes_1995,
A16 AS crimes_1996
FROM district;

CREATE VIEW vw_loans_enriched AS
SELECT
l.loan_id,
l.account_id,
l.date AS loan_date,
l.amount AS loan_amount,
l.duration AS loan_duration,
l.payments AS loan_monthly_payment,
l.status AS loan_status,
a.district_id AS account_district_id,
ad.A2 AS account_district_name,
ad.A3 AS account_region,
disp.disp_id,
disp.type AS disp_type,
c.client_id,
c.gender,
c.birth_date,
c.district_id AS client_district_id,
cd.A2 AS client_district_name,
cd.A3 AS client_region,
t.trans_id AS repayment_trans_id,
t.date AS repayment_trans_date,
t.amount AS repayment_amount,
t.balance AS balance_after_repayment,
t.k_symbol AS repayment_k_symbol
FROM loan l
LEFT JOIN account a ON l.account_id = a.account_id
LEFT JOIN district ad ON a.district_id = ad.district_id
LEFT JOIN disp ON disp.account_id = a.account_id
LEFT JOIN client c ON c.client_id = disp.client_id
LEFT JOIN district cd ON c.district_id = cd.district_id
LEFT JOIN trans t ON t.account_id = a.account_id AND t.k_symbol = 'UVER';
```
