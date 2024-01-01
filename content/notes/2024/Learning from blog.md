+++  
title = 'Learning from blog'  
date = 2024-01-01T08:19:32.3232+05:30  
draft = false  
+++

## **[How LinkedIn Adopted Protocol Buffers to Reduce Latency by 60%](https://newsletter.systemdesign.one/p/protocol-buffers-vs-json)**

- They heavly using microservice to avoid the latency between the service they go with protocol buffers
- Protocol buffer (**Protobuf**) is a data serialization format and a set of tools to exchange data. Protobuf keeps data and the metadata separate. And serializes data into binary format.
- Use Protobuf when:
    - Payload is big
    - Communication between non-JavaScript environments needed
    - Frequent changes to the payload schema expected

## **[Scaling Slack’s Job Queue](https://slack.engineering/scaling-slacks-job-queue/)**

- They using [redis queue](https://redis.com/glossary/redis-queue/) for all async operation they have face few issue with that such as **Operational Memory Constraints in Redis, Complex Redis Connection Structure ,Worker Scalability Dependency on Redis etc**
- Instead fully replace with kafa they go step by step.First they push message to kafa and from kafa they have written JQRelay which is a stateless service written in Go that relays jobs from a Kafka topic to its corresponding Redis cluster.
- When a JQRelay instance starts up, it attempts to acquire a [Consul](https://www.consul.io/) lock on an key/value entry corresponding to the Kafka topic. If it gets the lock, it starts relaying jobs from all partitions of this topic.

## **[Hashnode's Overall Architecture](https://engineering.hashnode.com/hashnodes-overall-architecture)**

- They caching mechanism
    - page cache using next js
    - API cache using `GraphQL Edge Caching with Stellate` and `Serverless Function Caching with Vercel`

## **[Scaling Cron Monitoring](https://sentry.engineering/blog/scaling-cron-monitoring)**

- They have Relay which listen for check in and other event from cron and push to kafa
    
- Sentry Django application will consume from and do the heavy lifting of storing and processing those check-ins
    
- To inform the user about missed check in they written a job which run for a minute and checks on table whether it has check in if it not inform the user but it have 2 problem
    
    - Assume due to overload in kafa consumer get consumed later but the job runs before and assume it has not checked in
    - During deploys of the celery beat scheduler, there is a short period where tasks may be skipped.
- Their solution involved leveraging the timestamp of each check-in message to determine when all check-ins up to a minute boundary had been processed. They tracked the last time a minute boundary was crossed based on these timestamps. Once they reached a new minute boundary, they knew all check-ins before that moment had been consumed.
    
    For instance, imagine a sequence of check-in messages with timestamps:
    
    - Message 1: Timestamp 12:03:20
    - Message 2: Timestamp 12:03:45
    - Message 3: Timestamp 12:04:02
    
    When Message 3 arrives, it crosses the minute boundary of 12:04. This signals that all check-ins before 12:04 have been processed. This event becomes the trigger to generate tasks for detecting missing check-ins, eliminating the need for periodic celery beat tasks. This way, they avoid skipping tasks during deployment and accurately detect missed check-ins even during Kafka backlog scenarios.