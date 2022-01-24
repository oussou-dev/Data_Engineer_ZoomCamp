## Week 1 Homework

In this homework we'll prepare the environment 
and practice with terraform and SQL

## Question 1. Google Cloud SDK

Install Google Cloud SDK. What's the version you have? 

To get the version, run `gcloud --version`

> **answer**  
`Google Cloud SDK 369.0.0`  
`alpha 2022.01.14`  
`app-engine-python 1.9.98`  
`beta 2022.01.14`
`bq 2.0.72`
`core 2022.01.14`
`gsutil 5.6`


## Google Cloud account 

Create an account in Google Cloud and create a project.


## Question 2. Terraform 

Now install terraform and go to the terraform directory (`week_1_basics_n_setup/1_terraform_gcp/terraform`)

After that, run

* `terraform init`
* `terraform plan`
* `terraform apply` 

Apply the plan and copy the output (after running `apply`) to the form.

It should be the entire output - from the moment you typed `terraform init` to the very end.

> **answer**  
`
Initializing the backend...`

`Successfully configured the backend "local"! Terraform will automatically
use this backend unless the backend configuration changes.`

`Initializing provider plugins...`  
`- Finding latest version of hashicorp/google...`  
`- Installing hashicorp/google v4.7.0...`  
`- Installed hashicorp/google v4.7.0 (signed by HashiCorp)`

`Terraform has created a lock file .terraform.lock.hcl to record the provider
selections it made above. Include this file in your version control repository
so that Terraform can guarantee to make the same selections by default when
you run "terraform init" in the future.`

`Terraform has been successfully initialized!`

`You may now begin working with Terraform. Try running "terraform plan" to see
any changes that are required for your infrastructure. All Terraform commands
should now work.`

`If you ever set or change modules or backend configuration for Terraform,
rerun this command to reinitialize your working directory. If you forget, other
commands will detect it and remind you to do so if necessary.`

`var.project  
  Your GCP Project ID`  
`
  Enter a value: dtc-de-course-339214   
`

`Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:`
  `+ create`

`Terraform will perform the following actions:`

 ` #google_bigquery_dataset.dataset will be created`
  + resource "google_bigquery_dataset" "dataset" {
      + creation_time              = (known after apply)
      + dataset_id                 = "trips_data_all"
      + delete_contents_on_destroy = false
      + etag                       = (known after apply)
      + id                         = (known after apply)
      + last_modified_time         = (known after apply)
      + location                   = "europe-west6"
      + project                    = "dtc-de-course-339214"
      + self_link                  = (known after apply)

      + access {
          + domain         = (known after apply)
          + group_by_email = (known after apply)
          + role           = (known after apply)
          + special_group  = (known after apply)
          + user_by_email  = (known after apply)

          + view {
              + dataset_id = (known after apply)
              + project_id = (known after apply)
              + table_id   = (known after apply)
            }
        }
    }

  `#google_storage_bucket.data-lake-bucket will be created`
  + resource "google_storage_bucket" "data-lake-bucket" {
      + force_destroy               = true
      + id                          = (known after apply)
      + location                    = "EUROPE-WEST6"
      + name                        = "dtc_data_lake_dtc-de-course-339214"
      + project                     = (known after apply)
      + self_link                   = (known after apply)
      + storage_class               = "STANDARD"
      + uniform_bucket_level_access = true
      + url                         = (known after apply)

      + lifecycle_rule {
          + action {
              + type = "Delete"
            }

          + condition {
              + age                   = 30
              + matches_storage_class = []
              + with_state            = (known after apply)
            }
        }

      + versioning {
          + enabled = true
        }
    }

`Plan: 2 to add, 0 to change, 0 to destroy.
`
─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────

`Note: You didn't use the -out option to save this plan, so Terraform can't guarantee to take exactly these actions if you run "terraform apply" now.`
`vscode ➜ .../Data_Engineer_ZoomCamp/week_1_basics_n_setup/1_terraform_gcp/terraform (main ✗) $ terraform apply`  
`var.project      
  Your GCP Project ID`

  `Enter a value: dtc-de-course-339214 `


`Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:`
  + create

