---
title: "Lessons from 2 Years of PR Reviews: 9 Costly Coding Mistakes"
date: 2025-01-12T17:39:14.1414+05:30
draft: false
tags:
---

Hey everyone! If you're a regular follower, you might already know a bit about me. But if not, let me quickly introduce myself: I’m a full-stack developer, and I’ve been working at a startup for the past two years. During this time, I’ve reviewed countless pull requests and noticed recurring coding mistakes. Today, I’m excited to share these insights with you. This will be focused on Node.js.

## 1. Constant Variables: When to Avoid the Global Constant File

A common mistake I see is defining constants in a global file when they’re only used in one function. This pollutes the global scope and unnecessarily keeps the variable in memory.

#### Why It's a Problem

Defining a constant in a separate file for use in just one function doesn’t make sense and adds unnecessary complexity to the codebase. The key issue here is **polluting the global namespace**. When you define a constant globally, it’s available throughout the entire application and remains in memory, even if it’s not needed outside a very limited scope.
#### The Solution

Define constants **locally** within the function where they’re needed. Only move them to a separate file if they’re used across multiple files.

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

## 2.  Environment Variables: Skip the Middleman

In Node.js, we typically store secrets in environment variables and access them with `process.env.KEY_NAME`. However, a common mistake I’ve seen is assigning the value of an environment variable to a constant, and then accessing it via that constant.

#### Why It’s a Problem

This adds unnecessary indirection. If you're just going to access `process.env.KEY_NAME` anyway, there's no need to assign it to a constant. It doesn’t improve the code, and only makes it harder to read and maintain.

#### The Solution

Access environment variables **directly** via `process.env.KEY_NAME` instead of assigning them to a constant unless the value needs to be used multiple times in a large scope.

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

## 3. Logs Without Indentation: Clean Up Your Debugging

 I often come across logs that are useful for debugging but lack proper indentation or context. Logs without structure make it harder to extract meaningful insights.

#### Why It’s a Problem

Logs without context or proper formatting can clutter production logs, making it difficult to search and analyze them. Without a consistent structure, finding relevant information becomes a nightmare.

#### The Solution

Log **only when necessary** and ensure logs are **structured** with relevant context. Use consistent formatting (e.g., timestamps, log levels) to make logs easier to read and search.

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

## 5. Handling Independent Async Calls: Run Promises in Parallel

Let say we have function which job is to fetch data from two service code will look like as below in nodejs 

```javascript
const serviceOneData = await fetch()
const serviceTwoData = await fetch()
```

Would you able to find the problem in the code ?

**Can You Spot the Issue?**

The problem here is that we’re waiting for each `fetch()` call to complete **sequentially**, meaning the second call won’t start until the first one finishes. This is inefficient, especially if these calls are independent of each other.

#### Why It’s a Problem

Executing asynchronous calls in sequence means you're **blocking** the execution, and the overall process takes longer than necessary. Since the two calls don’t depend on each other, there’s no reason to wait for one before starting the other.
#### The Solution

To improve efficiency, we can run both asynchronous calls **in parallel** using `Promise.all()`. This allows both requests to be processed simultaneously, reducing overall waiting time.

```javascript

const [serviceOneData, serviceTwoData] = await Promise.all([
    fetch('serviceOne'),
    fetch('serviceTwo')
]);

```

## 6. Trusting the Frontend

When performing CRUD operations, the frontend typically sends data as an object when creating or updating a document. On the backend, the data is spread and used directly to create or update the document. This technique is commonly used for both creation and updates.

#### Why It’s a Problem

This approach can be risky because users could potentially manipulate the frontend and send data that shouldn't be updated, or update fields that are not intended to be changed.

For example, imagine a collection with a field `ownedBy` that specifies the owner of a document. If this field is not meant to be updated once the document is created, but a user sends it in an update request, using the spread operator (`...req.body`) could unintentionally update the `ownedBy` field.

```javascript
db.findOneAndUpdate({}, {...req.body})
```

#### The Solution

To prevent this issue, don’t blindly trust the frontend. There are two primary ways to solve this problem:

1. **Strict Validation:** Add strict validation to check if the updated keys are allowed. If any unexpected fields are found in the frontend payload, reject the request.

2. **Avoid Using the Spread Operator:** Instead of using the spread operator to update documents, destructure the object to ensure that only the allowed fields are included in the update operation.

## 7. Ingoring comments

Have you ever encountered code that’s difficult to understand just by reading it? It may look vague, or the business logic behind it might be unclear, leaving you unsure about why it was written a certain way. This can be especially challenging when you’re trying to maintain or extend the code, as the original intent is lost without proper explanations.

