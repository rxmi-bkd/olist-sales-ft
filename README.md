# olist-end-to-end-data-engineering-project

TODO : setup data warehouse with airflow each time the pipeline is run

## Description

### Architecture

<img src="images/olist-architecture.drawio.svg">

### Pipeline

ETL stands for Extract, Transform, Load. An ETL pipeline is a set of processes used to collect data from various
sources, transform it into a format that is suitable for analysis or storage, and then load it into a target
destination, such as a database, data warehouse, or data lake.

In this project, we focusing on generating an orders fact table from the dataset provided
by [Olist](https://www.kaggle.com/olistbr/brazilian-ecommerce) in order to analyze the sales performance of the company.

<table>
    <tr>
        <th>Component</th>
        <th>Usage</th>
    </tr>
    <tr>
        <td>Minio</td>
        <td>Minio serves as the data lake where raw data is stored before being processed.</td>
    </tr>
    <tr>
        <td>Airflow</td>
        <td>Airflow is used to orchestrate the ETL pipeline.</td>
    </tr>
    <tr>
        <td>Spark & Python</td>
        <td>PySpark is used to process the raw data and generate the orders fact table.</td>
    </tr>
    <tr>
        <td>MySQL</td>
        <td>MySQL is used as the data warehouse where the orders fact table is stored.</td>
    </tr>
</table>

## Structure

```
.
├── README.md
├── data-warehouse.sql
├── compose.yaml
├── .gitignore
├── logs
│   └── ...
├── docker
├── dags
│   └── build_olist_orders_ft.py
├── jobs
│   ├── build_customers_dim.py
│   ├── build_dates_dim.py
│   ├── build_orders_detail_ft.py
│   ├── build_orders_header_ft.py
│   ├── build_products_dim.py
│   └── build_sellers_dim.py
└── jars
    ├── hadoop-aws-3.3.4.jar
    ├── aws-java-sdk-bundle-1.12.262.jar
    └── mysql-connector-java-8.4.0.jar
```

## Setup

### 1) Setup docker

```bash
docker-compose up
```

### 2) Setup data warehouse

run ```data-warehouse.sql``` script in mysql container

### 3) Setup spark connection in [airflow](http://localhost:8080/home)

follow
this [guide](https://airflow.apache.org/docs/apache-airflow/stable/howto/connection.html#creating-a-connection-with-the-ui)

<table>
  <tr>
    <th>Airflow login</th>
    <td>admin</td>
  </tr>
  <tr>
    <th>Airflow password</th>
    <td>admin</td>
  </tr>
  <tr>
    <th>Connection Id</th>
    <td>spark-conn</td>
  </tr>
  <tr>
    <th>Connection Type</th>
    <td>spark</td>
  </tr>
  <tr>
    <th>Host</th>
    <td>spark://spark</td>
  </tr>
  <tr>
    <th>Port</th>
    <td>7077</td>
  </tr>
</table>

### 4) Upload [dataset](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce) to datalake

use [minio web interface](http://localhost:9000) to upload dataset into olist bucket
<table>
  <tr>
    <th>login</th>
    <td>admin</td>
  </tr>
  <tr>
    <th>password</th>
    <td>adminadmin</td>
  </tr>
</table>

### 5) Download jars

download the following jars and place them in the jars folder

[mysql-connector-j](https://repo.maven.apache.org/maven2/com/mysql/mysql-connector-j/8.4.0/mysql-connector-j-8.4.0.jar) >>
used to connect to data warehouse

[hadoop-aws](https://repo.maven.apache.org/maven2/org/apache/hadoop/hadoop-aws/3.3.4/hadoop-aws-3.3.4.jar) & [aws-java-sdk](https://repo.maven.apache.org/maven2/com/amazonaws/aws-java-sdk-bundle/1.12.262/aws-java-sdk-bundle-1.12.262.jar) >>
used to connect to datalake
