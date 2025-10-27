6 views

```sql
CREATE VIEW v_patient AS
SELECT
p.ID,
p.SEX,
p.Birthday,
p.Description,
p."First Date",
p.Admission,
p.Diagnosis,
CAST((julianday(p."First Date") - julianday(p.Birthday)) / 365.25 AS INTEGER) AS AgeAtFirstVisit
FROM Patient AS p;

CREATE VIEW v_patient_laboratory AS
SELECT
p.ID,
p.SEX,
p.Birthday,
p.Description,
p."First Date",
p.Admission,
p.Diagnosis AS Patient_Diagnosis,
l.Date,
CAST((julianday(l.Date) - julianday(p.Birthday)) / 365.25 AS INTEGER) AS AgeAtLabDate,
l.GOT,
l.GPT,
l.LDH,
l.ALP,
l.TP,
l.ALB,
l.UA,
l.UN,
l.CRE,
l."T-BIL",
l."T-CHO",
l.TG,
l.CPK,
l.GLU,
l.WBC,
l.RBC,
l.HGB,
l.HCT,
l.PLT,
l.PT,
l.APTT,
l.FG,
l.PIC,
l.TAT,
l.TAT2,
l."U-PRO",
l.IGG,
l.IGA,
l.IGM,
l.CRP,
l.RA,
l.RF,
l.C3,
l.C4,
l.RNP,
l.SM,
l.SC170,
l.SSA,
l.SSB,
l.CENTROMEA,
l.DNA,
l."DNA-II"
FROM Patient AS p
LEFT JOIN Laboratory AS l
ON l.ID = p.ID;

CREATE VIEW v_patient_examination AS
SELECT
p.ID,
p.SEX,
p.Birthday,
p.Description,
p."First Date",
p.Admission,
p.Diagnosis AS Patient_Diagnosis,
e."Examination Date",
CAST((julianday(e."Examination Date") - julianday(p.Birthday)) / 365.25 AS INTEGER) AS AgeAtExamDate,
e."aCL IgG",
e."aCL IgM",
e.ANA,
e."ANA Pattern",
e."aCL IgA",
e.Diagnosis AS Exam_Diagnosis,
e.KCT,
e.RVVT,
e.LAC,
e.Symptoms,
e.Thrombosis
FROM Patient AS p
LEFT JOIN Examination AS e
ON e.ID = p.ID;

CREATE VIEW v_patient_latest_laboratory AS
SELECT
p.ID,
p.SEX,
p.Birthday,
p.Description,
p."First Date",
p.Admission,
p.Diagnosis AS Patient_Diagnosis,
l.Date,
CAST((julianday(l.Date) - julianday(p.Birthday)) / 365.25 AS INTEGER) AS AgeAtLabDate,
l.GOT,
l.GPT,
l.LDH,
l.ALP,
l.TP,
l.ALB,
l.UA,
l.UN,
l.CRE,
l."T-BIL",
l."T-CHO",
l.TG,
l.CPK,
l.GLU,
l.WBC,
l.RBC,
l.HGB,
l.HCT,
l.PLT,
l.PT,
l.APTT,
l.FG,
l.PIC,
l.TAT,
l.TAT2,
l."U-PRO",
l.IGG,
l.IGA,
l.IGM,
l.CRP,
l.RA,
l.RF,
l.C3,
l.C4,
l.RNP,
l.SM,
l.SC170,
l.SSA,
l.SSB,
l.CENTROMEA,
l.DNA,
l."DNA-II"
FROM Patient AS p
LEFT JOIN (
SELECT l1.*
FROM Laboratory AS l1
JOIN (
SELECT ID, MAX(Date) AS Date
FROM Laboratory
GROUP BY ID
) AS m
ON m.ID = l1.ID AND m.Date = l1.Date
) AS l
ON l.ID = p.ID;

CREATE VIEW v_patient_latest_examination AS
SELECT
p.ID,
p.SEX,
p.Birthday,
p.Description,
p."First Date",
p.Admission,
p.Diagnosis AS Patient_Diagnosis,
e."Examination Date",
CAST((julianday(e."Examination Date") - julianday(p.Birthday)) / 365.25 AS INTEGER) AS AgeAtExamDate,
e."aCL IgG",
e."aCL IgM",
e.ANA,
e."ANA Pattern",
e."aCL IgA",
e.Diagnosis AS Exam_Diagnosis,
e.KCT,
e.RVVT,
e.LAC,
e.Symptoms,
e.Thrombosis
FROM Patient AS p
LEFT JOIN (
SELECT e1.*
FROM Examination AS e1
JOIN (
SELECT ID, MAX("Examination Date") AS "Examination Date"
FROM Examination
GROUP BY ID
) AS m
ON m.ID = e1.ID AND m."Examination Date" = e1."Examination Date"
) AS e
ON e.ID = p.ID;

CREATE VIEW v_patient_examination_with_same_day_laboratory AS
SELECT
p.ID,
p.SEX,
p.Birthday,
p.Description,
p."First Date",
p.Admission,
p.Diagnosis AS Patient_Diagnosis,
e."Examination Date",
CAST((julianday(e."Examination Date") - julianday(p.Birthday)) / 365.25 AS INTEGER) AS AgeAtExamDate,
e."aCL IgG",
e."aCL IgM",
e.ANA,
e."ANA Pattern",
e."aCL IgA",
e.Diagnosis AS Exam_Diagnosis,
e.KCT,
e.RVVT,
e.LAC,
e.Symptoms,
e.Thrombosis,
l.Date AS Lab_Date,
l.GOT,
l.GPT,
l.LDH,
l.ALP,
l.TP,
l.ALB,
l.UA,
l.UN,
l.CRE,
l."T-BIL",
l."T-CHO",
l.TG,
l.CPK,
l.GLU,
l.WBC,
l.RBC,
l.HGB,
l.HCT,
l.PLT,
l.PT,
l.APTT,
l.FG,
l.PIC,
l.TAT,
l.TAT2,
l."U-PRO",
l.IGG,
l.IGA,
l.IGM,
l.CRP,
l.RA,
l.RF,
l.C3,
l.C4,
l.RNP,
l.SM,
l.SC170,
l.SSA,
l.SSB,
l.CENTROMEA,
l.DNA,
l."DNA-II"
FROM Patient AS p
LEFT JOIN Examination AS e
ON e.ID = p.ID
LEFT JOIN Laboratory AS l
ON l.ID = p.ID
AND l.Date = e."Examination Date";
```
