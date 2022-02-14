## Homework
[Form](https://forms.gle/ytzVYUh2RptgkvF79)  
We will use all the knowledge learned in this week. Please answer your questions via form above.  
**Deadline** for the homework is 14th Feb 2022 17:00 CET.

<br><br>


### Question 1: 
**What is count for fhv vehicles data for year 2019**  
Can load the data for cloud storage and run a count(*)

> **answer**
```sql
SELECT count(*)
FROM `dtc-de-course-339214.trips_data_all.fhv_tripdata`
WHERE DATE(pickup_datetime) BETWEEN "2019-01-01" AND "2019-12-31";
```
>->> **42084899**

<br><br>


### Question 2: 
**How many distinct dispatching_base_num we have in fhv for 2019**  
Can run a distinct query on the table from question 1

> **answer**
```sql
SELECT count(DISTINCT(dispatching_base_num))
FROM `dtc-de-course-339214.trips_data_all.fhv_tripdata`
WHERE DATE(pickup_datetime) BETWEEN "2019-01-01" AND "2019-12-31";
```
>->> **792**

<br><br>

### Question 3: 
**Best strategy to optimise if query always filter by dropoff_datetime and order by dispatching_base_num**  
Review partitioning and clustering video.   
We need to think what will be the most optimal strategy to improve query 
performance and reduce cost.

> **answer**  

>->> **Partition by dropoff_datetime**   
>->> **Cluster by dispatching_base_num**  

<br><br>

### Question 4: 
**What is the count, estimated and actual data processed for query which counts trip between 2019/01/01 and 2019/03/31 for dispatching_base_num B00987, B02060, B02279**  
Create a table with optimized clustering and partitioning, and run a 
count(*). Estimated data processed can be found in top right corner and
actual data processed can be found after the query is executed.

> **answer**
```sql
CREATE OR REPLACE TABLE `dtc-de-course-339214.trips_data_all.fhv_tripdata_clustered`
PARTITION BY DATE(pickup_datetime)
CLUSTER BY dispatching_base_num AS
SELECT * FROM `dtc-de-course-339214.trips_data_all.fhv_tripdata_external_table`;

SELECT count(*)
FROM `dtc-de-course-339214.trips_data_all.fhv_tripdata_clustered`
WHERE DATE(pickup_datetime) BETWEEN "2019-01-01" AND "2019-03-31"
AND dispatching_base_num IN ('B00987','B02060','B02279');
```
>->> Count:          **~ 26658**  
>->> Estimated data: **~ 400MB**  
>->> Processed data: **~ 155MB**  


### Question 5: 
**What will be the best partitioning or clustering strategy when filtering on dispatching_base_num and SR_Flag**  
Review partitioning and clustering video. 
Partitioning cannot be created on all data types.

> **answer**  

>->> **Partition by SR_Flag and cluster by dispatching_base_num**


### Question 6: 
**What improvements can be seen by partitioning and clustering for data size less than 1 GB**  
Partitioning and clustering also creates extra metadata.  
Before query execution this metadata needs to be processed.


> **answers**  

>->> **No improvements**  
>->> **Can be worse due to metadata**



### (Not required) Question 7: 
**In which format does BigQuery save data**  
Review big query internals video.

> **answer**   

>->> **Columnar**