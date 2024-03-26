# Kafka_streaming_project
##### This project dataset is from Kaggle; it contains all the metadata on Netflix for TV shows and movies. The project is to simulate Real-time streaming for movie details using Kafka. We used different technologies such as Python, Amazon EC2, Apache Kafka, Glue, Athena, and SQL.


## Launch an ec2 on AWS 
<img width="1267" alt="Screen Shot 2024-03-26 at 1 09 49 PM" src="https://github.com/gakas14/Kafka_streaming_project/assets/74584964/4edc7068-31d3-40bc-95ab-16d1e77de2be">

## Connect to your instance 
<img width="667" alt="Screen Shot 2024-03-26 at 1 13 31 PM" src="https://github.com/gakas14/Kafka_streaming_project/assets/74584964/26599614-5d38-4c89-953a-ee0e1fbff1db">

## Install Kafka 
```
  wget https://downloads.apache.org/kafka/3.7.0/kafka_2.13-3.7.0.tgz
  tar -xvf kafka_2.13-3.7.0.tgz
```

## Install Java  
```
  sudo yum install java-1.8.0
  java -version
```

## Edit inbound rules to allow the request from the local machine 


## Change the server to run on the public IP of the ec2 instance
```
sudo nano config/server.properties
```

## Start the zookeeper 
```
bin/zookeeper-server-start.sh config/zookeeper.properties
```

## Start Kafka server 
```
export KAFKA_HEAP_OPTS="-Xmx256M -Xms128M"
cd kafka_2.13-3.7.0
bin/kafka-server-start.sh config/server.properties
```
## Create a topic 
```
bin/kafka-topics.sh --create --topic netflix_data --bootstrap-server {Put the Public IP of your EC2 Instance:9092} --replication-factor 1 --partitions 1
```
<img width="566" alt="Screen Shot 2024-03-26 at 1 24 34 PM" src="https://github.com/gakas14/Kafka_streaming_project/assets/74584964/9edf2d15-1548-465f-99de-0cd87642c89b">

## Start Producer 
```
cd kafka_2.13-3.7.0
bin/kafka-console-producer.sh --topic netflix_data --bootstrap-server {Put the Public IP of your EC2 Instance:9092}
```

## Start Consumer 
```
cd kafka_2.13-3.7.0
bin/kafka-console-consumer.sh --topic netflix_data --bootstrap-server {Put the Public IP of your EC2 Instance:9092}
```

## Create a s3 bucket
<img width="1268" alt="Screen Shot 2024-03-26 at 1 27 22 PM" src="https://github.com/gakas14/Kafka_streaming_project/assets/74584964/03e2fc49-7db8-4849-a069-4bef74f1fd40">


## Open Jupyter Notebook and create a producer and consumer
###  Producer
<img width="1272" alt="Screen Shot 2024-03-26 at 1 33 25 PM" src="https://github.com/gakas14/Kafka_streaming_project/assets/74584964/e6e06384-a889-4db6-afcc-c39689e2614f">

<img width="1272" alt="Screen Shot 2024-03-26 at 1 34 43 PM" src="https://github.com/gakas14/Kafka_streaming_project/assets/74584964/df1405f5-1af2-4885-b0e2-0a2a49b704a3">



### Consumer
<img width="1268" alt="Screen Shot 2024-03-26 at 1 33 16 PM" src="https://github.com/gakas14/Kafka_streaming_project/assets/74584964/13adf630-78a7-4781-a8e7-bd5e1fa6e353">

 ## Check the data in the s3 bucket
 
<img width="1277" alt="Screen Shot 2024-03-26 at 2 12 40 PM" src="https://github.com/gakas14/Kafka_streaming_project/assets/74584964/a113a2b7-a04b-4e9c-8d74-b1cabc5de50c">


## Build a crawler in AWS Glue 
### Add the s3 bucket as a data source
<img width="1256" alt="Screen Shot 2024-03-26 at 1 44 05 PM" src="https://github.com/gakas14/Kafka_streaming_project/assets/74584964/cb03b0e4-df2b-4b41-b7e2-157c3cf8957e">

### Create a database
<img width="1257" alt="Screen Shot 2024-03-26 at 1 41 47 PM" src="https://github.com/gakas14/Kafka_streaming_project/assets/74584964/b7dd9cf9-c78f-4057-81e5-445ba99a9a60">

### Run the crawler

<img width="1178" alt="Screen Shot 2024-03-26 at 1 44 48 PM" src="https://github.com/gakas14/Kafka_streaming_project/assets/74584964/ba2adb4e-e319-4583-a45b-1d58deab5211">

<img width="1162" alt="Screen Shot 2024-03-26 at 1 46 33 PM" src="https://github.com/gakas14/Kafka_streaming_project/assets/74584964/489233fe-196b-4af4-947f-2007e6601f9d">


## Run queries on the table in Athena 

<img width="1245" alt="Screen Shot 2024-03-26 at 1 47 52 PM" src="https://github.com/gakas14/Kafka_streaming_project/assets/74584964/7706a0cd-557e-40f7-98a0-dd4dd87158a2">

![Screen Shot 2024-03-26 at 1 48 03 PM](https://github.com/gakas14/Kafka_streaming_project/assets/74584964/928adb2c-0d20-4f58-975c-a1ec7397f429)


### We can run different types of queries
##### query movie add in 2020
```
SELECT * FROM "netflix_movies_db"."gakas_kafka_netflix_data" WHERE release_year=2020;
```
<img width="785" alt="Screen Shot 2024-03-26 at 1 56 07 PM" src="https://github.com/gakas14/Kafka_streaming_project/assets/74584964/b2544154-1d92-4283-93ba-51de48bc9585">


<img width="803" alt="Screen Shot 2024-03-26 at 1 56 16 PM" src="https://github.com/gakas14/Kafka_streaming_project/assets/74584964/3a60e05f-3f87-4746-bb1d-3b6a1edce4b0">



#### Query count movies by type
```
SELECT type,count(*)  FROM "netflix_movies_db"."gakas_kafka_netflix_data" Group BY type;
```

<img width="1266" alt="Screen Shot 2024-03-26 at 2 04 23 PM" src="https://github.com/gakas14/Kafka_streaming_project/assets/74584964/032febcc-edbe-42f8-aec8-050858debe0b">


