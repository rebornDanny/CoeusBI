13 views

```sql
CREATE VIEW v_seasons AS
SELECT
seasons.year,
seasons.url AS season_url
FROM seasons;

CREATE VIEW v_circuits AS
SELECT
circuits.circuitId,
circuits.circuitRef,
circuits.name,
circuits.location,
circuits.country,
circuits.lat,
circuits.lng,
circuits.alt,
circuits.url
FROM circuits;

CREATE VIEW v_constructors AS
SELECT
constructors.constructorId,
constructors.constructorRef,
constructors.name,
constructors.nationality,
constructors.url
FROM constructors;

CREATE VIEW v_drivers AS
SELECT
drivers.driverId,
drivers.driverRef,
drivers.number,
drivers.code,
drivers.forename,
drivers.surname,
(drivers.forename || ' ' || drivers.surname) AS driver_name,
drivers.dob,
drivers.nationality,
drivers.url
FROM drivers;

CREATE VIEW v_races AS
SELECT
r.raceId,
r.year,
r.round,
r.name AS race_name,
r.date,
r.time,
r.url AS race_url,
r.circuitId,
c.circuitRef,
c.name AS circuit_name,
c.location AS circuit_location,
c.country AS circuit_country,
c.lat AS circuit_lat,
c.lng AS circuit_lng,
c.alt AS circuit_alt,
c.url AS circuit_url,
s.url AS season_url
FROM races r
LEFT JOIN circuits c ON r.circuitId = c.circuitId
LEFT JOIN seasons s ON r.year = s.year;

CREATE VIEW v_results AS
SELECT
res.resultId,
res.raceId,
r.year,
r.round,
r.name AS race_name,
r.date AS race_date,
r.time AS race_time,
r.url AS race_url,
r.circuitId,
c.circuitRef,
c.name AS circuit_name,
c.country AS circuit_country,
res.driverId,
d.driverRef,
d.forename,
d.surname,
(d.forename || ' ' || d.surname) AS driver_name,
d.number AS driver_number,
d.code AS driver_code,
d.nationality AS driver_nationality,
res.constructorId,
cons.constructorRef,
cons.name AS constructor_name,
cons.nationality AS constructor_nationality,
res.grid,
res.position,
res.positionText,
res.positionOrder,
res.points,
res.laps,
res.time,
res.milliseconds,
res.fastestLap,
res.rank,
res.fastestLapTime,
res.fastestLapSpeed,
res.statusId,
st.status,
s.url AS season_url
FROM results res
LEFT JOIN races r ON res.raceId = r.raceId
LEFT JOIN circuits c ON r.circuitId = c.circuitId
LEFT JOIN drivers d ON res.driverId = d.driverId
LEFT JOIN constructors cons ON res.constructorId = cons.constructorId
LEFT JOIN status st ON res.statusId = st.statusId
LEFT JOIN seasons s ON r.year = s.year;

CREATE VIEW v_qualifying AS
SELECT
q.qualifyId,
q.raceId,
r.year,
r.round,
r.name AS race_name,
r.date AS race_date,
r.time AS race_time,
r.url AS race_url,
r.circuitId,
c.circuitRef,
c.name AS circuit_name,
q.driverId,
d.driverRef,
d.forename,
d.surname,
(d.forename || ' ' || d.surname) AS driver_name,
d.number AS driver_number,
d.code AS driver_code,
d.nationality AS driver_nationality,
q.constructorId,
cons.constructorRef,
cons.name AS constructor_name,
cons.nationality AS constructor_nationality,
q.number,
q.position,
q.q1,
q.q2,
q.q3,
s.url AS season_url
FROM qualifying q
LEFT JOIN races r ON q.raceId = r.raceId
LEFT JOIN circuits c ON r.circuitId = c.circuitId
LEFT JOIN drivers d ON q.driverId = d.driverId
LEFT JOIN constructors cons ON q.constructorId = cons.constructorId
LEFT JOIN seasons s ON r.year = s.year;

CREATE VIEW v_driver_standings AS
SELECT
ds.driverStandingsId,
ds.raceId,
r.year,
r.round,
r.name AS race_name,
r.date AS race_date,
r.time AS race_time,
r.url AS race_url,
r.circuitId,
c.circuitRef,
c.name AS circuit_name,
ds.driverId,
d.driverRef,
d.forename,
d.surname,
(d.forename || ' ' || d.surname) AS driver_name,
d.nationality AS driver_nationality,
ds.points,
ds.position,
ds.positionText,
ds.wins,
s.url AS season_url
FROM driverStandings ds
LEFT JOIN races r ON ds.raceId = r.raceId
LEFT JOIN circuits c ON r.circuitId = c.circuitId
LEFT JOIN drivers d ON ds.driverId = d.driverId
LEFT JOIN seasons s ON r.year = s.year;

CREATE VIEW v_constructor_standings AS
SELECT
cs.constructorStandingsId,
cs.raceId,
r.year,
r.round,
r.name AS race_name,
r.date AS race_date,
r.time AS race_time,
r.url AS race_url,
r.circuitId,
c.circuitRef,
c.name AS circuit_name,
cs.constructorId,
cons.constructorRef,
cons.name AS constructor_name,
cons.nationality AS constructor_nationality,
cs.points,
cs.position,
cs.positionText,
cs.wins,
s.url AS season_url
FROM constructorStandings cs
LEFT JOIN races r ON cs.raceId = r.raceId
LEFT JOIN circuits c ON r.circuitId = c.circuitId
LEFT JOIN constructors cons ON cs.constructorId = cons.constructorId
LEFT JOIN seasons s ON r.year = s.year;

CREATE VIEW v_lap_times AS
SELECT
lt.raceId,
r.year,
r.round,
r.name AS race_name,
r.date AS race_date,
r.time AS race_time,
r.url AS race_url,
r.circuitId,
c.circuitRef,
c.name AS circuit_name,
lt.driverId,
d.driverRef,
d.forename,
d.surname,
(d.forename || ' ' || d.surname) AS driver_name,
lt.lap,
lt.position,
lt.time,
lt.milliseconds,
s.url AS season_url
FROM lapTimes lt
LEFT JOIN races r ON lt.raceId = r.raceId
LEFT JOIN circuits c ON r.circuitId = c.circuitId
LEFT JOIN drivers d ON lt.driverId = d.driverId
LEFT JOIN seasons s ON r.year = s.year;

CREATE VIEW v_pit_stops AS
SELECT
ps.raceId,
r.year,
r.round,
r.name AS race_name,
r.date AS race_date,
r.time AS race_time,
r.url AS race_url,
r.circuitId,
c.circuitRef,
c.name AS circuit_name,
ps.driverId,
d.driverRef,
d.forename,
d.surname,
(d.forename || ' ' || d.surname) AS driver_name,
ps.stop,
ps.lap,
ps.time,
ps.duration,
ps.milliseconds,
s.url AS season_url
FROM pitStops ps
LEFT JOIN races r ON ps.raceId = r.raceId
LEFT JOIN circuits c ON r.circuitId = c.circuitId
LEFT JOIN drivers d ON ps.driverId = d.driverId
LEFT JOIN seasons s ON r.year = s.year;

CREATE VIEW v_constructor_results AS
SELECT
cr.constructorResultsId,
cr.raceId,
r.year,
r.round,
r.name AS race_name,
r.date AS race_date,
r.time AS race_time,
r.url AS race_url,
r.circuitId,
c.circuitRef,
c.name AS circuit_name,
cr.constructorId,
cons.constructorRef,
cons.name AS constructor_name,
cons.nationality AS constructor_nationality,
cr.points,
cr.status,
s.url AS season_url
FROM constructorResults cr
LEFT JOIN races r ON cr.raceId = r.raceId
LEFT JOIN circuits c ON r.circuitId = c.circuitId
LEFT JOIN constructors cons ON cr.constructorId = cons.constructorId
LEFT JOIN seasons s ON r.year = s.year;

CREATE VIEW v_status AS
SELECT
status.statusId,
status.status
FROM status;
```
