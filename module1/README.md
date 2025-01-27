# Question 1
## commands:
- docker run -it test:pandas
- pip -V

## result:
pip 24.3.1 from /usr/local/lib/python3.12/site-packages/pip (python 3.12)


# Question 3
[Command line to start up postgress and pgAdmin](pgAdmin/pgAdmin_docker)

[Load data for green taxi trips](upload-nyt-data/upload-data-docker)

[Load taxi zones -- bottom of script](upload-nyt-data/upload-data.ipynb)

## Trip Segmentation Count
```sql
SELECT 1, count(1) FROM public.green_taxi_trips WHERE lpep_dropoff_datetime >= '2019-10-01' AND lpep_dropoff_datetime < '2019-11-01' 
AND trip_distance <= 1
UNION
SELECT 2, count(1) FROM public.green_taxi_trips WHERE lpep_dropoff_datetime >= '2019-10-01' AND lpep_dropoff_datetime < '2019-11-01' 
AND trip_distance > 1 and trip_distance <= 3
UNION
SELECT 3, count(1) FROM public.green_taxi_trips WHERE lpep_dropoff_datetime >= '2019-10-01' AND lpep_dropoff_datetime < '2019-11-01' 
AND trip_distance > 3 and trip_distance <= 7
UNION
SELECT 3, count(1) FROM public.green_taxi_trips WHERE lpep_dropoff_datetime >= '2019-10-01' AND lpep_dropoff_datetime < '2019-11-01' 
AND trip_distance > 7 and trip_distance <= 10
UNION
SELECT 4, count(1) FROM public.green_taxi_trips WHERE lpep_dropoff_datetime >= '2019-10-01' AND lpep_dropoff_datetime < '2019-11-01' 
AND trip_distance > 10
ORDER BY 1;
```

# Question 4
## Longest trip for each day
```sql
SELECT lpep_pickup_datetime::date, MAX(trip_distance) 
FROM public.green_taxi_trips 
GROUP BY 1 ORDER BY 2 DESC LIMIT 1; 
```

# Question 5
## Three biggest pickup zones
```sql
SELECT tz."Zone", sum(tt.total_amount) 
FROM public.green_taxi_trips tt, taxi_zones tz 
WHERE lpep_pickup_datetime >= '2019-10-18' 
AND lpep_pickup_datetime <= '2019-10-18 23:59:59' 
AND tt."PULocationID" = tz."LocationID" 
GROUP BY tz."Zone" 
HAVING sum(tt.total_amount) > 13000;
```

# Question 6
## Largest tip
```sql
SELECT max(tt.tip_amount), dtz."Zone" 
FROM public.green_taxi_trips tt, taxi_zones tz, taxi_zones dtz 
WHERE EXTRACT(year FROM lpep_pickup_datetime) = 2019 
AND EXTRACT(month FROM lpep_pickup_datetime) = 10 
AND tz."LocationID" = tt."PULocationID" 
AND tz."Zone" = 'East Harlem North' 
AND dtz."LocationID" = tt."DOLocationID" 
GROUP BY dtz."Zone" 
ORDER BY max(tt.tip_amount) DESC LIMIT 1;
```



