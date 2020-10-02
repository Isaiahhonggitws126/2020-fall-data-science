
# SQL:  Structured Query Language  Exercise

### Getting Started
1. Go to BigQuery UI https://console.cloud.google.com/bigquery
2. Add in the public data sets.
	3. Click the Add Data icon
	4. Add any dataset
	5. `bigquery-public-data` should become visible and populate in the BigQuery UI.
3. Add your queries where it says [YOUR QUERY HERE].
4. Make sure you add your query in between the triple tick marks.
---

For this section of the exercise we will be using the `bigquery-public-data.austin_311.311_service_requests`  table.

5. Write a query that tells us how many rows are in the table.
	```
	[YOUR QUERY HERE]
	SELECT
		count(*)
	FROM
		`bigquery-public-data.austin_311.311_service_requests`
	```

7. Write a query that tells us how many _distinct_ values there are in the complaint_description column.
	```
	[YOUR QUERY HERE]
	SELECT
		COUNT(DISTINCT complaint_description)
	FROM
		`bigquery-public-data.austin_311.311_service_requests`
	```

8. Write a query that counts how many times each owning_department appears in the table and orders them from highest to lowest.
	```
	[YOUR QUERY HERE]
	SELECT
	  owning_department,
	  COUNT(*) as T
	FROM
	  `bigquery-public-data.austin_311.311_service_requests`
	GROUP BY
	  owning_department
	ORDER BY
	  T DESC

	```

9. Write a query that lists the top 5 complaint_description that appear most and the amount of times they appear in this table. (hint... limit)
	```
	[YOUR QUERY HERE]
	SELECT
	  complaint_description,
	  COUNT(*) as T
	FROM
	  `bigquery-public-data.austin_311.311_service_requests`
	GROUP BY
	  complaint_description
	ORDER BY
	  T DESC
	  LIMIT 5
	  ```
10. Write a query that lists and counts all the complaint_description, just for the where the owning_department is 'Animal Services Office'.
	```
	[YOUR QUERY HERE]
	SELECT
	  COUNT(complaint_description) as T,
	  owning_department
	FROM
	  `bigquery-public-data.austin_311.311_service_requests`
	WHERE
	  owning_department = 'Animal Services Office'
	GROUP BY
	  owning_department

	```

11. Write a query to check if there are any duplicate values in the unique_key column (hint.. There are two was to do this, one is to use a temporary table for the groupby, then filter for values that have more than one count, or, using just one table but including the  `having` function).
	```
	[YOUR QUERY HERE]
	WITH
	  T AS (
	  SELECT
	    COUNT(unique_key) AS key
	  FROM
	    `bigquery-public-data.austin_311.311_service_requests`
	  GROUP BY
	    unique_key )
	SELECT
	  *
	FROM
	  T
	WHERE
	  key > 1

	```


### For the next question, use the `census_bureau_usa` tables.

1. Write a query that returns each zipcode and their population for 2000 and 2010.
	```
	[YOUR QUERY HERE]

		WITH
	  TABLE_2000 AS(
	  SELECT
	    zipcode,
	    population AS population_2000
	  FROM
	    `bigquery-public-data.census_bureau_usa.population_by_zip_2000` ),
	  TABLE_2010 AS(
	  SELECT
	    zipcode,
	    population AS population_2010
	  FROM
	    `bigquery-public-data.census_bureau_usa.population_by_zip_2010` )
		SELECT
		  A.zipcode,
		  A.population_2000,
		  B.population_2010
		FROM
		  TABLE_2000 AS A
		JOIN
		  TABLE_2010 AS B
		ON
		  A.zipcode = B.zipcode

	```

### For the next section, use the  `bigquery-public-data.google_political_ads.advertiser_weekly_spend` table.
1. Using the `advertiser_weekly_spend` table, write a query that finds the advertiser_name that spent the most in usd.
	```
	[YOUR QUERY HERE]
	SELECT
  advertiser_name,
  MAX(spend_usd) AS USD
	FROM
	  `bigquery-public-data.google_political_ads.advertiser_weekly_spend`
	GROUP BY
	  advertiser_name
	ORDER BY
  USD DESC

	```
2. Who was the 6th highest spender? (No need to insert query here, just type in the answer.)
	```
	[YOUR ANSWER HERE]
	DSCC

	```

3. What week_start_date had the highest spend? (No need to insert query here, just type in the answer.)
	```
	[YOUR ANSWER HERE]
	2020-02-23

	```

