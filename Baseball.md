# Practice 2

Working with baseball data set to find out the best player

```sql
SELECT birthYear
from   Master
where playerID = 'abercda01';
```


### CORE TABLE
```sql
WITH T1 AS
(
SELECT 		Batting.playerID,
		Batting.teamID,
                Master.birthYear,
                Batting.yearID,
                (Batting.yearID - Master.birthYear) AS age,
                Batting.AB,
                Batting.HR
FROM   		Batting
LEFT JOIN    	Master
ON		Batting.playerID = Master.playerID
),
T2 AS(
WITH R1 AS
(
SELECT 	    	playerID,
		yearID,
                SUM(G) AS Sum_non_P
FROM		Fielding
WHERE		POS NOT LIKE 'P'
GROUP BY	playerID,
		yearID
),
R2 AS
(SELECT		playerID,
		yearID,
		SUM(G) AS Sum_P
FROM 	        Fielding
WHERE	        POS = 'P'
GROUP BY	playerID,
		yearID
)
SELECT		R1.playerID,
		R1.yearID,
                Sum_P,
                Sum_non_P
FROM		R1
INNER JOIN	R2
ON		R1.playerID = R2.playerID AND
		R1.yearID = R2.yearID
)
SELECT	        age,
		SUM(HR) OVER(ORDER BY age) AS Cumulative_HR,
                SUM(AB) OVER(ORDER BY age) AS Cumulative_AB,
                SUM(HR) OVER(ORDER BY age)/SUM(AB) OVER(ORDER BY age) AS Cumulative_home_run_rate,
                HR/AB AS Non_cumulative_home_run_rate
FROM		T1
INNER JOIN	T2
ON		T1.playerID = T2.playerID AND
		T1.yearID = T2.yearID
WHERE	        T1.yearID >= 2000 AND T1.yearID <= 2016 AND
		Sum_P < Sum_non_P
GROUP BY	age;

SELECT 	HR/AB as Team_HR
FROM    Batting
WHERE 	teamID = 'TEX' AND
	yearID >= 2000 AND yearID <= 2016;
```
            
                
                
### FILTER THE PLAYER              

```sql
WITH R1 AS
(
SELECT 		playerID,
		yearID,
                SUM(G) AS Sum_non_P
FROM		Fielding
WHERE		POS NOT LIKE 'P'
GROUP BY	playerID,
		yearID
),
R2 AS
(SELECT	        playerID,
		yearID,
		SUM(G) AS Sum_P
FROM 	        Fielding
WHERE		POS = 'P'
GROUP BY	playerID,
		yearID
)
SELECT		R1.playerID,
		R1.yearID,
                Sum_P,
                Sum_non_P
FROM		R1
INNER JOIN	R2
ON		R1.playerID = R2.playerID AND
		R1.yearID = R2.yearID;
```