When code lacks comments, future developers (including your future self) might waste time trying to figure out the reasoning behind specific logic, which can slow down development and increase the risk of introducing bugs.

Take a look at the following JavaScript function:

```js
function getMatchingQuery(initialMatchQuery, value) {
    const durationMap = {
        lessThanThirtySeconds: { $gt: 0, $lte: 29 },
        betweenThirtySecondsAndOneMinute: { $gt: 29, $lte: 60 },
        betweenOneAndFive: { $gt: 60, $lte: 300 },
        betweenFiveAndTen: { $gt: 300, $lte: 600 },
        greaterThanTen: { $gt: 600 }, 
    };

    if (value && Array.isArray(value)) {
        initialMatchQuery = {
            ...initialMatchQuery,
            duration: { $gt: 0 },
        };

        if (value.length < 5) {
            delete initialMatchQuery.duration;
            value.sort((a, b) => a.key - b.key);
            const mergedIntervals = [];
            let currentInterval = { ...durationMap[value[0].id] };
            for (let i = 1; i < value.length; i++) {
                const range = durationMap[value[i].id];
                if (currentInterval.$lte && currentInterval.$lte === range.$gt) {
                    if (range.$lte) {
                        currentInterval.$lte = range.$lte;
                    } else {
                        delete currentInterval.$lte;
                    }
                } else {
                    mergedIntervals.push(currentInterval);
                    currentInterval = { ...range };
                }
            }
        }
    }
    return initialMatchQuery;
}

```

At first glance, understanding this function isn’t straightforward. Without any comments, it's hard to determine its purpose and logic. Imagine a developer trying to debug or modify this function without any context—this could lead to frustration, wasted time, and potentially breaking the functionality.

A simple yet effective way to make this code more maintainable is by adding meaningful comments. Comments act as a guide, explaining why certain decisions were made and making it easier for others (and your future self) to work with the code.
## 8. Handling Atomic operation  in code level

When implementing logic that modifies user credits, it’s common to reduce the credit balance each time a specific action is performed, as shown in the following code snippet:

```
const userCredit = user.credit
db.user.update(
	{ username: user.username },
	{ $set: { credit: userCredit - 5 } }
);
```

#### Why It’s a Problem

This approach can lead to issues when the same user accesses their account from multiple devices simultaneously. In such cases, both devices could initiate a credit deduction, and because the code reads the credit balance before updating it, both case might get the same initial value (for example, 10 credits). As a result, both devices would try to reduce the credit by 5, leaving only 5 credits instead of the expected 0.
#### The Solution

To prevent this issue, use the database’s atomic operators. By using the `$inc` operator, the credit deduction becomes an atomic operation that ensures the update is applied correctly, even in concurrent scenarios:

```
db.user.update( { username: user.username }, { $inc: { credit: -5 } } 
```

This method guarantees that the credit will be correctly decremented by 5 credits, regardless of how many devices or processes are attempting to update the user’s credit at the same time. This ensures consistency and integrity in the database, even under concurrent load.

If you would like to learn more about ACID properties in DB check out here

## 9. Overprotecting Your Code:

I’ve noticed that many developers overcomplicate their code by adding excessive safeguards, which often goes against the fundamental principle that code should work as intended. Overthinking and over-safeguarding the code can lead to unnecessary complexity and inefficiency.

Let me explain with an example:

```js
try {
  // Business logic here
  db.user.update({ username: username }, { $set: { credit: 100 } });
} catch (err) {
  console.log("Failed to update", err);
}
```

In this code, the database update is performed to set a user’s credit to 100. Most of the time, the database is operational, so the likelihood of a failure is minimal. Therefore, in this case, there is often no need for a `try-catch` block if a global error handler is in place.

You should use a `try-catch` block only when you need to perform specific business logic in response to a failure or if the error requires special handling. Otherwise, a global error handler is sufficient.

```js
try{
  // Business logic here
  db.user.update({ username: username }, { $set: { credit: 100 } });
} catch (err){
	//you going to do the business logic in response to a failure of db operation 
}
```

Another anti-pattern I’ve observed is throwing an error after catching it, like this:

```js
try {
  // some code
} catch (err) {
  console.log("err", err);
  throw new Error(err.message);
}
```

In this case, the `try-catch` block is redundant. You can simply remove the `try-catch` and allow the error to propagate naturally, as the error is being logged and then re-thrown, which doesn’t add any value.

### Open to Feedback

Growth comes from continuous learning, and staying open to feedback is key to improvement. If you have a different perspective, feel free to share I appreciate thoughtful discussions. Keep coding, keep growing!
