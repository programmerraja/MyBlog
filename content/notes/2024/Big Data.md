---
title : Big Data
date : 2024-03-03T10:06:51.5151+05:30
draft : true
tags : 
---

Kappa architecture


#### Apache Druid
[Apache Druid](https://druid.apache.org/) is an in-memory, columnar, distributed, open-source data store designed for sub-second queries on real-time and historical data. Druid enables low latency (real-time) data ingestion, flexible data exploration and fast data aggregation resulting in sub-second query latencies.

#### ClickHouse
ClickHouse is an open-source, column-oriented database for online analytical processing. One of ClickHouse’s standout factors is its high performance—due to a combination of factors such as column-based data storage & processing, data compression, and indexing.


Data warhouse and Data lake

Data warhouse for anlaytics purpose also know as ETL (extract transform and load) where all historically data stored in warhouse for analytics

Data warehouses are designed for querying and reporting. They support complex queries and are used for business intelligence (BI) tasks, where high performance and consistency are crucial.

Traditional data warehouse solutions include Amazon Redshift, Google BigQuery, and Snowflake.

Data lakes store data in its raw, unstructured, or semi-structured format. They use a schema-on-read approach, meaning the structure is applied when the data is read, not when it is stored.

Data lakes are designed to store vast amounts of raw data for future analysis. They are often used for data exploration, data science, and machine learning, where flexibility and the ability to process diverse data types are important.

Popular data lake solutions include Amazon S3 with AWS Glue, Azure Data Lake, and Google Cloud Storage with BigQuery.