---
title: "PR Nightmares: 5 Common Coding Mistakes I've Seen in the Last 2 Years"
date: 2025-01-12T17:39:14.1414+05:30
draft: true
tags:
---

Hey everyone! If you're a regular follower of mine, you might already know a bit about me. But if not, let me quickly introduce myself. I’m a full-stack developer, and I’ve been working at a startup for the past two years. Over this time, I’ve reviewed countless pull requests and come across some recurring coding mistakes. Today, I’m excited to share these insights with you!


### Constant Variables: When to Avoid the Global Constant File

A common mistake I see is defining constants in a global file when they’re only used in one function. This pollutes the global scope and unnecessarily keeps the variable in memory.

#### Why It's a Problem

Defining a constant in a separate file for use in just one function doesn’t make sense and adds unnecessary complexity to the codebase. The key issue here is **polluting the global namespace**. When you define a constant globally, it’s available throughout the entire application and remains in memory, even if it’s not needed outside a very limited scope.
#### The Solution

Define constants **locally** within the function where they’re needed. Only move them to a separate file if they’re used across multiple files.

#### Example:

**Bad Code:**

```javascript
// constants.js
export const MAX_RETRIES = 3;

// In another file
import { MAX_RETRIES } from './constants';

function fetchData() {
    if (retries > MAX_RETRIES) {
        // handle error
    }
}
```

**Better Code:**

```javascript
function fetchData() {
    const MAX_RETRIES = 3; // Defined locally
    if (retries > MAX_RETRIES) {
        // handle error
    }
}
```

This keeps your code cleaner and more efficient.

### Environment Variables: Skip the Middleman

In Node.js, we typically store secrets in environment variables and access them with `process.env.KEY_NAME`. However, a common mistake I’ve seen is assigning the value of an environment variable to a constant, and then accessing it via that constant.

#### Why It’s a Problem

This adds unnecessary indirection. If you're just going to access `process.env.KEY_NAME` anyway, there's no need to assign it to a constant. It doesn’t improve the code, and only makes it harder to read and maintain.

#### The Solution

Access environment variables **directly** via `process.env.KEY_NAME` instead of assigning them to a constant unless the value needs to be used multiple times in a large scope.

#### Example:

**Bad Code:**

```javascript
const DB_HOST = process.env.DB_HOST;
const DB_PORT = process.env.DB_PORT;

function connect() {
    console.log(DB_HOST, DB_PORT);
}
```

**Better Code:**

```javascript
function connect() {
    console.log(process.env.DB_HOST, process.env.DB_PORT);
}
```

This keeps the code cleaner and reduces unnecessary assignments.

### Logs Without Indentation: Clean Up Your Debugging

During PR reviews, I often come across logs that are useful for debugging but lack proper indentation or context. Logs without structure make it harder to extract meaningful insights.

#### Why It’s a Problem

Logs without context or proper formatting can clutter production logs, making it difficult to search and analyze them. Without a consistent structure, finding relevant information becomes a nightmare.

#### The Solution

Log **only when necessary** and ensure logs are **structured** with relevant context. Use consistent formatting (e.g., timestamps, log levels) to make logs easier to read and search.

#### Example:

**Bad Code:**

```javascript
function fetchData(username){
	try {
	    //some api call
	} catch(err){
	   console.log("error",err);
	}

}```

**Better Code:**

```javascript
function fetchData(username){
	try {
		//some api call
	} catch(err){
		console.error(`[fetchData] [ERROR] Error in fetching data for username ${username}`,err);
	}
}
```
