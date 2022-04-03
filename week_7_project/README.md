# **Project**

<br>


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



## **Data Architecture Design**

![](https://github.com/oussou-dev/Data_Engineer_ZoomCamp/blob/main/week_7_project/img/archi_design_def.png)


<br><br>

## **Cloud**

### **Provider**
- GCP

### **Steps**


> look snapshots folder [img/1_project_service_account](https://github.com/oussou-dev/Data_Engineer_ZoomCamp/blob/main/week_7_project/img/1_project_service_account/)

- Creating a Project : `Yelp-Zoomcamp-DE`
- Creating Service account and setting of Cloud SDK
  - Enable `service account API` first
  - Create new `servive account`
    - name
    - role : `editor`


  - Create `keys` for new service account
    - Select the new service account
    - Click on `KEYS` option
    - Create a `new key` with key type `JSON` and save it (`credentials` folder)
    - Configure service account with google cloud SDK:  
        run `gcloud auth activate-service-account  yelp-sa-rw@yelp-zoomcamp-de.iam.gserviceaccount.com  --key-file=/home/ubuntu/z_vscode_p/Data_Engineer_ZoomCamp/week_7_project/credentials/yelp-zoomcamp-de-99dcbf327104.json --project=yelp-zoomcamp-de`



  - Install and setting of Cloud SDK -> [link](https://cloud.google.com/sdk/docs/install?hl=en)

<br><br>

## **Data ingestion**

### **Batch / Workflow orchestration**

- Downloading Yelp Dataset (in folder `yelp_dataset`)  -> links:
  - [yelp_academic_dataset_business](https://www.yelp.com/dataset/download)
  - [yelp_academic_dataset_covid_features](https://www.dropbox.com/s/y6ac1lpu7ayezlj/yelp_academic_dataset_covid_features.json)
  - via CLI check nb of lines(go to `yelp_dataset` folder):  `cat yelp_academic_dataset_covid_features.json | wc -l`

<br>

> look snapshots folder [img/2_gcs_bucket](https://github.com/oussou-dev/Data_Engineer_ZoomCamp/blob/main/week_7_project/img/2_gcs_bucket/)


-  Creating Cloud bucket 
   -  via web interface :`Cloud Storage` -> `CREATE BUCKET` 

   -  via CLI : `gsutil  mb gs://gcs-bucket-yelp`

   -  list all buckets via CLI : `gsutil ls`
  

- and transferring data to Cloud bucket
  - via web interface : `gcs-bucket-yelp` -> `CREATE FOLDER` (name: data)-> `UPLOAD FILES` 

  - via CLI : (in `yelp_dataset` folder) `gsutil cp  yelp_academic_dataset_business.json gs://gcs-bucket-yelp/data/` or `gsutil cp *.json gs://gcs-bucket-yelp/data/`



- Creation of Dataflow Batch Job and Execution
  -  Check `scripts/dataflow_batch_test.py`: It reads JSON encoded messages from GCS file, transforms the message data and writes the results to BigQuery

  -  run dataflow batch job from CLI: (replace options with your own parameters)  
    `python3 scripts/dataflow_batch_test.py --input_path=gs://gcs-bucket-yelp/data/* --table=test.business --error_table=test.error --runner DataflowRunner --project yelp-zoomcamp-de --region us-west1 --service_account_email yelp-sa-rw@yelp-zoomcamp-de.iam.gserviceaccount.com --staging_location gs://gcs-bucket-yelp/dataflow/staging --temp_location gs://gcs-bucket-yelp/dataflow/temp --job_name test-batch-bq --num_workers 1 --max_num_workers 4`


<br>

### **Stream**

> look snapshots folder [img/3_stream](https://github.com/oussou-dev/Data_Engineer_ZoomCamp/blob/main/week_7_project/img/3_stream/)


- Creating PubSub topic subscription
  - Enable `Cloud Pub/Sub API` first
  - Create  `NEW TOPIC` in `Cloud Pub/Sub` via web interface
  - Complete `python3 scripts/publish_messages.py`
  - Get credentials, in CLI: `gloud config list`
  - Complete `config/publish_config.ini`

- and publishing messages (to the topic)
  - run `pip3 install --upgrade google-auth --upgrade protobuf`

  - run `python3 scripts/publish_messages.py --config_path=config/publish_config.ini`

  - check:
    - in CLI: `cat yelp_dataset/yelp_academic_dataset_covid_features.json | wc -l` => 209795
    - in web interface: `Cloud Pub/Sub` -> `gcp-topic-yelp` -> `SUBSCRIPTIONS` -> `gcp-topic-yelp-sub` -> `OVERVIEW` => Unacked message count = 209795
    - in web interface: `Cloud Pub/Sub` -> `gcp-topic-yelp` -> `SUBSCRIPTIONS` -> `gcp-topic-yelp-sub` -> `MESSAGES` click on `PULL` 


- Creation of dataflow stream jobs
  - run pip3 install apache-beam apache-beam[gcp] 

  -  Check `scripts/dataflow_stream.py`: script for reading JSON encoded messages from Pub/Sub, transforming the message data and writing the results to BigQuery

  -  Get credentials, in CLI: `gloud config list`

  -  Set credentials: export GOOGLE_APPLICATION_CREDENTIALS="/home/ubuntu/z_vscode_p/Data_Engineer_ZoomCamp/week_7_project/credentials/yelp-zoomcamp-de-99dcbf327104.json"

  -  run dataflow stream job from CLI:  (replace options with your own parameters)   
        `python3 scripts/dataflow_stream.py --input_subscription=projects/yelp-zoomcamp-de/subscriptions/gcp-topic-yelp-sub --output_table=test.yelp_covid --output_error_table=test.error --runner DataflowRunner --project yelp-zoomcamp-de --region us-west1 --service_account_email yelp-sa-rw@yelp-zoomcamp-de.iam.gserviceaccount.com --staging_location gs://gcs-bucket-yelp/dataflow/staging --temp_location gs://gcs-bucket-yelp/dataflow/temp --job_name test-stream-bq --num_workers 1 --max_num_workers 2`

<br><br>

## **Transformations**

- Creation of Google Cloud composer instance


<br><br>

## **Data warehouse**

- Querying and Visualisation of data in Google BigQuery


<br><br>

## **Dashboard**