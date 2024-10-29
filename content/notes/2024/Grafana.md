---
title : Grafana
date : 2024-05-12T15:20:04.044+05:30
draft : true
tags : 
---


**Prometheus Server**: This component is responsible for scraping and storing time-series data. It collects metrics from monitored targets by periodically scraping HTTP endpoints, allowing for flexible querying and visualization.

**Prometheus Client Libraries**: These libraries are available for various programming languages, enabling instrumentation of application code to expose custom metrics. (**Prometheus Exporter**)

**Storage:** metrics are stored in a time-series database called the "TSDB" (Time Series Database). The TSDB is a core component of the Prometheus server. When Prometheus scrapes metrics from targets, it stores them locally in its TSDB.

The endpoint exposed for metrics **/metrics**

**Metrics format**
- Metrics entries: Type and HELP attributes
- Help -> description of what the metrics is
- Type -> Counter,Gauge,histogram
	- Counter -> how many times x happened
	- Gauge -> what is the current vaule of x now
	- Histogram -> how long?


**PromQL:** is the query language used in Prometheus for querying and analyzing time-series data



#### PromQL

##### **Expression language data types**

**Scalar** - a simple numeric floating point value

**String** - a simple string value; currently unused

**Instant vector** - a set of time series containing a single sample for each time series, all sharing the same timestamp 

Example `http_requests_total` -> will return elements for all time series that have this metric name. `http_requests_total{job="prometheus",group="canary"}`  -> with lables match

**Range vector** - a set of time series containing a range of data points over time for each time series

Example `http_requests_total{job="prometheus"}[5m]` -> return recorded within the last 5 minutes for all time series




