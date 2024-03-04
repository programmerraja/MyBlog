+++
title = 'Domain driven design'
date = 2024-01-24T19:58:19.1919+05:30
draft = true
tags =[]
+++ 


## Putting the Domain Model to Work

#### Crunching Knowledge

the iterative process of understanding the domain and refining the model based on continuous learning

## Communication and the Use of Language

The Ubiquitous Language is a common, shared language that both technical and non-technical team members use to discuss the domain. It ensures that everyone understands the domain's terms and concepts.

`Use the model as the backbone of a language. Committhe team to exercising that language relentlessly in all communication within the team and in the code. Use the same language in diagrams, writing, and especially speech`


Chapter 3 
## Binding Model and I mplementation





vaule objects
tell dont ask -> if u have module that u want to do some validation u write a function that get required data from module and do the validation in here we just getting the data so  instead put the validation inside module



### **Bounded Contexts**

Bounded Contexts are explicit boundaries within which a particular domain model is defined and applicable. They help prevent conflicts and misunderstandings when working on large, complex systems.

### **Entities and Value Objects**

Entities are objects with distinct identities that are defined by their attributes. Value Objects, on the other hand, are objects defined by their attributes but without a distinct identity. They represent concepts like dates, money, or geographical coordinates.

### **Aggregates**

Aggregates are groups of related Entities and Value Objects treated as a single unit. They enforce consistency and transactional boundaries within the domain.

### The Repository Pattern

The Repository pattern is a design pattern that acts as an abstraction layer between your domain model and the data access code. It provides a set of methods for querying and manipulating domain objects without exposing the underlying data store (such as a database or web service)


### Tactical Design Patterns

three fundamental building blocks: Entities, Value Objects, and Aggregates

###  **Entities**

Entities are objects with a distinct identity that runs through time and different states. Think of them as nouns in your domain. Entities are often the most critical elements within your domain model. They have a lifecycle and can change over time while preserving their identity.

In E-commerce customer is entites

### **Value Objects**

Value Objects represent concepts like dates, money, addresses, and more. They are immutable, meaning their attributes cannot change once set.

### **Aggregates**

are clusters of related Entities and Value Objects treated as a single unit. They ensure transactional consistency within a part of your domain. 
Example : An "Order" Aggregate could include the Order Entity as the Aggregate Root and related Order Items as Entities or Value Objects.


### Domain Services and Factories


## Chapter Four. Isolating the Domain


SRC : https://dev.to/ruben_alapont/domain-services-and-factories-in-domain-driven-design-55lo




https://thepaulrayner.com/blog/aggregates-and-entities-in-domain-driven-design/
https://github.com/ddd-by-examples/library
https://github.com/ddd-crew/free-ddd-learning-resources?tab=readme-ov-file#ddd-introductions-and-fundamentals

https://www.elbandit.co.uk/images/DDDEU-Booklet.pdf







Hexhongal arch
1. Bussines logic neeed to exsist center and other things need to arounf the bussiness logic








Topic related to that 
1. domain story telling
2. Events stroming and example mapping (kenny baas schwegler)
3. Domain quiz (zsofia)
4. DDD heuristic (heuristic -> enabling someone to discover or learn something for themselves.)
5. decision making heuristic 
6. 





resources
1. explore ddd conference





- you should talk with the subject matter or domain experts to gain a good understanding of the domai