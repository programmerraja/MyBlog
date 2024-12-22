---
title: Learning from blog
date: 2024-01-01T08:19:32.3232+05:30
draft: false
---
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

	- Message Timestamps: Each check-in message has a timestamp indicating when the cron job was supposed to run1. For instance, your cron job that was supposed to run at 1:00 PM would have a 1:00 PM timestamp on its check-in message, even if the message arrives at 1:03 PM1.
	- Consumer-Driven Clock: Sentry uses the messages to drive a clock. This means it keeps track of the last time it crossed a minute boundary based on the timestamp of the check-ins1. This clock moves forward as messages are consumed.
	- Kafka Partitions and Multiple Consumers: Incoming check-in messages are distributed across multiple partitions in Kafka2. Multiple consumers read from these partitions, and each consumer processes messages at their own speed2.
	- Global Clock Synchronization: To handle the multiple consumers and partitions, the system maintains a global clock that is synchronized across all partitions3. This global clock will only advance when all partitions have processed messages up to the same time3.
		- For example, if some partitions have processed messages up to 1:01 PM, and others have not, the global clock will not advance to 1:01 PM yet3.
		-  The global clock will only advance to 1:01 PM once all consumers have processed all messages up to that time



## [ An overview of Cloudflare's logging pipeline](https://blog.cloudflare.com/an-overview-of-cloudflares-logging-pipeline)

- They used BSD syslog protocol to send the log from system

-  [Syslog-ng ](https://github.com/syslog-ng/syslog-ng) is a daemon that implements the aforementioned BSD syslog protocol. In our case, it reads logs from journald, and applies another layer of rate limiting. It then applies rewriting rules to add common fields, such as the name of the machine that emitted the log, the name of the data center the machine is in

- Logs are pushed to Kafka 

- Logs stored in ELK stack, and a Clickhouse cluster
	- ElasticSearch with 90 cluster 
	- Clickhouse with 10 Nodes of cluster


## https://roughlywritten.substack.com/p/a-simple-technique-for-more-reliable

- Find the core service our app and list the things
- List *how* something could fail, not *why* something could fail.
- For each failure case, reason through a fix (Automated recovery,Manual recovery)
- Prioritize and execute


Two data processing architectures that are extremely popular in the industry are the **Lambda architecture** and the **Kappa architecture**.

https://read.engineerscodex.com/p/how-apple-built-icloud-to-store-billions

1. Apple uses [FoundationDB](https://www.foundationdb.org/) (apple bought this) and Cassandra


## [ How Uber Serves Over 40 Million Reads Per Second from Online Storage Using an Integrated Cache](https://www.uber.com/en-IN/blog/how-uber-serves-over-40-million-reads-per-second-using-an-integrated-cache/)
- See how they avoiding before itself when the redis down they know that there DB get more query so they adjust the query time out in db to reduce the load in there DB

## [Using checksums to verify syncing 100M database records](https://sirupsen.com/napkin/problem-14-using-checksums-to-verify)
- Pick one unique filed in collection let say `id` add all `id` and hash the content and do the same for another database and if both same it sync
- To optimize do batch by batch check first 1000 and so until if one is not matched or all matched.

## Backoff technique to heavy load of db write

- Canva implemented the backoff in there autosave feature where they write custom monitor which will monitor each update query in db and check does it corssing the limit if so it will inform the front end to backoff the auto save time by sending in header `Canva-Throttle: true`



