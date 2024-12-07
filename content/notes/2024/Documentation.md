---
title : Documentation
date : 2024-06-28T19:48:00.000+05:30
draft : true
tags : 
---


## Architectural Decisions Records

 ADRs provide a template for documenting decisions, including the context, constraints, and consequences of each decision.
```
# ADR: Decision to Use a Microservices Architecture

## Context
We are building a new e-commerce platform that requires scalability and flexibility.

## Decision
We will use a microservices architecture to allow for independent development and deployment of each service.

## Constraints
* We need to ensure high availability and scalability.
* We need to minimize dependencies between services.

## Consequences
* We will need to invest in additional infrastructure and tooling to support microservices.
* We will need to develop new skills and expertise in areas such as service discovery and communication.

## Alternatives
* We considered using a monolithic architecture, but it would not provide the same level of scalability and flexibility.
```


##  Brag document

Brag document is a document where individuals record that we achievements, accomplishments, skills, and positive feedback we received.

Template

```
 ### Goals for this year:
	- List your major goals here! Sharing your goals with your manager & coworkers is really nice because it helps them see how they can support you in accomplishing those goals!


### Projects
For each one, go through:

- What your contributions were (did you come up with the design? Which components did you build? Was there some useful insight like “wait, we can cut scope and do what we want by doing way less work” that you came up with?)

- The impact of the project – who was it for? Are there numbers you can attach to it? (saved X dollars? shipped new feature that has helped sell Y big deals? Improved performance by X%? Used by X internal users every day?). Did it support some important non-numeric company goal (required to pass an audit? helped retain an important user?)

Remember: don’t forget to explain what the results of you work actually were! It’s often important to go back a few months later and fill in what actually happened after you launched the project.

### Collaboration & mentorship

Examples of things in this category:

- Helping others in an area you’re an expert in (like “other engineers regularly ask me for one-off help solving weird bugs in their CSS” or “quoting from the C standard at just the right moment”)
- Mentoring interns / helping new team members get started
- Writing really clear emails/meeting notes
- Foundational code that other people built on top of
- Improving monitoring / dashboards / on call
- Any code review that you spent a particularly long time on / that you think was especially important
- Important questions you answered (“helped Risha from OTHER_TEAM with a lot of questions related to Y”)
- Mentoring someone on a project (“gave Ben advice from time to time on leading his first big project”)
- Giving an internal talk or workshop

### Design & documentation

List design docs & documentation that you worked on

- Design docs: I usually just say “wrote design for X” or “reviewed design for X”
- Documentation: maybe briefly explain the goal behind this documentation (for example “we were getting a lot of questions about X, so I documented it and now we can answer the questions more quickly”)

### Company building

This is a category we have at work – it basically means “things you did to help the company overall, not just your project / team”. Some things that go in here:

- Going above & beyond with interviewing or recruiting (doing campus recruiting, etc)
- Improving important processes, like the interview process or writing better onboarding materials

### What you learned

My friend Julian suggested this section and I think it’s a great idea – try listing important things you learned or skills you’ve acquired recently! Some examples of skills you might be learning or improving:

- how to do performance analysis & make code run faster
- internals of an important piece of software (like the JVM or Postgres or Linux)
- how to use a library (like React)
- how to use an important tool (like the command line or Firefox dev tools)
- about a specific area of programming (like localization or timezones)
- an area like product management / UX design
- how to write a clear design doc
- a new programming language

It’s really easy to lose track of what skills you’re learning, and usually when I reflect on this I realize I learned a lot more than I thought and also notice things that I’m _not_ learning that I wish I was.

### Outside of work

It’s also often useful to track accomplishments outside of work, like:

- blog posts
- talks/panels
- open source work
- Industry recognition
- 
```


## C4 Model

The C4 model is a hierarchical way to visualize the architecture of a software system. It consists of four levels of diagrams:

- Context (Level 1): This high-level diagram shows the system in its environment, including users and other interacting systems.
- Containers (Level 2): This diagram zooms into the system, showing the major components (applications, data stores, etc.) and their interactions.
- Components (Level 3): This level focuses on the internal structure of a specific container, showing the components within it and their relationships.
- Code (Level 4): This level details the code-level implementation of components.


## DORA Metrics

The four key DORA metrics are:

- Deployment Frequency: How often code is deployed to production.
- Lead Time for Changes: The time it takes for a code change to go from commit to production.
- Change Failure Rate: The percentage of deployments that result in a failure or rollback.
- Time to Restore Service: The time it takes to restore service after an incident or outage.


