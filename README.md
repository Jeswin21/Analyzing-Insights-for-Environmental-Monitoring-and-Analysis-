# Analyzing-Insights-for-Environmental-Monitoring-and-Analysis-

Description: This project uses SQL to analyze a real-world database, how to extract the most useful information from the dataset, how to pre-process the data using Python for improved performance, and how to use a structured query language to retrieve useful information from the database.

# Module 1 Data Pre-Processing:
Data Pre-processing is one of the important steps in data analytics because data that is not processed can lead to different unwanted results when the data will be used for further applications. This task includes sub-tasks such as handling null values, deletion or transformation of irrelevant values, datatype transformation, removing duplicates, etc. The tasks to be performed for cleaning the data set are given below:

Reading the Data from CSV

Renaming the columns

Checking for null values

Removing Duplicates

Handling missing values

Data type conversion

Exporting the cleaned dataset

Generate tables using the cleaned dataset

File: Analyzing insights for environmental monitoring and analysis.ipynb

Uncleaned Dataset: iot_telemetry_data.csv
Cleaned dataset: Cleaned environment.csv

# Module 2 Data Analysis using SQL:

Data analysis is performed on the pre-processed data from the previous module and Data Analysis is conducted using SQL

Task 1: Write an SQL query to solve the given problem statement.
Find the average temperature recorded for each device.

Ans: SELECT device_id, AVG(temperature) 
FROM cleaned_environment
GROUP BY device_id;

Task 2: Write an SQL query to solve the given problem statement.
Calculate the average temperature recorded in the cleaned_environment table.

Ans: SELECT AVG(temperature) AS average_temperature
FROM cleaned_environment;

Task 3: Write an SQL query to solve the given problem statement.
Find the timestamp and temperature of the highest recorded temperature for each device.

Ans: SELECT device_id, timestamp, MAX(temperature) 
FROM cleaned_environment
GROUP BY device_id;

Task 4: Write an SQL query to solve the given problem statement.
Identify devices where the temperature has increased from the minimum recorded temperature to the maximum recorded temperature

Ans: SELECT device_id
FROM (
  SELECT device_id, MIN(temperature) AS min_temperature, MAX(temperature) AS max_temperature
  FROM cleaned_environment
  GROUP BY device_id
) AS temperature_ranges
WHERE max_temperature > min_temperature;

Task 5: Write an SQL query to solve the given problem statement.
Calculate the exponential moving average of temperature for each device limit to 10 devices.

Ans: WITH ema_calculation AS (
SELECT device_id, timestamp, temperature,
AVG(temperature) OVER (PARTITION BY device_id
ORDER BY timestamp ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS ema_temperature,
ROW_NUMBER() OVER (PARTITION BY device_id ORDER BY timestamp) AS row_num
FROM cleaned_environment
)
SELECT device_id, timestamp, temperature, ema_temperature
FROM ema_calculation 
limit 10;

Task 6: Write an SQL query to solve the given problem statement.
Find the timestamps and devices where the carbon monoxide level exceeds the average carbon monoxide level of all devices.

Ans: SELECT timestamp, device_id
FROM cleaned_environment
WHERE carbon_monoxide > (
    SELECT AVG(carbon_monoxide)
    FROM cleaned_environment )

Task 7: Write an SQL query to solve the given problem statement.
Retrieve the devices with the highest average temperature recorded.

Ans: SELECT device_id, AVG(temperature) as average_temperature
FROM cleaned_environment
GROUP BY device_id;

Task 8: Write an SQL query to solve the given problem statement.
Calculate the average temperature for each hour of the day across all devices.

Ans: SELECT EXTRACT(HOUR FROM timestamp) AS hour_of_day, AVG(temperature) AS average_temperature
FROM cleaned_environment
GROUP BY EXTRACT(HOUR FROM timestamp)
ORDER BY hour_of_day

Task 9: Write an SQL query to solve the given problem statement.
Which device(s) in the cleaned environment dataset has recorded only a single distinct temperature value?

Ans: SELECT device_id
FROM cleaned_environment
GROUP BY device_id
HAVING COUNT(DISTINCT temperature) = 1

Task 10: Write an SQL query to solve the given problem statement.
Calculate the average temperature for each device, excluding outliers (temperatures beyond 3 standard deviations).

Ans: SELECT device_id, AVG(temperature) AS avg_temperature
FROM cleaned_environment
WHERE temperature > (
    SELECT AVG(temperature) - (3 * STDDEV(temperature))
    FROM cleaned_environment
)
GROUP BY device_id;

Task 11: Write an SQL query to solve the given problem statement.
Retrieve the devices that have experienced a sudden change in humidity (greater than 50% difference) within a 30-minute window.

Ans: SELECT device_id,timestamp, humidity
FROM (
SELECT device_id, humidity, timestamp,
LAG(humidity) OVER (PARTITION BY device_id ORDER BY timestamp) AS prev_humidity,
LAG(timestamp) OVER (PARTITION BY device_id ORDER BY timestamp) AS prev_timestamp
FROM cleaned_environment
) AS subquery
WHERE ABS(humidity - prev_humidity) > 0.5
AND TIMESTAMPDIFF(SECOND, prev_timestamp, timestamp) <= 1800

Task 12: Write an SQL query to solve the given problem statement.
Find the average temperature for each device during weekdays and weekends separately.

Ans: SELECT
  device_id,
  CASE
    WHEN timestamp = 1 or 7 THEN 'Weekday'
    ELSE 'Weekend'
  END AS day_type,
  AVG(temperature) AS average_temperature
FROM
  cleaned_environment
GROUP BY
  device_id;

Task 13: Write an SQL query to solve the given problem statement.
Calculate the cumulative sum of temperature for each device, ordered by timestamp limit to 10.

Ans: SELECT
  device_id,
  timestamp,
  temperature,
  SUM(temperature) OVER (PARTITION BY device_id ORDER BY timestamp) AS cumulative_temperature
FROM
  cleaned_environment
LIMIT 10;

 
    



