# Practice 1

**1. Syntax with ORDER BY**

A simple syntax to select bmi and age fields from the data set, order the result in descending order and only select the 5 first rows of the result.


```sql
SELECT 	        bmi,
	        age
FROM		health
ORDER BY	bmi DESC
LIMIT	        5;
```
**2. Syntax with CASE WHEN**

to categorize each patient to different groups.

```sql
SELECT	CASE 
WHEN	sbp > 180 OR dbp > 120 THEN 'Hypertensive Crisis'
WHEN	sbp >= 140 OR dbp >= 90 THEN 'Hypertension Stage 2'
WHEN	(sbp >= 130 AND sbp <= 139) OR (dbp >= 80 AND dbp <= 89) THEN 'Hypertension Stage 1'
WHEN	(sbp >= 120 AND sbp <= 129) AND (dbp < 80) THEN 'Elevated'
WHEN	sbp < 120 AND dbp < 80 THEN 'Normal'
ELSE	NULL
END AS BP_Group,
COUNT(*) AS Total_Patients,
ROUND(AVG(bmi),2) AS Avg_BMI
	FROM		health
	WHERE	status = 'Alive' AND
			sbp >= 0 AND
			dbp >= 0
	GROUP BY	BP_Group;
```

**3. Syntax to work with date data**

```sql
SELECT	AVG(TIMESTAMPDIFF(MINUTE,
        MAKETIME(SUBSTR(CAST(CRSDepTime AS CHAR),1, LENGTH(TRIM(CAST(CRSDepTime AS CHAR)))-2),RIGHT(CRSDepTime,2),0),
        MAKETIME(SUBSTR(CAST(DepTime AS CHAR),1,LENGTH(TRIM(CAST(DepTime AS CHAR)))-2),RIGHT(DepTime,2),0)
            )) AS MyAVGDelay,
        AVG(DepDelay)
FROM 		ontime
WHERE		(CRSDepTime >= 800 AND CRSDepTime <= 1600) AND 					ABS(DepDelay) < 360 AND 
			Cancelled = 0 AND 
			Diverted = 0;
```



