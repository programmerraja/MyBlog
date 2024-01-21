+++
title = 'Software principles'
date = 2024-01-21T10:14:59.5959+05:30
draft = true
tags =[]
+++ 

## Stable Dependency Principle

**Stable components:** are components that aren't expected to change that often.

**Volatile components:** are ones that are more likely to jitter and require frequent changes.

it's important to know when a component is volatile and ensure that we don't make stable components depend on them.

##  Responsibility-Driven Design
Start with a requirement (functional or non-functional), convert it into responsibilities, assign those to roles, then find the collaborations.

TODO
1. https://khalilstemmler.com/letters/3-steps-to-solve-most-design-problems/
2. https://khalilstemmler.com/articles/the-phases-of-craftship/code-first/
3. 