+++
title = 'Tech stack of popular companies'
date  = 2024-01-12T08:19:32.3232+05:30
draft = true
+++

## Shopify’s
1. **Back-end**: Ruby on Rails (+ some Lua and Go)
2. **Datastores** MySQL , Redis for queues and background jobs
3.  **Front-end**: React with GraphQL API
4. Type : **monolith**
5. **architecture**: **domain-driven**, Multi-Tenant Architecture  
6.  **load balancing layer** : Nginx, Lua, and OpenResty.

## Tinder
1. 500 microservice

## Linkedin 
1. Azure front end for CDN

## Cloudfare
1. Logging : ELK and Clickhouse

## Discord
1. Mongodb -> cassendara -> Sycalldb

## Netflix
1. AWS

## instagram
1. Rabbitmq
2. Django
3. postgresql -> user, media frendship,comments
4. cassendra -> feeds activites,etc
5. celery
6. memcache

## Reddit
- Rabbitmq
- Cassendra/postgrsql
- Monlotith
- zookeeper
- AWS
- GO
- Graphql
- AWS Aurora Postgres  (for storing media metadata)


## Apple
1. ICloud
	1. iCloud is partly powered by Cassandra. Apple runs one of the largest Cassandra deployments in the world, [according to DataStax](https://news.ycombinator.com/item?id=9307563).

## Slack
1. AWS (cell based architecture)
2. mySQL
3. PHP 


## Discord
1. Scallydb 
2. They have data base service build with rust where it faster https://tokio.rs/


## Quora
1. Kubernetes clusters for container orchestration and separate EC2 instances for particular services.
2. AWS

## Zapier
- They run the Nginx web server and Python [Django](https://substack.com/redirect/64c1a061-6e94-4719-9b2e-06afd8993547?j=eyJ1IjoibjRqeW8ifQ.nH1cA-1Vi6dJQ5z2LJKZVMl1Hi7wGsFWpp4_SYW_xWo) framework on the backend.
- stores data in MySQL and Redis. Put another way, Zaps gets stored in MySQL.
- AWS Lambda runs _custom scripts_ provided by the user.
- Celery(Distributed Task Queue) and rabbitMq
- Kafka
- Elasticsearch
- kubernetes

## Uber
1. Docstore database top of mysql build by own
2. Redis and uses a _cache-aside_ strategy. for caching

## Airbnb
-  **Service-Oriented Architecture**
- **Data Service -** This is the bottom layer and acts as the entry-point for all read and write operations on the data entities. A data service must not be dependent on any other service because it only accesses the data storage.
    
- **Derived Data Service -** The derived services stay one layer above the data service. These services can read from the data services and also apply some basic business logic.
    
- **Middle Tier -** They are used to house large pieces of business logic that doesn’t fit at the data service level or the derived data service level.
    
- **Presentation Service** - At the very top of the structure are the presentation services. Their job is to aggregate data from all the other services. In addition, the presentation service also applies some frontend specific business logic before returning the data to the client.

## Canva
- Using Mongodb as primary database and having sharding

#### Paypal
- JunoDB [[Databases#JunoDB’s]]  for caching
- PostgreSQL  primary datastor

## Lyft
- DynamoDB
-  **Redis cluster**

#### Figma
- Amazon RDS
- Ruby

## Notion
- PostgreSQL
- AWS

Zerodha
- Postgres
- Nomad (alternative to kubernetes by hasicrop)