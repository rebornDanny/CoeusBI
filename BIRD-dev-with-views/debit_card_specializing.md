5 views

```sql
CREATE VIEW vw_sales AS
SELECT
t.TransactionID,
t.Date,
t.Time,
(t.Date || ' ' || COALESCE(t.Time, '00:00:00')) AS DateTime,
-- Customer
t.CustomerID,
c.Segment AS CustomerSegment,
c.Currency,
t.CardID,
-- Gas station
t.GasStationID,
gs.ChainID,
gs.Country,
gs.Segment AS GasStationSegment,
-- Product
t.ProductID,
p.Description AS ProductDescription,
-- Measures
t.Amount,
t.Price,
COALESCE(t.Amount, 0) * COALESCE(t.Price, 0.0) AS TotalPrice,
-- Date parts
substr(t.Date, 1, 4) AS Year,
substr(t.Date, 6, 2) AS Month,
substr(t.Date, 9, 2) AS Day,
substr(t.Date, 1, 7) AS YearMonth,
-- Monthly customer consumption for the transaction month
ym.Consumption AS MonthlyConsumption
FROM "transactions_1k" AS t
LEFT JOIN customers AS c
ON c.CustomerID = t.CustomerID
LEFT JOIN gasstations AS gs
ON gs.GasStationID = t.GasStationID
LEFT JOIN products AS p
ON p.ProductID = t.ProductID
LEFT JOIN "yearmonth" AS ym
ON ym.CustomerID = t.CustomerID
AND ym.Date = substr(t.Date, 1, 7);

CREATE VIEW vw_customer_months AS
SELECT
c.CustomerID,
c.Segment,
c.Currency,
ym.Date AS YearMonth,
substr(ym.Date, 1, 4) AS Year,
substr(ym.Date, 6, 2) AS Month,
ym.Consumption
FROM customers AS c
LEFT JOIN "yearmonth" AS ym
ON ym.CustomerID = c.CustomerID;

CREATE VIEW vw_customers AS
SELECT
CustomerID,
Segment,
Currency
FROM customers;

CREATE VIEW vw_gasstations AS
SELECT
GasStationID,
ChainID,
Country,
Segment
FROM gasstations;

CREATE VIEW vw_products AS
SELECT
ProductID,
Description
FROM products;
```