`Terraform will perform the following actions:`

  `#google_bigquery_dataset.dataset will be created`
  + resource "google_bigquery_dataset" "dataset" {
      + creation_time              = (known after apply)
      + dataset_id                 = "trips_data_all"
      + delete_contents_on_destroy = false
      + etag                       = (known after apply)
      + id                         = (known after apply)
      + last_modified_time         = (known after apply)
      + location                   = "europe-west6"
      + project                    = "dtc-de-course-339214"
      + self_link                  = (known after apply)

      + access {
          + domain         = (known after apply)
          + group_by_email = (known after apply)
          + role           = (known after apply)
          + special_group  = (known after apply)
          + user_by_email  = (known after apply)

          + view {
              + dataset_id = (known after apply)
              + project_id = (known after apply)
              + table_id   = (known after apply)
            }
        }
    }

  `#google_storage_bucket.data-lake-bucket will be created`
  + resource "google_storage_bucket" "data-lake-bucket" {
      + force_destroy               = true
      + id                          = (known after apply)
      + location                    = "EUROPE-WEST6"
      + name                        = "dtc_data_lake_dtc-de-course-339214"
      + project                     = (known after apply)
      + self_link                   = (known after apply)
      + storage_class               = "STANDARD"
      + uniform_bucket_level_access = true
      + url                         = (known after apply)

      + lifecycle_rule {
          + action {
              + type = "Delete"
            }

          + condition {
              + age                   = 30
              + matches_storage_class = []
              + with_state            = (known after apply)
            }
        }

      + versioning {
          + enabled = true
        }
    }

`Plan: 2 to add, 0 to change, 0 to destroy.`

`Do you want to perform these actions?
  Terraform will perform the actions described above.`
  `Only 'yes' will be accepted to approve.`

  `Enter a value: yes`

`google_bigquery_dataset.dataset: Creating...
google_storage_bucket.data-lake-bucket: Creating...
google_bigquery_dataset.dataset: Creation complete after 2s [id=projects/dtc-de-course-339214/datasets/trips_data_all]
google_storage_bucket.data-lake-bucket: Creation complete after 2s [id=dtc_data_lake_dtc-de-course-339214]`



## Prepare Postgres 

Run Postgres and load data as shown in the videos

> **CMD**  
docker-compose up


We'll use the yellow taxi trips from January 2021:

```bash
wget https://s3.amazonaws.com/nyc-tlc/trip+data/yellow_tripdata_2021-01.csv
```

You will also need the dataset with zones:

```bash 
wget https://s3.amazonaws.com/nyc-tlc/misc/taxi+_zone_lookup.csv
```

Download this data and put it to Postgres


## Question 3. Count records 

How many taxi trips were there on January 15?

Consider only trips that started on January 15.

> **QUERY**  
```sql
SELECT COUNT(*)
FROM trips
WHERE (tpep_pickup_datetime>='2021-01-15 00:00:00' 
AND tpep_pickup_datetime<'2021-01-16 00:00:00');
```  
> **53024**


## Question 4. Largest tip for each day

Find the largest tip for each day. 
On which day it was the largest tip in January?

Use the pick up time for your calculations.

(note: it's not a typo, it's "tip", not "trip")


```sql  
SELECT CAST(tpep_pickup_datetime AS DATE) AS day, MAX(tip_amount) AS max_tip
FROM trips
GROUP BY  day
ORDER BY max_tip DESC;
```
> **2021-01-20**


## Question 5. Most popular destination

What was the most popular destination for passengers picked up 
in central park on January 14?

Use the pick up time for your calculations.

Enter the zone name (not id). If the zone name is unknown (missing), write "Unknown" 

```sql
SELECT CONCAT(zpu."Borough", '/', zpu."Zone") AS pickup_loc,
       CONCAT(zdo."Borough", '/', zdo."Zone") AS dropoff_loc,
       COUNT(*) AS amount_of_trips
FROM trips t 
JOIN zones zpu ON t."PULocationID" = zpu."LocationID"
JOIN zones zdo ON t."DOLocationID" = zdo."LocationID"
WHERE t."PULocationID" = ( SELECT "LocationID"
		                   FROM zones zpu
		                   WHERE zpu."Zone" IN ('Central Park')
                        )
GROUP BY	pickup_loc, dropoff_loc 
ORDER BY	amount_of_trips DESC
;
```
> **Upper East Side North**



## Question 6. Most expensive locations

What's the pickup-dropoff pair with the largest 
average price for a ride (calculated based on `total_amount`)?

Enter two zone names separated by a slash

For example:

"Jamaica Bay / Clinton East"

If any of the zone names are unknown (missing), write "Unknown". For example, "Unknown / Clinton East". 

```sql
SELECT CONCAT(zpu."Zone", '/', zdo."Zone") AS zone_pair,
       AVG(t."total_amount") AS total_amount_average
FROM trips t 
JOIN zones zpu ON t."PULocationID" = zpu."LocationID"
JOIN zones zdo ON t."DOLocationID" = zdo."LocationID"
GROUP BY zone_pair
ORDER BY total_amount_average DESC
;
```
> **Alphabet City/Unknown**



## Submitting the solutions

* Form for submitting: https://forms.gle/yGQrkgRdVbiFs8Vd7
* You can submit your homework multiple times. In this case, only the last submission will be used. 

Deadline: 26 January (Wednesday), 22:00 CET

