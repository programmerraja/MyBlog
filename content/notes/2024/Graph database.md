---
title : Graph database
date : 2024-06-15T07:52:04.044+05:30
draft : true
tags : 
---

## Neo4J

### Concepts

### **1. Nodes**

- **Definition:** Nodes are the basic entities or data points in a graph.
- **Usage:** Represent objects such as people, places, or events.
- **Properties:** Key-value pairs stored within nodes to add context (e.g., `{name: "Alice", age: 30}`).
- **Labels:** Categorize nodes, allowing filtering and organizing (e.g., `:Person`, `:Product`).
### **2. Relationships**

- **Definition:** Relationships define how nodes are connected and represent the interaction or link between nodes.
- **Directionality:** Relationships are directional (`->` or `<-`) but can be queried bidirectionally.
- **Types:** Each relationship has a type (e.g., `:FRIENDS_WITH`, `:PURCHASED`).
- **Properties:** Relationships can also store key-value pairs to provide context (e.g., `{since: 2020}`).

### **3. Properties**

- **Definition:** Metadata or data attached to nodes and relationships, stored as key-value pairs.
- **Example:**
    - Node property: `{name: "Neo", role: "The One"}`
    - Relationship property: `{weight: 0.8}`
### **4. Labels**

- **Definition:** Tags assigned to nodes for categorization.
- **Purpose:** Simplifies queries and indexing by grouping nodes of the same type.
- **Example:**

```cypher
   CREATE (n:Person {name: "Alice"})
```

- Where n is simply a **variable name** used to refer to nodes during the execution of the query
### **5. Cypher Query Language (CQL)**

- **Definition:** Declarative language used to interact with Neo4j.
- **Key Features:**
    - Create, Read, Update, Delete (CRUD) operations.
    - Match patterns in the graph.
    - Perform aggregations and path searches.

### **6. Paths**

- **Definition:** A sequence of nodes and relationships connecting them.
- **Purpose:** Enables complex queries like shortest paths, all paths, or custom traversals.
- **Example:**
    
    ```cypher
    MATCH p=(a:Person)-[:KNOWS*]->(b:Person)
    RETURN p
    ```

### **7. Indexes**

- **Definition:** Structures that optimize query performance.
- **Usage:** Created on node properties or relationships to improve lookup times.
- **Example:**
    
    ```cypher
    CREATE INDEX FOR (n:Person) ON (n.name)
    ```

### **8. Constraints**

- **Definition:** Rules to enforce data integrity.
- **Types:**
    - Uniqueness (`REQUIRE property IS UNIQUE`)
    - Existence (`REQUIRE property IS NOT NULL`)
- **Example:**
    
    ```cypher
    CREATE CONSTRAINT FOR (n:Person) REQUIRE n.id IS UNIQUE
    ```


### **9. Graph Algorithms**

- **Definition:** Built-in algorithms for analyzing and deriving insights from the graph.
- **Examples:**
    - **Pathfinding:** Shortest path between nodes.
    - **Centrality:** Identify influential nodes (e.g., PageRank).
    - **Community Detection:** Find clusters (e.g., Louvain).

### **10. Traversals**

- **Definition:** Navigating through the graph from one node to another using relationships.
- **Types:**
    - Depth-first search.
    - Breadth-first search.
- **Example:**
    
    ```cypher
    MATCH (n:Person)-[:FRIEND*1..3]->(friend)
    RETURN friend
    ```


### CheatSheet

#### Create Nodes:

```cypher
CREATE (n:Label {property: 'value', anotherProp: 123})
```

#### Create Relationships:

```cypher
CREATE (a:Person {name: 'Alice'})-[:KNOWS]->(b:Person {name: 'Bob'})
```

#### Match Nodes:

```cypher
MATCH (n:Label) RETURN n
```

#### Match Relationships:

```cypher
MATCH (a)-[r:RELATIONSHIP]->(b) RETURN a, r, b
```

#### Delete Nodes/Relationships:

```cypher
MATCH (n:Label) DELETE n
```

- **With Relationships:** Use `DETACH DELETE`.

```cypher
MATCH (n:Label) DETACH DELETE n
```

---

### **2. Reading Data**

#### Filtering:

```cypher
MATCH (n:Label {property: 'value'}) RETURN n
```

#### WHERE Clause:

```cypher
MATCH (n:Person)
WHERE n.age > 30
RETURN n
```

#### LIMIT & SKIP:

```cypher
MATCH (n:Person) RETURN n LIMIT 10 SKIP 5
```

---

### **3. Relationships**

#### Directed Relationships:

```cypher
MATCH (a)-[:RELATIONSHIP]->(b) RETURN a, b
```

#### Undirected Relationships:

```cypher
MATCH (a)-[:RELATIONSHIP]-(b) RETURN a, b
```

#### Variable-Length Paths:

```cypher
MATCH (a)-[:RELATIONSHIP*1..3]->(b) RETURN a, b
```

#### Path Variables:

```cypher
MATCH p=(a)-[:RELATIONSHIP*]->(b) RETURN p
```

---

### **4. Updating Data**

#### Set Properties:

```cypher
MATCH (n:Person {name: 'Alice'})
SET n.age = 25
```

#### Remove Properties:

```cypher
MATCH (n:Person {name: 'Alice'})
REMOVE n.age
```

#### Merge (Create or Match):

```cypher
MERGE (n:Person {name: 'Alice'})
ON CREATE SET n.createdAt = timestamp()
ON MATCH SET n.lastSeen = timestamp()
```

#### Count:

```cypher
MATCH (n:Person) RETURN COUNT(n)
```

#### Aggregate Functions:

```cypher
MATCH (n:Person)
RETURN AVG(n.age), MAX(n.age), MIN(n.age), SUM(n.age)
```

#### ORDER BY:

```cypher
MATCH (n:Person)
RETURN n.name, n.age
ORDER BY n.age DESC
```



