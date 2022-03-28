# **Project**

## **Problem description** 



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


<br>

### **Stream**

~~~
(look snapshots folders img/3_stream)
~~~


- Creating PubSub topic subscription
  - Enable `Cloud Pub/Sub API` first
  - Create  `NEW TOPIC` in `Cloud Pub/Sub` via web interface
  - Complete and run `python3 scripts/publish_messages.py`
  - 


- and publishing messages




<br><br>
## **Transformations**



## **Data warehouse**


## **Dashboard**