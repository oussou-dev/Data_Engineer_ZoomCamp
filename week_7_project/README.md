# **Project**

## **Problem description** 

### **Intro**

Yelp is a local business directory service and review site with social networking features.  
It allows users to give ratings and review businesses.  
The review is usually short text consisting of few lines with about hundred words.  
Often, a review describes various dimensions about a business and the experience of user with respect to those dimensions.  
This dataset is a subset of Yelp's businesses, reviews, and user data.  
It was originally put together for the Yelp Dataset Challenge which is a chance for students to conduct research or analysis on Yelp's data and share their discoveries.  
In the dataset you'll find information about businesses across 11 metropolitan areas in four countries.


### **Glossary of terms**

- Yelp: A website which publishes crowd source reviews to help users and business owners . Business: A local body listed on yelp like Restaurants, Department Stores, Bars, Home-Local Services, Cafes, Automotive.
    
- Existing business owner: A person who has listed his business on Yelp site and getting views from yelp users.
    
- Future business owner: A person who wants to start new business in future time
    
- User: A person who has registered on yelp who is writing reviews about different business after vising them or a person who is using yelp reviews to choose business.

- Analytics: Extract knowledge out of data which can be used by system users to make important decisions which is very difficult just by looking at the data.

- Review: It is text written by user after vising business about the over-all experience. I is also a numeric representation (out of 5) to compare it with other business.


### **Goals**
- Yelp dataset File: connected to Google SDK or GcsFuse for transfer of data to Google cloud storage 

- Yelp dataset JSON streams are published to Google PubSub which is used for real-time ingestion or streaming datasets

<br><br>
## **Cloud**

### **Provider**
- GCP

### **Steps**

~~~
(look snapshots folders img/1_project_service_account)
~~~
- Creating a Project : `Yelp-Zoomcamp-DE`
- Creating Service account and setting of Cloud SDK
  - Enable `service account API` first
  - Create new `servive account`
    - name
    - role : `editor`
  - Create `keys` for new service account
    - Select the new service account
    - Click on `KEYS` option
    - Create a `new key` with key type `JSON` and save it 
  - Install and setting of Cloud SDK -> [link](https://cloud.google.com/sdk/docs/install?hl=en)

<br><br>

## **Data ingestion**

### **Batch / Workflow orchestration**

- Downloading Yelp Dataset -> [link](https://www.yelp.com/dataset/download)
  

~~~
(look snapshots folders img/2_gcs_bucket)
~~~

-  Creating Cloud bucket 
   -  via web interface :`Cloud Storage` -> `CREATE BUCKET` 
   -  via CLI : `gsutil  mb gs://gcs-bucket-yelp`
   -  list all buckets via CLI : `gsutil ls`
  

- and transferring data to Cloud bucket
  - via web interface : `gcs-bucket-yelp` -> `CREATE FOLDER` (name: data)-> `UPLOAD FILES` 
  - via CLI : (in `yelp_dataset` folder) `gsutil cp  yelp_academic_dataset_business.json gs://gcs-bucket-yelp/data/`



- Creation of Dataflow Batch Job and Execution
  - 


<br>

### **Stream**

~~~
(look snapshots folders img/3_stream)
~~~


- Creating PubSub topic subscription
  - Enable `Cloud Pub/Sub API` first
  - Create  `NEW TOPIC` in `Cloud Pub/Sub` via web interface
  - Complete `python3 scripts/publish_messages.py`
  - Complete `config/publish_config.ini`
  - 


- and publishing messages (to the topic)
  - run `python3 scripts/publish_messages.py --config_path=config/publish_config.ini`


- Creation of dataflow stream jobs
  -  script for reading JSON encoded messages from Pub/Sub, transforming the message data and writing the results to BigQuery: `scripts/dataflow_stream.py` 


<br><br>
## **Transformations**



## **Data warehouse**


## **Dashboard**