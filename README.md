# Project 4: Create a Data Lake with Spark

## Summary
* [Introduction](#Introduction)
* [Partition parquet files](#Partition-parquet-files)
* [ETL process](#ETL-process)
* [Project structure](#Project-structure)
* [How to run](#How-to-run)


--------------------------------------------

### Introduction


A music streaming startup, Sparkify, has grown their user base and song database and want to move their processes and data onto the cloud. Their data resides in S3, in a directory of JSON logs on user activity on the app, as well as a directory with JSON metadata on the songs in their app.

As their data engineer, you are tasked with building an ETL pipeline that extracts 
their data from S3, stages them in Redshift, and transforms data into a set of 
dimensional tables for their analytics team to continue finding insights in what songs 
their users are listening to. You'll be able to test your database and ETL pipeline 
by running queries given to you by the analytics team from Sparkify and compare your 
results with their expected results

In this project, you'll apply what you've learned on Spark and data lakes to build an ETL 
pipeline for a data lake hosted on S3. To complete the project, you will need to load 
data from S3, process the data into analytics tables using Spark, and load them back 
into S3. You'll deploy this Spark process on a cluster using AWS.

The data sources to ingest into data lake are provided by two public S3 buckets:

1. Songs bucket (s3://udacity-dend/song_data), contains info about songs and artists. 

2. Event bucket (s3://udacity-dend/log_data), contains info about actions done by users related to song database.

### Partition parquet files
Below is the schema of the datalake

#### Dimension tables

##### TABLE users

~~~~
root
 |-- firstName: string (nullable = true)
 |-- lastName: string (nullable = true)
 |-- gender: string (nullable = true)
 |-- level: string (nullable = true)
 |-- userId: string (nullable = true)
~~~~

##### TABLE songs (partitioned by year and artist_id)

~~~~
root
 |-- song_id: string (nullable = true)
 |-- title: string (nullable = true)
 |-- artist_id: string (nullable = true)
 |-- year: long (nullable = true)
 |-- duration: double (nullable = true)
~~~~


##### TABLE artists

~~~~
root
 |-- artist_id: string (nullable = true)
 |-- artist_name: string (nullable = true)
 |-- artist_location: string (nullable = true)
 |-- artist_latitude: double (nullable = true)
 |-- artist_longitude: double (nullable = true)
~~~~

##### TABLE time (partitioned by year and month)

~~~~
root
 |-- start_time: timestamp (nullable = true)
 |-- hour: integer (nullable = true)
 |-- day: integer (nullable = true)
 |-- week: integer (nullable = true)
 |-- month: integer (nullable = true)
 |-- year: integer (nullable = true)
 |-- weekday: integer (nullable = true)
~~~~


#### Fact table

##### TABLE songplays (Partitioned by year and month)

~~~~
root
 |-- start_time: timestamp (nullable = true)
 |-- userId: string (nullable = true)
 |-- level: string (nullable = true)
 |-- sessionId: long (nullable = true)
 |-- location: string (nullable = true)
 |-- userAgent: string (nullable = true)
 |-- song_id: string (nullable = true)
 |-- artist_id: string (nullable = true)
 |-- songplay_id: long (nullable = false)
~~~~


--------------------------------------------

### ETL process

All the ETL steps are done in Spark.

--------------------------------------------

#### Project structure

The structure is:

* <b> etl.py </b> - This script executes the actual ETL process.

We need an extra file with the credentials an information about AWS resources named <b>dhw.cfg</b>

#### How to run

1. Replace AWS IAM Credentials in dl.cfg with your credentials
2. Modify input and output data paths (If needed) in etl.py main function.It is currently set to the default location.
3. On the terminal run the command - python etl.py
