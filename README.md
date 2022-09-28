# Data-Pipline-Apache-Airflow

## Overview
This project is about building an Airflow ETL Pipeline for Sparkify Company. The company wants to automate and monitor their data warehousing ETL on AWS.
The source data resides in S3 and needs to be processed in Sparkify's data warehouse in Amazon Redshift. The source datasets consist of JSON logs that tell about user activity in the application and JSON metadata about the songs the users listen to. Also, wants Data Quality tests run against their datasets after the ETL steps have been executed to catch any discrepancies in the datasets.

## Prerequisites:
1- Create an IAM User in AWS. </br>
Attach Policies: `AdministratorAccess`, `AmazonRedshiftFullAccess` and `AmazonS3FullAccess`

2- Create a redshift cluster.

## Airflow Connection:
1- Connect Airflow and AWS (AWS Credentials). </br>
Run `/opt/airflow/start.sh`, Click on the Admin tab and select Connections. </br>
Then create Amazon Web Services conn you will Enter `Access Key` in login and `Secret key` in password from the IAM User credentials.

2- Connect Airflow to the AWS Redshift Cluster. </br>
Create Postgres Conn with credentials to Redshift

## Project Dataset
There are two datasets that reside in S3:

- Song data: `s3://udacity-dend/song_data`
- Log data: `s3://udacity-dend/log_data`

#### Song Dataset
The first dataset is a subset of real data from the Million Song Dataset. Each file is in JSON format and contains metadata about a song and the artist of that song. The files are partitioned by the first three letters of each song's track ID. For example, here are file paths to two files in this dataset.
```
song_data/A/B/C/TRABCEI128F424C983.json
song_data/A/A/B/TRAABJL12903CDCF1A.json
```
And below is an example of what a single song file, TRAABJL12903CDCF1A.json, looks like.
```
{"num_songs": 1, "artist_id": "ARJIE2Y1187B994AB7", "artist_latitude": null, "artist_longitude": null, "artist_location": "", "artist_name": "Line Renaud", "song_id": "SOUPIRU12A6D4FA1E1", "title": "Der Kleine Dompfaff", "duration": 152.92036, "year": 0}
```

#### Log Dataset
The second dataset consists of log files in JSON format generated by this event simulator based on the songs in the dataset above. These simulate activity logs from a music streaming app based on specified configurations.

The log files in the dataset you'll be working with are partitioned by year and month. For example, here are filepaths to two files in this dataset.
```
log_data/2018/11/2018-11-12-events.json
log_data/2018/11/2018-11-13-events.json
```

## Database Schema Design

#### Fact Table:
1. ***songplays***: records in log data associated with song plays i.e. records with page NextSong
        -songplay_id, start_time, user_id, level, song_id, artist_id, session_id, location, user_agent
        
#### Dimension Tables
1. ***users*** - users in the app
        -user_id, first_name, last_name, gender, level
2. ***songs*** - songs in music database
        -song_id, title, artist_id, year, duration
3. ***artists*** - artists in music database
        -artist_id, name, location, latitude, longitude
4. ***time*** - timestamps of records in songplays broken down into specific units
        -start_time, hour, day, week, month, year, weekday
        
## Project Template
Project files<br>

1. `dl.cfg`: Contains AWS credentials.
2. `etl.py`: Reads data from S3, processes that data using Spark, and writes them back to S3.
3. `README.md`: Provides discussion on the project.

## ETL pipeline
- Extract data from AWS S3, `Song data` and `Log data`.
- Transform to create dimenstional and fact tables using Apache Spark.
- Load them back to AWS S3 Data Lake partitioned parquet files. <br>
 We used Parquet format because: Low storage consumption and higher execution speed.


## Confguration
To get AWS Credentials:
1. Create IAM User with `AmazonS3FullAccess` Policy.
2. Then you will get the `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`.

## How to run the Python Scripts
  
Run `etl.py`.

  ``` python etl.py```

## Author
Esraa Ahmed | <a href="https://linkedin.com/in/esraa-ahmed-ibrahim2" target="blank"><img align="center" src="https://raw.githubusercontent.com/rahuldkjain/github-profile-readme-generator/master/src/images/icons/Social/linked-in-alt.svg" alt="esraa-ahmed-ibrahim2" height="15" width="15" /></a>

Created on 10/09/2022
