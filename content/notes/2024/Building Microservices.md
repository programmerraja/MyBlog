---
title: Building Microservices
date: 2024-01-23T18:50:07.077+05:30
draft: false
tags:
  - microservices
  - book
---

## Microservices

service-oriented architecture vs microserice
The microservice approach has emerged from real-world use, taking our better 
understanding of systems and architecture to do SOA well

Before thinking as service try to see does the problem can solved by
- Shared Libraries (Creating the service as lib and reusing where it needed)
- Modules (same as libraries)


## How to Model Services

Cohesion related behavior to sit together, and unrelated behavior to sit elsewhere. Why? Well, if we want to change behavior, we want to be able to change it in one place, and release that change as soon as possible. If we have to change that behavior in lots of different places, we’ll have to release lots of different services  to deliver that change. Making changes in lots of different places is slower, and deploying lots of services at once is risky

Bounded context
- understand the bussiness well and user then only we can come up with bounded context
- Bounded context decouple parts. parts are code and teams
- Bounded context  are about enabling team autonomy AAA

clue for discovering context
- Linguistic boundaries (group the word that are smillar)
- data flow owner ship
- Domain expert boundaries


finding service boundaries the one rule that matters maximise your ability to frequently deliver and get feedback

`the art of discovering bounded context is the art of aligning orignisational and technical boundaries`


book: theory of constraints,finding service boundaires,alingment at scale,scs-architeture.org,lean enterprise,Beyond Software Architecture

events stroming

Need to study above chapter

Ways to find bounded context
 
 [Domain story telling](https://youtu.be/Y1ykXnl6r7s?si=3PYtb_ak-IQP63_4)
 - Draw the story with character and arrow of data flow (try to group the things where the arrows going to single point )	

[Event Stroming](https://youtu.be/mLXQIYEwK24?si=C-WZb2_IvQRCtu52)
-  is a collaborative meeting technique for Domain-Driven Design. It enables cross-functional teams to rapidly explore and model complex business domains through _workshop meetings_. It is a visual and interactive process that encourages team members to collaborate and share their knowledge to uncover _domain events_ and identify _business rules_.

https://medium.com/building-inventa/how-we-used-event-storming-meetings-for-enabling-software-domain-driven-design-401e5d708eb

## Integration

#### Hide Internal Implementation Detail

If we doing changes in one service and the service that depends no need to worry about and we want make sure that not much depend on the internal for this service
#### Synchronous Versus Asynchronous

Synchronous are blocking the resources until we getting the response where async are event driven which will subscribe for the event
#### Orchestration Versus Choreography

 **Orchestration:** With orchestration, we rely on a central brain to guide and drive the process, much like the conductor in an orchestra.
- `Example:` when user login we need to send welcome and other things in orchestration it will send to orchestrator about the event where it will call corresponding service for trigger the events
- High coupling 

**Choreography:** is a decentralized approach where each microservice plays an active role in determining when and how to communicate with other microservices.
- `Example:` when user login we need to send welcome and other things in choreography the user login service will take responsiblity of trigger events in other serivce
- low coupling
- Extra work to monitor and track the processes across system boundaries

#### Asynchronous Event-Based
- Message broker (RabbitMQ)

#### Services as State Machines

A Service is responsible for controlling all lifecycle events associated with the customer. This includes not just CRUD (Create, Read, Update, Delete) operations but also other events and processes related to the service lifecycle, such as account verification, status changes, or notifications. in case of customer service.`So dont't create a service that only exposing CRUD for the service`

#### Reactive Extensions

RX are a mechanism to compose the results of multiple calls together and run operations on them.

Rx are centered around handling asynchronous and event-driven programming in a more declarative and composable way

#### Access by Reference

Assume we sending the user email to mail service where mail service using queue to consume the msg. due to high traffic the msg we send is on queue mean while when user is updating his email we not aware of it so instead of sending user email send the userId such that when mail service consuming the msg it do fetching and send the mail.

#### DRY

Avoiding coupling between microservices by being cautious about sharing code across service boundaries. even though code duplication (violating the "Don't Repeat Yourself" principle or DRY) within a microservice should generally be avoided.

`Example:` Logging Utility in Microservices where all service sending logs to the service if we do changes in logging all service need to adapt it which is high coupling so instead of seprate service we can have logging code duplicated in each service.

#### How to avoid breaking changes between services

**Versioning:**  Follow `Semantic Versioning` when updating the service such that other service will know does it major minor or breaking changes. even though we updated they can use the old versioning untill they want to update

**Coexist Different Endpoints:** When we going to do breaking changes in API that will only process the endpoint with query params make it optional until other service upating it.

 **Postel's Law:** `Be conservative in what you do, be liberal in what you accept from others.`

- **Conservative in What You Do:** When designing a system or a component, be conservative in what you produce or generate. This means adhering strictly to standards, specifications, or well-defined contracts. Ensure that your outputs are clear, unambiguous, and follow established conventions.
- `Example:` HTTP specifications. where they have standard that need to follow when sending request
- **Liberal in What You Accept from Others:** On the receiving end,designing your system to handle various inputs gracefully, even if they deviate slightly from the expected standards. This flexibility promotes interoperability and resilience.

## Splitting the Monolith

#### Seams
a portion of the code that can be treated in isolation and worked on without impacting the rest of the codebase.

#### The Reasons to Split the Monolith

**Ask following questions** 

- Why we need to split strong reason for that?
- What need to split first and what make less impact in code level and high impact in product

**Things need to considered**
 - Database
 - Programming language


#### How to refactor the database

Some methods to refactor the database when splitting microservice 

- If we have two collection that have relationship as foreign key we want two as seprate service (assume user table and shipping address table). we want to split user service and shippping service so if we want to acess the user realted detail we need to call the user service instead of directly accessing from DB. because this will help when we moving to seprate DB for the service

`Book recommened : Refactoring Databases by Scott J. Ambler and Pramod J. Sadalage`
#### Repository layer

The repository acts as an abstraction layer that encapsulates the details of how data is stored, retrieved, and manipulated. This helps in decoupling the application's business logic from the specifics of the underlying data storage technology.

**Handling Transactions**

We have two options one we make sure the transactions happen on different service or we can do eventual consistency 

Algorithm for handling distributed transactions
- two-phase commit
- use idopempotency and retry mechanism if one action failed

**Grouping data from different service**

**Data Retrieval via Service Calls**
- If data is huge we can store in s3 or any storage that we can access  instead of sending in HTTP

**Data Pumps** 
- Sync the data from service A to B so if we need data in B we can get from B itself. 
- this will be usefull when we do frequent data access from A
- Netfilx they instead of sync data from service A database they have backup for there data from the backup the create a pipeliine to gather the data


## Deployment
- one microservice per build
- Countinous deployment
- Our services need some configuration. Ideally, this should be a small amount, and limited to those features that change from one environment to another.Try to make ENV differenec for the service need to be minimal that will helpful when deploying in CI
- Single Service Per Host


## Monitoring





My understanding
- It is more about code it helps to how to orignize the pepole in company and how to make continous pushing code and understand well users
- imporve velocity of dev 





Resources
1. [The Art of Discovering Bounded Contexts by Nick Tune](https://www.youtube.com/watch?v=ez9GWESKG4I&t=982s)




## Question to ask when creating a service
- why it required 
- what are the benefits after we creating this service compare to before and after
- what is if keep the current system as it as and check what pros and cons that make before service and after service