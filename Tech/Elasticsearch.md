- [[Tech Fundamentals]]

## Overview
- Distributed search and analytics engine
- Built on Apache Lucene
- JSON document store
- Real-time search and analytics
- RESTful API

## Core Concepts

### Index
- Collection of documents (like database)
- Similar documents grouped together
- Example: `users`, `products`, `orders`

### Document
- JSON object stored in index
- Basic unit of information
- Has unique ID

### Type (Deprecated)
- Removed in Elasticsearch 7.x+
- All documents in index are same type

### Mapping
- Schema definition for documents
- Defines field types and properties
- Dynamic or explicit

### Shards
- Index split into multiple shards
- Distributed across nodes
- Enables horizontal scaling

### Replicas
- Copy of shard
- Provides high availability
- Improves read performance

## Basic Operations

### Create Index
```bash
PUT /products
{
  "settings": {
    "number_of_shards": 1,
    "number_of_replicas": 1
  },
  "mappings": {
    "properties": {
      "name": { "type": "text" },
      "price": { "type": "double" },
      "category": { "type": "keyword" }
    }
  }
}
```

### Index Document (Create/Update)
```bash
# Auto-generate ID
POST /products/_doc
{
  "name": "Laptop",
  "price": 999.99,
  "category": "Electronics"
}

# Specify ID
PUT /products/_doc/1
{
  "name": "Laptop",
  "price": 999.99,
  "category": "Electronics"
}
```

### Get Document
```bash
GET /products/_doc/1
```

### Update Document
```bash
POST /products/_update/1
{
  "doc": {
    "price": 899.99
  }
}
```

### Delete Document
```bash
DELETE /products/_doc/1
```

### Delete Index
```bash
DELETE /products
```

## Search Queries

### Match Query (Full-text search)
```bash
GET /products/_search
{
  "query": {
    "match": {
      "name": "laptop"
    }
  }
}
```

### Term Query (Exact match)
```bash
GET /products/_search
{
  "query": {
    "term": {
      "category": "Electronics"
    }
  }
}
```

### Bool Query (Combining queries)
```bash
GET /products/_search
{
  "query": {
    "bool": {
      "must": [
        { "match": { "name": "laptop" } }
      ],
      "filter": [
        { "range": { "price": { "gte": 500, "lte": 1000 } } }
      ],
      "must_not": [
        { "term": { "status": "discontinued" } }
      ]
    }
  }
}
```

### Range Query
```bash
GET /products/_search
{
  "query": {
    "range": {
      "price": {
        "gte": 100,
        "lte": 500
      }
    }
  }
}
```

### Wildcard Query
```bash
GET /products/_search
{
  "query": {
    "wildcard": {
      "name": "lap*"
    }
  }
}
```

### Aggregations
```bash
GET /products/_search
{
  "size": 0,
  "aggs": {
    "avg_price": {
      "avg": { "field": "price" }
    },
    "by_category": {
      "terms": { "field": "category" }
    }
  }
}
```

## JavaScript Examples

### Using Elasticsearch Client
```javascript
const { Client } = require('@elastic/elasticsearch');
const client = new Client({ node: 'http://localhost:9200' });

// Index document
async function indexDocument() {
  await client.index({
    index: 'products',
    id: '1',
    document: {
      name: 'Laptop',
      price: 999.99,
      category: 'Electronics'
    }
  });
}

// Search
async function searchProducts(query) {
  const result = await client.search({
    index: 'products',
    query: {
      match: {
        name: query
      }
    }
  });
  
  return result.hits.hits.map(hit => hit._source);
}

// Update document
async function updatePrice(productId, newPrice) {
  await client.update({
    index: 'products',
    id: productId,
    doc: {
      price: newPrice
    }
  });
}

// Delete document
async function deleteProduct(productId) {
  await client.delete({
    index: 'products',
    id: productId
  });
}
```

### Bulk Operations
```javascript
async function bulkIndex(products) {
  const body = products.flatMap(product => [
    { index: { _index: 'products', _id: product.id } },
    product
  ]);
  
  const { errors } = await client.bulk({ body });
  
  if (errors) {
    console.error('Bulk indexing errors:', errors);
  }
}
```

## Field Types

### Text
- Full-text search
- Analyzed (tokenized)
- Example: product descriptions

### Keyword
- Exact match
- Not analyzed
- Example: categories, tags, IDs

### Numeric Types
- `long`, `integer`, `short`, `byte`
- `double`, `float`
- `half_float`, `scaled_float`

### Date
- ISO format dates
- Supports various formats

### Boolean
- `true` or `false`

### Object & Nested
- Complex objects
- Nested for arrays of objects

## Analyzers

### Standard Analyzer (Default)
- Lowercases text
- Splits on whitespace/punctuation
- Removes stop words

### Custom Analyzer
```bash
PUT /products
{
  "settings": {
    "analysis": {
      "analyzer": {
        "my_analyzer": {
          "type": "custom",
          "tokenizer": "standard",
          "filter": ["lowercase", "stop"]
        }
      }
    }
  }
}
```

## Best Practices

### Index Design
- Use appropriate number of shards (1 per 10-50GB)
- Set replicas for high availability
- Use aliases for zero-downtime reindexing

### Query Optimization
- Use filters instead of queries when possible (cached)
- Avoid wildcard queries on leading wildcards
- Use `_source` filtering to reduce data transfer
- Use pagination (`from`/`size` or `search_after`)

### Performance
- Bulk operations for multiple documents
- Use refresh strategically (disable for bulk loads)
- Monitor cluster health
- Use appropriate field types

### Mapping
- Define explicit mappings
- Use `keyword` for exact matches
- Use `text` for full-text search
- Avoid dynamic mapping in production

## Common Use Cases
- Full-text search
- Log aggregation and analysis
- Real-time analytics
- Autocomplete/suggestions
- Geospatial search
- Time-series data

## Query DSL Examples

### Multi-match (search multiple fields)
```bash
GET /products/_search
{
  "query": {
    "multi_match": {
      "query": "laptop",
      "fields": ["name^2", "description"]
    }
  }
}
```

### Fuzzy Query (typo tolerance)
```bash
GET /products/_search
{
  "query": {
    "fuzzy": {
      "name": {
        "value": "laptp",
        "fuzziness": "AUTO"
      }
    }
  }
}
```

### Highlighting
```bash
GET /products/_search
{
  "query": {
    "match": { "description": "laptop" }
  },
  "highlight": {
    "fields": {
      "description": {}
    }
  }
}
```

### Sorting
```bash
GET /products/_search
{
  "query": { "match_all": {} },
  "sort": [
    { "price": { "order": "asc" } },
    "_score"
  ]
}
```

## Monitoring

### Cluster Health
```bash
GET /_cluster/health
```

### Index Stats
```bash
GET /products/_stats
```

### Search Performance
```bash
GET /products/_search
{
  "profile": true,
  "query": {
    "match": { "name": "laptop" }
  }
}
```

