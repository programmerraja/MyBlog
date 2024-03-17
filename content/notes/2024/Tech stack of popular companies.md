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

## Apple
1. ICloud
	1. iCloud is partly powered by Cassandra. Apple runs one of the largest Cassandra deployments in the world, [according to DataStax](https://news.ycombinator.com/item?id=9307563).

## Slack
1. AWS (cell based architecture)