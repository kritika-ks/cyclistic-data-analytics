/* Filtering the data 
1- to extract data from the exact time frame(2022-07-01 to 2023-06-30), and
2- to extract the only columns that are necessary for the analysis.
*/

# Table creation with data
Below is the command to create a table and adding filtered data from cyclistic database with the exact timeframe.
```sql
CREATE TABLE Filtered_Cyclistic_Data AS (
SELECT ride_id, rideable_type, started_at, ended_at, member_casual
FROM public."Cyclistic_Trip_Data" 
WHERE started_at > '2022-06-30 23:59:59' AND ended_at < '2023-07-01 00:00:00'
ORDER BY started_at ASC 
	)
```
	
/* *Cleaning the data* 
1- eliminate errors & nulls, 
2- trim data for leading and trailing spaces
3- remove duplicates
4- assign the columns with their appropriate data types
*/

CREATE TABLE cleaned_cyclistic_data AS (
SELECT DISTINCT(COALESCE(TRIM(CAST(ride_id AS VARCHAR(16))))) AS ride_id, COALESCE(TRIM(CAST(rideable_type AS VARCHAR (13)))) AS rideable_type, started_at, ended_at, COALESCE(TRIM(CAST(member_casual AS VARCHAR (6)))) AS member_casual
FROM "filtered_cyclistic_data"
WHERE (ended_at > started_at)
ORDER BY started_at ASC
	)
	
/* Organising data
1- extracting the start time, end time, start day, end day, start month and end month of the trip.
2- calcucating the actual duration of the trip in minutes.
*/

CREATE TABLE organized_cyclistic_data AS (
SELECT ride_id,
member_casual AS Membership_Type,
rideable_type AS Bike_Type,
started_at AS Start_Timestamp,
cast("started_at" as time) AS start_time,
To_Char("started_at", 'Day') AS start_day,
To_Char("started_at", 'Month') AS start_month,
ended_at AS End_Timestamp,
cast("ended_at" as time) AS end_time,
To_Char("ended_at", 'Day') AS end_day,
To_Char("ended_at", 'Month') AS end_month,
ROUND(((EXTRACT(EPOCH FROM (ended_at - started_at) ))/60), 2) AS duration_in_min
FROM "cleaned_cyclistic_data"
	)




