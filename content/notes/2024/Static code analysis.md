---
title : CodeQL
date : 2024-09-28T06:39:28.2828+05:30
draft : true
---

Static code analysis is the process of examining source code without actually executing it. This type of analysis helps in detecting potential errors, vulnerabilities, coding standard violations, and performance issues early in the development cycle. Static analysis tools automatically review the code to find issues like:

- **Syntax errors**: Mistakes in the structure of the code.
- **Coding standard violations**: Ensures adherence to predefined coding guidelines (e.g., consistent naming conventions).
- **Security vulnerabilities**: Identifies potential security risks like SQL injection or buffer overflow.
- **Performance issues**: Finds inefficient code that may cause performance bottlenecks.
- **Dead code**: Code that is never used or executed.


## CodeQL

CodeQL enables you to query code as though it were data. Write a query to find all variants of a vulnerability, eradicating it forever. Then share your query to help others do the same.

Working with **CodeQL** involves setting up the tool, creating or running predefined queries, and analyzing the results. Here’s a step-by-step guide to get started with **CodeQL**:

Discover vulnerabilities across a codebase with CodeQL, our industry-leading semantic code analysis engine. CodeQL lets you query code as though it were data. Write a query to find all variants of a vulnerability, eradicating it forever. Then share your query to help others do the same. 
https://codeql.github.com/

### 1. **Install CodeQL CLI**
The CodeQL CLI (Command-Line Interface) is required to create and analyze databases from your source code. Follow these steps to install it:

- **Download CodeQL CLI**: Get it from the official [GitHub CodeQL releases page](https://github.com/github/codeql-cli-binaries/releases).
- **Unzip the download** and move the `codeql` binary to your system’s PATH.


### 2. **Initialize a CodeQL Database**
You’ll need to create a **CodeQL database** for the code you want to analyze. This database is a representation of your source code that CodeQL can query.

```bash
# Create a CodeQL database for a project (specify the language)
codeql database create codeql-db --language=javascript
```

This command will analyze the code and store it in a CodeQL database, which can later be queried.

### 3. **Run Queries**
Once you have the database, you can either run predefined queries or create your own. GitHub provides a library of **CodeQL queries** for security and code quality. You can clone the query repository:

```bash
git clone https://github.com/github/codeql.git
```

Now, run a query on your database:

```bash
# Run a query (replace <query-file> with the actual query path)
codeql query run codeql-repo/javascript/ql/src/Security/CWE/CWE-079/ReflectedXss.ql --database=codeql-db
```

This will execute the query and return any potential issues in the code.

### 4. **View Results**
Results of CodeQL queries can be exported in various formats (JSON, SARIF) for detailed analysis:

```bash
# Run the query and output the results in SARIF format (useful for GitHub Code Scanning)
codeql query run codeql-repo/javascript/ql/src/Security/CWE/CWE-079/ReflectedXss.ql --database=codeql-db --format=sarif > result.sarif
```

SARIF (Static Analysis Results Interchange Format) is a standardized output format that you can upload to platforms like GitHub for easier viewing.

### 5. **Custom Queries (Optional)**
You can write your own **custom queries** in CodeQL. The language is declarative and SQL-like, allowing you to search for patterns in your code.

Here’s an example of a simple query that finds unused variables in JavaScript:

```ql
import javascript

from Variable v
where not exists (v.getAnAccess())
select v, "Unused variable."
```

To run this query:

```bash
codeql query run custom-query.ql --database=codeql-db
```




## Semgrep

Lightweight static analysis for many languages. Find bug variants with patterns that look like source code.
https://github.com/semgrep/semgrep



## Jorens

An open-source platform for static code analysis using code property graphs.

CMDs
- Creating Code Property Graphs

```
jorens cpg --language <language> --input <path_to_source_code>
```

- Running Static Analysis
```
jorens analyze --cpg <path_to_cpg> --rules <path_to_rules>
```


## sonarJs

https://github.com/SonarSource/SonarJS
## espree

## GritQL

[GritQL](https://github.com/getgrit/gritql) is a query language for searching, linting, and modifying code.


## Glean 

System for collecting, deriving and querying facts about source code
From facebook
[Get Started](https://glean.software/docs/introduction/) 


## Codemod

https://codemod.com/


https://codescene.com/
https://snyk.io/platform/deepcode-ai/
## Resources
- https://github.com/analysis-tools-dev/static-analysis?tab=readme-ov-file 