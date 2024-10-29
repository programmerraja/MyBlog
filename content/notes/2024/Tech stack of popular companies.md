---
title: Tech stack of popular companies
date: 2024-01-12T08:19:32.3232+05:30
draft: true
tags:
  - learning
---

---
## Shopify
- **Back-end**: Ruby on Rails (plus some Lua and Go)
- **Datastores**: MySQL, Redis (for queues and background jobs)
- **Front-end**: React with GraphQL API
- **Type**: Monolith
- **Architecture**: Domain-Driven, Multi-Tenant Architecture
- **Load Balancing Layer**: Nginx, Lua, OpenResty

## Tinder
- **Microservices**: 500 microservices

## LinkedIn
- **Front-end CDN**: Azure

## Cloudflare
- **Logging**: ELK (Elasticsearch, Logstash, Kibana), Clickhouse

## Discord
- **Datastores**: MongoDB, Cassandra, ScyllaDB
- **Additional Info**: Database service built with Rust for performance (Tokio)

## Netflix
- **Cloud Provider**: AWS

## Instagram
- **Datastores**: PostgreSQL (user, media, friendship, comments), Cassandra (feeds, activities, etc.)
- **Message Broker**: RabbitMQ
- **Framework**: Django
- **Task Queue**: Celery
- **Cache**: Memcache

## Reddit
- **Datastores**: Cassandra, PostgreSQL
- **Message Broker**: RabbitMQ
- **Architecture**: Monolith
- **Other Tools**: Zookeeper
- **Cloud Provider**: AWS
- **Languages**: Go
- **API**: GraphQL
- **Relational Database**: AWS Aurora PostgreSQL (for storing media metadata)

## Apple
- **iCloud**: Powered by Cassandra (one of the largest Cassandra deployments in the world)

## Slack
- **Cloud Provider**: AWS (cell-based architecture)
- **Datastores**: MySQL
- **Language**: PHP

## Quora
- **Container Orchestration**: Kubernetes clusters
- **Cloud Provider**: AWS

## Zapier
- **Web Server**: Nginx
- **Backend Framework**: Python Django
- **Datastores**: Postgress, Redis
- **Task Queue**: Celery, RabbitMQ
- **Message Broker**: Kafka
- **Search**: Elasticsearch
- **Container Orchestration**: Kubernetes
- **Serverless**: AWS Lambda

## Uber
- **Database**: Docstore (built on top of MySQL)
- **Cache**: Redis (cache-aside strategy)

## Airbnb
- **Architecture**: Service-Oriented Architecture
  - **Data Service**: Handles all read and write operations on data entities.
  - **Derived Data Service**: Reads from data services and applies basic business logic.
  - **Middle Tier**: Houses large pieces of business logic.
  - **Presentation Service**: Aggregates data from other services and applies frontend-specific logic.

## Canva
- **Database**: MongoDB (with sharding)

## PayPal
- **Cache**: JunoDB
- **Primary Datastore**: PostgreSQL

## Lyft
- **Datastore**: DynamoDB
- **Cache**: Redis cluster

## Figma
- **Database**: Amazon RDS
- **Language**: Ruby

## Notion
- **Datastore**: PostgreSQL
- **Cloud Provider**: AWS

## Zerodha
- **Database**: PostgreSQL
- **Container Orchestration**: Nomad (alternative to Kubernetes by HashiCorp)

## Twitter
- **Search**: Elasticsearch

## Facebook
- **Cache**: Memcached

## Stripe
- **Database**: MongoDB and  DocDB [[System Design Case study#Stripe]]  on top of MongoDB.

## **Pinterest**
- **Analaytics**: StarRocks [[Databases#StarRocks]] is an open-source, OLAP (_analytics-focused_) database that’s designed for running low-latency queries on data in real-time
- 

