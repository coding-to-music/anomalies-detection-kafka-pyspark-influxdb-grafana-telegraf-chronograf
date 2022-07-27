# anomalies-detection-kafka-pyspark-influxdb-grafana-telegraf-chronograf

# 🚀 visualize summaries of the data stream(s) and develop an ML model to detect anomalies in the data set. 🚀

https://github.com/coding-to-music/anomalies-detection-kafka-pyspark-influxdb-grafana-telegraf-chronograf

From / By Mohamed Aharrat https://github.com/medaharrat

https://github.com/medaharrat/anomalies-detection

http://localhost:8086/signin (influxdb userid=admin password=admin123)

http://localhost:3000 (Grafana - there are no dashboareds)

http://localhost:8080 (Chronograf - there are no dashboards)

http://localhost:9092 (Kafka - no UI)

http://localhost:8125 (Telegraf - no UI)

http://localhost:8888 (PySpark - no UI)

pyspark crashes connecting to Kafka

There are no dashboards

## Environment variables:

copy .env-example to .env

```java
cp .env-example .env

# example values:

INFLUXDB_DB=swat
INFLUXDB_HOST=http://influxdb:8086
AUTH_TOKEN=iJHZR-dq4I5LIpFZCc5bTUHx-I7dyz29ZTO-B4W5DpU4mhPVDFg-aAb2jK4Vz1C6n0DDb6ddA-bJ3EZAanAOUw==
DEFAULT_BUCKET=swat
MONITORING_BUCKET=primary
DEFAULT_ORGANIZATION=primary
ADMIN_USERNAME=admin
ADMIN_PASSWORD=admin123
```

## GitHub

```java
git init
git add .
git remote remove origin
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:coding-to-music/anomalies-detection-kafka-pyspark-influxdb-grafana-telegraf-chronograf.git
git push -u origin main
```

# Anomaly Detection in Secure Water Treatment Systems.

## Background

This project serves as an assignment project for the Open Source Technologies / Stream Mining subjects.

## Purpose

The project task is to visualize summaries of the data stream(s) and develop an ML model to detect anomalies in the data set.

## Architecture

The project streams data from an xlsx file through Kafka to a topic called SWAT.
This topic is then subscribed by the PySpark instance in another container.
Each batch is pre-processed and stored in InfluxDB as a data point together with anomalies.
Data is visualized in Grafana in 4 different boards representing anomalies detected using four different approaches.
In addition to that, a monitoring board is put in place using Telegraf to collect metrics and visualize in Chronograf.
<br/>
Each part of the project is containerized using Docker, in addition to two built images

## Technologies

The following technologies are used:

### Kafka

Apache Kafka is an open-source distributed event streaming platform used by thousands of companies for high-performance data pipelines, streaming analytics, data integration, and mission-critical applications.

### PySpark

PySpark is an interface for Apache Spark in Python. It not only allows you to write Spark applications using Python APIs, but also provides the PySpark shell for interactively analyzing your data in a distributed environment. PySpark supports most of Spark’s features such as Spark SQL, DataFrame, Streaming, MLlib (Machine Learning) and Spark Core.

### InfluxDB

InfluxDB is an open-source time series database developed by the company InfluxData. It is written in the Go programming language for storage and retrieval of time series data in fields such as operations monitoring, application metrics, Internet of Things sensor data, and real-time analytics

### Grafana

Grafana is a multi-platform open source analytics and interactive visualization web application. It provides charts, graphs, and alerts for the web when connected to supported data sources

### Chronograf

Chronograf is an open-source web application and visualization tool developed by InfluxData as part of the InfluxDB project

### Telegraf

Telegraf is a plugin-driven server agent for collecting & reporting metrics. Telegraf has plugins to source a variety of metrics directly from the system it’s running on, pull metrics from third-party APIs, or even listen for metrics via a statsd and Kafka consumer services.

## Run the project

```sh
sh run.sh
```

Getting this output:

```
[>] Exporting variables
[>] Creating influxdb directory
[>] Creating grafana directory
[>] Creating chronograf directory
[>] Running docker compose
Building pyspark
Step 1/15 : FROM jupyter/pyspark-notebook
 ---> 458b41649487
Step 2/15 : COPY . .
 ---> Using cache
 ---> 344ec311bab2
Step 3/15 : RUN python3 -m pip install -r requirements.txt
 ---> Using cache
 ---> 8f94e6174a9e
Step 4/15 : USER root
 ---> Using cache
 ---> 633a675fff27
Step 5/15 : RUN chmod +x spark-submit.sh
 ---> Using cache
 ---> bb7e509d3909
Step 6/15 : RUN echo '[...] Preprocessing data'
 ---> Using cache
 ---> 6f0dcc79c1f5
Step 7/15 : RUN python3 preprocess.py
 ---> Using cache
 ---> 13aa0b8e2e19
Step 8/15 : RUN echo '[...] Fitting models'
 ---> Using cache
 ---> 3d24b4575edf
Step 9/15 : RUN python3 pipeline1_ocsvm.py
 ---> Using cache
 ---> d95a0222eb87
Step 10/15 : RUN python3 pipeline2_iso_log.py
 ---> Using cache
 ---> 6503f4bc6625
Step 11/15 : RUN python3 pipeline3_kmeans.py
 ---> Using cache
 ---> c58fd85b167d
Step 12/15 : RUN python3 pipeline4_dbscan.py
 ---> Running in e569dfc780c7
ERROR: Service 'pyspark' failed to build: The command '/bin/bash -o pipefail -c python3 pipeline4_dbscan.py' returned a non-zero code: 137
```

## Docker Error: Returned a Non-zero Code: 137

https://samwize.com/2016/05/19/docker-error-returned-a-non-zero-code-137/

19 May 2016

1 min read
While running a node app with Docker, there is an error 137:

```
The command '/bin/sh -c npm install' returned a non-zero code: 137
```

This means a out of memory error.

To fix, you can add more RAM.

Or you can add more swap memory (FREE!). Swap memory uses part of your harddisk for temporary memory.

These steps are exactly the same from a previous guide:

```
# Confirm you have no swap
sudo swapon -s

# Allocate 1GB (or more if you wish) in /swapfile
sudo fallocate -l 1G /swapfile

# Make it secure
sudo chmod 600 /swapfile
ls -lh /swapfile

# Activate it
sudo mkswap /swapfile
sudo swapon /swapfile

# Confirm again there's indeed more memory now
free -m
sudo swapon -s

# Configure fstab to use swap when instance restart
sudo nano /etc/fstab

# Add this line to /etc/fstab, save and exit
/swapfile   none    swap    sw    0   0

# Change swappiness to 10, so that swap is used only when 10% RAM is unused
# The default is too high at 60
echo 10 | sudo tee /proc/sys/vm/swappiness
echo vm.swappiness = 10 | sudo tee -a /etc/sysctl.conf
```

## Clean Up

```
docker image prune -a

docker container prune

docker volume prune
```
