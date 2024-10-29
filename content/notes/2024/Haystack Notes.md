---
title : Haystack
date : 2024-08-24T09:32:35.3535+05:30
draft : true
tags : 
---

## Haystack

Haystack provides all the tools you need to build a custom RAG pipelines with LLMs that works for you. This includes everything from prototyping to deployment.

Key Concepets
- Components
- Generators : generating text responses after you give them a prompt.
- Retrievers : go through all the documents in a Document Store, select the ones that match the user query.
	- Data Classes: to carry the data through the system. The data classes are mostly likely to appear as inputs or outputs of your pipelines. example `Document` and `Answer`
- Document Stores:  is an object that stores your documents in Haystack,
- Pipelines : combine various components, Document Stores, and integrations in to pipelines

