---
title: Software principles
date: 2024-01-21T10:14:59.5959+05:30
draft: false
tags:
  - software_architecture
---

## Stable Dependency Principle

**Stable components:** are components that aren't expected to change that often.

**Volatile components:** are ones that are more likely to jitter and require frequent changes.

it's important to know when a component is volatile and ensure that we don't make stable components depend on them.

##  Responsibility-Driven Design

Start with a requirement (functional or non-functional), convert it into responsibilities, assign those to roles, then find the collaborations.

## The 6 Stereotypes[](https://khalilstemmler.com/articles/object-oriented/design/object-stereotypes/#The-6-Stereotypes)

1. **The Information Holder**
2. **The Structurer**
3. **The Service Provider**
4. **The Coordinator**
5. **The Controller**
6. **The Interfacer**
TODO
1. https://khalilstemmler.com/letters/3-steps-to-solve-most-design-problems/
2. https://khalilstemmler.com/articles/the-phases-of-craftship/code-first/
3. [A collection of learning resources for curious software engineers ](https://github.com/charlax/professional-programming)



Blog for Software principles
1. https://khalilstemmler.com/ 


## Clean code


## Hexgonal architecture

write bussiness logic in centerk


## Reactive Architecture

A group of people came together in 2014 and publish a spesification to define the term called Reactive Manifesto. There are 4 principles in the context of Reactive Systems.
- **Responsive** : The system has to respond quickly to all users under all conditions.
- **Resilient**: The system stays responsive in the face of [failure](https://www.reactivemanifesto.org/glossary#Failure).
- **Elastic**: The system should provide responsiveness, despite increases(or decreases) in load. Scaling up provides responsiveness during peak, while scaling down improves cost effectiveness.
- **Message Driven**: Reactive Systems rely on asynchronous message-passing to establish a boundary between components that ensures loose coupling, isolation and location transparency.

#### Actor Model

It’s a programming paradigm that supports construction of Reactive Systems.It is Message Driven and provide Elasticity and Resilience. So we can use it to build Responsive software. Example [Akka](https://akka.io/)


### **Responsibility-Driven Design (RDD)**
RDD is a software design methodology that emphasizes the assignment of responsibilities to objects or components in a system. The primary idea behind RDD is that each object (or component) should be responsible for performing certain tasks and managing certain aspects of the system’s behavior. In this context, **responsibility** refers to the **behavior** of an object, as well as the **data** it holds.

- **Key Concepts:**
    - Objects are designed by assigning responsibilities to them.
    - Design focuses on what each object _does_ (its responsibility) rather than what it _is_ (its state).
    - RDD stresses interaction between objects. When objects collaborate, they share responsibilities, which helps in defining clear boundaries and reducing complexity.
    - The design process involves answering questions like "What is this object responsible for?" and "How does it collaborate with other objects to fulfill its responsibilities?"
- **Focus**: RDD focuses on defining and assigning responsibilities to objects, fostering clarity in object collaboration and behavior.

Stereotypes in RDD
1. **The Information Holder**
2. **The Structurer**
3. **The Service Provider**
4. **The Coordinator**
5. **The Controller**
6. **The Interfacer**
- https://khalilstemmler.com/articles/3-steps-to-solve-most-design-problems/