4. Using the `advertiser_weekly_spend` table, write a query that returns the sum of spend by week (using week_start_date) in usd for the month of August only.
	```
	[YOUR QUERY HERE]
	SELECT
  week_start_date,
  SUM(spend_usd) AS USD
	FROM
	  `bigquery-public-data.google_political_ads.advertiser_weekly_spend`
	WHERE
	  CAST(week_start_date AS STRING) LIKE '%2020-08%'
	GROUP BY
	  week_start_date

	```
6.  How many ads did the 'TOM STEYER 2020' campaign run? (No need to insert query here, just type in the answer.)
	```
	[YOUR ANSWER HERE]
	50

	```
7. Write a query that has, in the US region only, the total spend in usd for each advertiser_name and how many ads they ran. (Hint, you're going to have to join tables for this one).
	```
		[YOUR QUERY HERE]
		SELECT
  advertiser_name,
  SUM(spend_usd) AS TOTAL_SPENT,
  COUNT(advertiser_id) AS TOTAL_ADS
	FROM
	  `bigquery-public-data.google_political_ads.advertiser_weekly_spend`
	WHERE
	  election_cycle LIKE '%US%'
	GROUP BY
	  advertiser_name

	```
8. For each advertiser_name, find the average spend per ad.
	```
	[YOUR QUERY HERE]
	SELECT
  advertiser_name,
  AVG(spend_usd) AS AVERAGE_SPENT
	FROM
	  `bigquery-public-data.google_political_ads.advertiser_weekly_spend`
	GROUP BY
	  advertiser_name

	```
10. Which advertiser_name had the lowest average spend per ad that was at least above 0.
	```
	[YOUR QUERY HERE]
	SELECT
  advertiser_name,
  AVG(spend_usd) AS AVERAGE_SPENT
	FROM
	  `bigquery-public-data.google_political_ads.advertiser_weekly_spend`
	WHERE
	  spend_usd > 0
	GROUP BY
	  advertiser_name
	ORDER BY
	  AVERAGE_SPENT ASC

	```
## For this next section, use the `new_york_citibike` datasets.

1. Who went on more bike trips, Males or Females?
	```
	[YOUR QUERY HERE]
	SELECT
  gender,
  COUNT(tripduration) AS BIKE_TRIPS
	FROM
	  `bigquery-public-data.new_york_citibike.citibike_trips`
	GROUP BY
	  gender

	male 35611787

	```
2. What was the average, shortest, and longest bike trip taken in minutes?
	```
	[YOUR QUERY HERE]
	SELECT
  MIN(SHORTEST_TRIPS) AS AVG_SHORT_TRIP,
  MAX(LONGEST_TRIPS) AS AVG_LONG_TRIP
	FROM (
	  SELECT
	    bikeid,
	    AVG(tripduration) AS SHORTEST_TRIPS,
	    AVG(tripduration) AS LONGEST_TRIPS
	  FROM
	    `bigquery-public-data.new_york_citibike.citibike_trips`
	  GROUP BY
	    bikeid)

	```

3. Write a query that, for every station_name, has the amount of trips that started there and the amount of trips that ended there. (Hint, use two temporary tables, one that counts the amount of starts, the other that counts the number of ends, and then join the two.)
	```
	[YOUR QUERY HERE]

	WITH starting AS(
  SELECT bikeid, start_station_name, count(start_station_id) AS START_TRIPS
  FROM `bigquery-public-data.new_york_citibike.citibike_trips`
  GROUP BY start_station_name, bikeid
  ),
	ending AS (
  SELECT bikeid, end_station_name, count(end_station_id) AS END_TRIPS
  FROM `bigquery-public-data.new_york_citibike.citibike_trips`
  GROUP BY end_station_name, bikeid
  ) SELECT A.START_TRIPS, B.END_TRIPS FROM starting AS A JOIN ending AS B ON A.bikeid = B.bikeid

	```
# The next section is the Google Colab section.  
1. Open up this [this Colab notebook](https://colab.research.google.com/drive/1kHdTtuHTPEaMH32GotVum41YVdeyzQ74?usp=sharing).
2. Save a copy of it in your drive.
3. Rename your saved version with your initials.
4. Click the 'Share' button on the top right.  
5. Change the permissions so anyone with link can view.
6. Copy the link and paste it right below this line.
	* YOUR LINK:  ________________________________
9. Complete the two questions in the colab notebook file.
