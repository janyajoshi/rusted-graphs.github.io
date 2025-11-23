# Rusted Graphs

A lightweight, encrypted graph database built with Rust.

Rusted Graphs is a small, embeddable graph database written in Rust. It stores all graph data — collections, nodes, and edges — in encrypted form and exposes a simple, ergonomic API for creating collections, inserting nodes, updating and querying them.

## Table of Contents

- [Installation](#installation)
- [Getting Started](#getting-started)
- [Collection Management](#collection-management)
- [Node Operations](#node-operations)
- [Edge Operations](#edge-operations)
- [Querying Nodes](#querying-nodes)
- [Bulk Operations](#bulk-operations)

---

## Installation

### Python

```bash
pip install rusted_graphs_python
```

### JavaScript

```bash
npm install rusted_graphs_js
```

---

## Getting Started

### Python

```python
import rusted_graphs_python as rgp

# Initialize the database
rgp.init()
```

### JavaScript

```javascript
const rg = require("rusted_graphs_js");

// Initialize the database
rg.init();
```

---

## Collection Management

Collections are containers for your graph data. Each collection holds nodes and edges independently.

### Create a Collection

#### Python

```python
collectionName = "people"
rgp.create_collection(collectionName)
```

#### JavaScript

```javascript
let collectionName = "people";
rg.createCollection(collectionName);
```

### Delete a Collection

#### Python

```python
rgp.delete_collection(collectionName)
```

#### JavaScript

```javascript
rg.deleteCollection(collectionName);
```

### Clear a Collection

Removes all nodes and edges from a collection without deleting the collection itself.

#### Python

```python
rgp.clear_collection(collectionName)
```

#### JavaScript

```javascript
rg.clearCollection(collectionName);
```

### Inspect Collections

View all collections and their contents.

#### Python

```python
rgp.lookup()
```

#### JavaScript

```javascript
rg.lookup();
```

---

## Node Operations

Nodes represent entities in your graph. Each node requires a unique `label` identifier.

### Add Nodes

#### Python

```python
rgp.add_nodes(collectionName, [
    {"label": "p1", "type": "people"},
    {"label": "p2", "type": "people"}
])
```

#### JavaScript

```javascript
rg.addNodes(collectionName, [
  { label: "p1", type: "people" },
  { label: "p2", type: "people" },
]);
```

### Update a Node

Updates merge new properties with existing ones.

#### Python

```python
rgp.update_node(collectionName, "p2", {
    "label": "p2",
    "type": "people",
    "isUpdated": True
})
```

#### JavaScript

```javascript
rg.updateNode(collectionName, "p2", {
  label: "p2",
  type: "people",
  isUpdated: true,
});
```

### Remove Nodes

#### Python

```python
rgp.remove_nodes(collectionName, ["p1"])
```

#### JavaScript

```javascript
rg.removeNodes(collectionName, ["p1"]);
```

---

## Edge Operations

Edges represent relationships between nodes. Use node labels to define connections.

### Add Edges

#### Python

```python
rgp.add_edges(collectionName, [
    {
        "from": "p1",
        "to": "p2",
        "data": {"label": "colleague", "timeInYears": 3}
    }
])
```

#### JavaScript

```javascript
rg.addEdges(collectionName, [
  {
    from: "p1",
    to: "p2",
    data: { label: "colleague", timeInYears: 3 },
  },
]);
```

### Remove an Edge

#### Python

```python
rgp.remove_edge(collectionName, "p1", "p2")
```

#### JavaScript

```javascript
rg.removeEdge(collectionName, "p1", "p2");
```

---

## Querying Nodes

Query nodes using MongoDB-style conditional expressions. Queries are written as strings with operator functions.

### Supported Operators

- `Eq(path, value)` - Equal to
- `Ne(path, value)` - Not equal to
- `Lt(path, value)` - Less than
- `Lte(path, value)` - Less than or equal to
- `Gt(path, value)` - Greater than
- `Gte(path, value)` - Greater than or equal to
- `In(path, [value1, value2, ...])` - Value in array

### Query Syntax

Queries use dot notation to access nested properties. Use `data.` prefix to query node data fields.

#### Basic Query Examples

**Find nodes where age is greater than 30:**

##### Python

```python
matched_nodes = rgp.query_node(collectionName, "Gt(data.age, 30)")
print(matched_nodes[0])
```

##### JavaScript

```javascript
matched_nodes = rg.queryNode(collectionName, "Gt(data.age, 30)");
console.log(matched_nodes[0]);
```

**Find nodes with specific type:**

##### Python

```python
matched_nodes = rgp.query_node(collectionName, "Eq(type, 'people')")
```

##### JavaScript

```javascript
matched_nodes = rg.queryNode(collectionName, "Eq(type, 'people')");
```

**Find nodes with name in a list:**

##### Python

```python
matched_nodes = rgp.query_node(collectionName, "In(data.name, ['Alice', 'Bob', 'Charlie'])")
```

##### JavaScript

```javascript
matched_nodes = rg.queryNode(
  collectionName,
  "In(data.name, ['Alice', 'Bob', 'Charlie'])"
);
```

### Complex Queries with Logical Operators

Combine conditions using `And` and `Or` operators.

**Find nodes where age > 30 AND type is 'people':**

##### Python

```python
matched_nodes = rgp.query_node(
    collectionName,
    "And(Gt(data.age, 30), Eq(type, 'people'))"
)
```

##### JavaScript

```javascript
matched_nodes = rg.queryNode(
  collectionName,
  "And(Gt(data.age, 30), Eq(type, 'people'))"
);
```

**Find nodes where age < 25 OR age > 60:**

##### Python

```python
matched_nodes = rgp.query_node(
    collectionName,
    "Or(Lt(data.age, 25), Gt(data.age, 60))"
)
```

##### JavaScript

```javascript
matched_nodes = rg.queryNode(
  collectionName,
  "Or(Lt(data.age, 25), Gt(data.age, 60))"
);
```

### Querying Nested Properties

Access nested object properties using dot notation.

##### Python

```python
# Query nested property: data.address.city
matched_nodes = rgp.query_node(collectionName, "Eq(data.address.city, 'New York')")
```

##### JavaScript

```javascript
// Query nested property: data.address.city
matched_nodes = rg.queryNode(
  collectionName,
  "Eq(data.address.city, 'New York')"
);
```

### Query Array Elements

Access array elements by index.

##### Python

```python
# Query first element of tags array
matched_nodes = rgp.query_node(collectionName, "Eq(data.tags.0, 'important')")
```

##### JavaScript

```javascript
// Query first element of tags array
matched_nodes = rg.queryNode(collectionName, "Eq(data.tags.0, 'important')");
```

---

## Bulk Operations

Load nodes and edges from JSON files for efficient bulk imports.

### File Format

**nodes.json:**

```json
[
  {
    "label": "person1",
    "type": "people",
    "data": { "age": 35, "name": "Alice" }
  },
  { "label": "person2", "type": "people", "data": { "age": 28, "name": "Bob" } }
]
```

**edges.json:**

```json
[
  {
    "from": "person1",
    "to": "person2",
    "data": { "label": "friend", "since": 2020 }
  }
]
```

### Add from Files

#### Python

```python
rgp.add_nodes_from_file(collectionName, "/path/to/nodes.json")
rgp.add_edges_from_file(collectionName, "/path/to/edges.json")
```

#### JavaScript

```javascript
rg.addNodesFromFile(collectionName, "./path/to/nodes.json");
rg.addEdgesFromFile(collectionName, "./path/to/edges.json");
```

---

## Complete Example

### Python

```python
import rusted_graphs_python as rgp
from pprint import pprint

# Initialize
rgp.init()
collectionName = "social_network"

# Create collection
rgp.create_collection(collectionName)

# Add nodes
rgp.add_nodes(collectionName, [
    {"label": "alice", "type": "person", "data": {"age": 32, "city": "NYC"}},
    {"label": "bob", "type": "person", "data": {"age": 28, "city": "LA"}},
    {"label": "charlie", "type": "person", "data": {"age": 35, "city": "NYC"}}
])

# Add edges
rgp.add_edges(collectionName, [
    {"from": "alice", "to": "bob", "data": {"relationship": "friend"}},
    {"from": "alice", "to": "charlie", "data": {"relationship": "colleague"}}
])

# Query nodes older than 30
older_people = rgp.query_node(collectionName, "Gt(data.age, 30)")
pprint(older_people)

# Query people in NYC
nyc_people = rgp.query_node(collectionName, "Eq(data.city, 'NYC')")
pprint(nyc_people)
```

### JavaScript

```javascript
const rg = require("rusted_graphs_js");

// Initialize
rg.init();
let collectionName = "social_network";

// Create collection
rg.createCollection(collectionName);

// Add nodes
rg.addNodes(collectionName, [
  { label: "alice", type: "person", data: { age: 32, city: "NYC" } },
  { label: "bob", type: "person", data: { age: 28, city: "LA" } },
  { label: "charlie", type: "person", data: { age: 35, city: "NYC" } },
]);

// Add edges
rg.addEdges(collectionName, [
  { from: "alice", to: "bob", data: { relationship: "friend" } },
  { from: "alice", to: "charlie", data: { relationship: "colleague" } },
]);

// Query nodes older than 30
let olderPeople = rg.queryNode(collectionName, "Gt(data.age, 30)");
console.log(olderPeople);

// Query people in NYC
let nycPeople = rg.queryNode(collectionName, "Eq(data.city, 'NYC')");
console.log(nycPeople);
```

---

## Best Practices

1. **Unique Labels**: Ensure each node has a unique label within a collection
2. **Data Structure**: Store complex node attributes under the `data` property
3. **Query Performance**: Use specific queries instead of fetching all nodes and filtering in-memory
4. **Bulk Operations**: Use file-based imports for large datasets
5. **Collection Organization**: Group related graphs in separate collections for better organization
