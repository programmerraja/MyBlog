+++
title = 'Learning Domain-Driven Design'
date = 2024-03-27T08:16:39.3939+05:30
draft = true
tags =[]
+++ 


DDD can be divided into two parts: strategic and tactical

Strategic tools of DDD are used to analyze business domains and strategy, and to foster a shared understanding of the business between the different stakeholders
for answering `the questions of “what?” and “why?”—what software we are building and why we are building it.`

Tactical tools address a different aspect of communication issues. DDD’s tactical patterns allow us to write code in a way that reflects the business domain, addresses its goals, and speaks the language of the business.
`Tactical is all about the “how”—how each component is implemented`

## Strategic Design

#### Analyzing Business Domains

Business Domain: The main area of the company example `Starbucks is best known for its coffee.`

Subdomain:  The subdomain of the main business domain
Types 
- core -> Where bussiness core that make differ from other company
- generic -> the thing common for all business/software example implementing Authentication
- supporting -> Database landing page etc that help the core subdomain

#### Discovering Domain Knowledge

Ubiquitous Language

The Ubiquitous Language is a common, shared language that both technical and non-technical team members use to discuss the domain. It ensures that everyone understands the domain's terms and concepts.

`Use the model as the backbone of a language. Committhe team to exercising that language relentlessly in all communication within the team and in the code. Use the same language in diagrams, writing, and especially speech`


Managing Domain Complexity chapter 3 need to start