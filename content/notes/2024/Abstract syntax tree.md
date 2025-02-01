---
title : Abstract syntax tree
date : 2024-06-12T08:32:54.5454+05:30
draft : true
tags : 
---


```js
function add(a, b) {
  return a + b;
}

```

AST

```json
{
  "type": "FunctionDeclaration",
  "id": { "type": "Identifier", "name": "add" },
  "params": [
    { "type": "Identifier", "name": "a" },
    { "type": "Identifier", "name": "b" }
  ],
  "body": {
    "type": "BlockStatement",
    "body": [
      {
        "type": "ReturnStatement",
        "argument": {
          "type": "BinaryExpression",
          "operator": "+",
          "left": { "type": "Identifier", "name": "a" },
          "right": { "type": "Identifier", "name": "b" }
        }
      }
    ]
  }
}

```

Resources
- https://github.com/babel/babel/blob/main/packages/babel-parser/ast/spec.md
- https://astexplorer.net/
- https://github.com/tree-sitter/tree-sitter


