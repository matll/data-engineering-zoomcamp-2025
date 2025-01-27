# data-engineering-zoomcamp-2025
# Question 1. Version of pip
  sudo docker run --entrypoint bash -it python:3.12.8
  pip --version
Question 2. Understanding Docker networking and docker-compose
	hostname is the service name
	host port is the first one in ports:'5433:5432'
# Question 3. Trip Segmentation Count
  SELECT
      COUNT(*)
  FROM 
      public.green_taxi_data
  WHERE
  	trip_distance <= 1

SELECT
    COUNT(*)
FROM 
    public.green_taxi_data
WHERE
	trip_distance > 1 AND trip_distance <= 3

SELECT
    COUNT(*)
FROM 
    public.green_taxi_data
WHERE
	trip_distance > 3 AND trip_distance <= 7

SELECT
    COUNT(*)
FROM 
    public.green_taxi_data
WHERE
	trip_distance > 7 AND trip_distance <= 10

SELECT
    COUNT(*)
FROM 
    public.green_taxi_data
WHERE
	trip_distance > 10

# Question 4. Longest trip for each day
SELECT
	  MAX(trip_distance),
    lpep_pickup_datetime::date
FROM 
    public.green_taxi_data
WHERE lpep_pickup_datetime::date IN (
		'2019-10-11',
		'2019-10-24',
		'2019-10-26',
		'2019-10-31')
GROUP BY
	lpep_pickup_datetime::date

# Question 5. Three biggest pickup zones
SELECT
	  z."Zone",
	  SUM(total_amount),
    lpep_pickup_datetime::date
FROM 
    public.green_taxi_data g
JOIN
	public.zones z ON g."PULocationID" = z."LocationID"
WHERE lpep_pickup_datetime::date = '2019-10-18'
GROUP BY z."Zone", lpep_pickup_datetime::date
HAVING SUM(total_amount) > 13000
ORDER BY SUM(total_amount) DESC

# Question 6. Largest tip
SELECT
	zpu."Zone" AS "Pickup zone",
	zdo."Zone" AS "Drop off zone",
	MAX(tip_amount) as "Max tip",
	date_part('month', lpep_pickup_datetime) AS "Month",
	date_part('year', lpep_pickup_datetime) AS "Year"
FROM 
    public.green_taxi_data g
JOIN
	public.zones zpu ON g."PULocationID" = zpu."LocationID"
JOIN
    zones zdo ON g."DOLocationID" = zdo."LocationID"
WHERE date_part('month', lpep_pickup_datetime) = '10'
	AND date_part('year', lpep_pickup_datetime) = '2019'
	AND zpu."Zone" = 'East Harlem North'
GROUP BY "Pickup zone", "Drop off zone", "Month", "Year"
ORDER BY "Max tip" DESC

# Question 7. Terraform Workflow
terraform init
terraform apply -auto-approve
terraform destroy

  
