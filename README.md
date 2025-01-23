# SQL Statements ;

## Question 3;

```sql
SELECT 
    CASE
        WHEN trip_distance <= 1 THEN 'Up to 1 mile'
        WHEN trip_distance > 1 AND trip_distance <= 3 THEN '1 to 3 miles'
        WHEN trip_distance > 3 AND trip_distance <= 7 THEN '3 to 7 miles'
        WHEN trip_distance > 7 AND trip_distance <= 10 THEN '7 to 10 miles'
        ELSE 'Over 10 miles'
    END AS distance_range,
    COUNT(*) AS trip_count
FROM green_taxi_data1
WHERE lpep_pickup_datetime >= '2019-10-01 00:00:00'
  AND lpep_pickup_datetime < '2019-11-01 00:00:00'
GROUP BY distance_range
ORDER BY trip_count DESC;
```

## Question 4;

```sql
SELECT 
    TO_CHAR(lpep_pickup_datetime, 'YYYY-MM-DD') AS pickup_day,
    MAX(trip_distance) AS longest_trip_distance
FROM 
    green_taxi_data1
GROUP BY 
    TO_CHAR(lpep_pickup_datetime, 'YYYY-MM-DD')
ORDER BY 
    longest_trip_distance DESC
LIMIT 1;
```

## Question 5;

SELECT 
    z."Borough", 
    z."Zone", 
    SUM(t.total_amount) AS total_amount
FROM 
    green_taxi_data1 t
JOIN 
    zones z ON t."PULocationID" = z."LocationID"
WHERE 
    TO_CHAR(t.lpep_pickup_datetime, 'YYYY-MM-DD') = '2019-10-18'
GROUP BY 
  z."Borough", 
  z."Zone"
HAVING 
    SUM(t.total_amount) > 13000
ORDER BY 
    total_amount DESC;
```


## Question 6;

```sql
SELECT 
    dz."Zone" AS dropoff_zone,
    MAX(t.tip_amount) AS max_tip
FROM 
    green_taxi_data1 t
JOIN 
    zones pz ON t."PULocationID" = pz."LocationID"
JOIN 
    zones dz ON t."DOLocationID" = dz."LocationID"
WHERE 
    TO_CHAR(t.lpep_pickup_datetime, 'YYYY-MM') = '2019-10'
    AND pz."Zone" = 'East Harlem North'
GROUP BY 
    dz."Zone"
ORDER BY 
    max_tip DESC
LIMIT 1;
```
