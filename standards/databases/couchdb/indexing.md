# Apache CouchDB Indexing Guide (2026)

## Overview & Accuracy Analysis

The technical breakdown provided is **accurate and aligned with modern CouchDB practices**:

- **Mango JSON Indexes (`/_find`)**: Declarative B-Tree indexes for standard equality, range, and sort operations.
- **MapReduce Views**: Pre-calculated B-Tree indexes built incrementally for custom key emissions and aggregations (`_count`, `_sum`, `_stats`).
- **Nouveau Search**: The modern full-text engine introduced in CouchDB 3.4+, leveraging Apache Lucene to replace legacy Clouseau/Erlang setups.
- **Optimization & Operations**: Explicit index definition, partition-aware indexing, and `ken` daemon tuning are production-essential practices.

---

## 1. Indexing Options Comparison

| Feature               | Mango JSON Indexes                 | MapReduce Views                             | Nouveau Search                                         |
| :-------------------- | :--------------------------------- | :------------------------------------------ | :----------------------------------------------------- |
| **Primary Endpoint**  | `POST /<db>/_index`                | `PUT /<db>/_design/<id>`                    | `PUT /<db>/_design/<id>`                               |
| **Query Mechanism**   | `POST /<db>/_find`                 | `GET /<db>/_design/<id>/_view/<name>`       | `GET /<db>/_design/<id>/_nouveau/<name>`               |
| **Best For**          | Fast declarative lookups & sorting | Custom keys, groupings, precomputed metrics | Full-text, phrase search, tokenization, fuzzy matching |
| **Underlying Engine** | Erlang B-Tree                      | Erlang B-Tree                               | Apache Lucene (Java)                                   |

---

## 2. Implementation Patterns

### A. Mango JSON Index (`/_find`)

Creates a declarative index over target document fields.

**Request:** `POST /<database>/_index`

```json
{
  "index": {
    "fields": ["type", "createdAt"]
  },
  "ddoc": "idx-type-created",
  "name": "type-created-json",
  "type": "json"
}
```

**Executing Query:** `POST /<database>/_find`

```json
{
  "selector": {
    "type": "task",
    "createdAt": { "$gte": "2026-01-01" }
  },
  "sort": [{ "type": "asc" }, { "createdAt": "asc" }],
  "use_index": ["_design/idx-type-created", "type-created-json"]
}
```

---

### B. MapReduce View Index

Used for relational aggregation or custom emitted keys.

**Request:** `PUT /<database>/_design/analytics`

```json
{
  "_id": "_design/analytics",
  "views": {
    "by_status_count": {
      "map": "function (doc) { if (doc.type && doc.status) { emit([doc.type, doc.status], 1); } }",
      "reduce": "_count"
    }
  },
  "language": "javascript"
}
```

**Executing Query:** `GET /<database>/_design/analytics/_view/by_status_count?group_level=2`

---

### C. Nouveau Full-Text Index

For advanced full-text queries powered by Apache Lucene.

**Request:** `PUT /<database>/_design/search_v1`

```json
{
  "_id": "_design/search_v1",
  "nouveau": {
    "doc_search": {
      "default_analyzer": "standard",
      "index": "function(doc) { if (doc.title) { index('text', 'title', doc.title); } }"
    }
  }
}
```

**Executing Query:** `GET /<database>/_design/search_v1/_nouveau/doc_search?q=title:couchdb`

---

## 3. Production Best Practices

1. **Avoid Full Scans**: Always define explicit indexes before issuing `_find` queries. If no suitable index exists, CouchDB defaults to a full database scan (`_all_docs`), which rapidly degrades performance on large datasets.
2. **Strict Sort Alignment**: Any field specified in a `_find` `sort` clause **must** be included in the matching Mango index in the exact same field order.
3. **Partitioned Indexes**: On partitioned databases (`"partitioned": true`), partition-bound queries route directly to the designated database shard, drastically reducing overall resource footprint and query latency.
4. **Tune Background Indexing (`[ken]`)**: Configure the `[ken]` daemon in your `local.ini` file (`batch_channels`, `incremental_channels`) to prevent index warming tasks from overwhelming I/O capacity during high-throughput updates.
