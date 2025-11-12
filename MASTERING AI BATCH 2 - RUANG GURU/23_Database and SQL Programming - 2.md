# Database and SQL Programming - Part 2

## Elasticsearch

### Apa itu Elasticsearch?

Elasticsearch adalah search engine berbasis Apache Lucene yang powerful untuk:
- Full-text search
- Real-time data analysis
- Log dan event data
- Monitoring dan observability

**Perbedaan dengan SQL Database:**
- NoSQL (document-oriented)
- Optimized untuk search dan analytics
- Schema-less (fleksibel)
- Distributed dan scalable

### Kapan Pakai Elasticsearch?

**Use Cases:**
1. **Search aplikasi:** e-commerce, blog, dokumentasi
2. **Log analysis:** centralized logging system
3. **Metrics dan monitoring:** system performance
4. **Analytics:** real-time data analysis

**Kelebihan:**
- Search yang sangat cepat
- Scalable horizontal (tinggal tambah node)
- Real-time indexing
- RESTful API

### Instalasi Elasticsearch

**Via Docker (recommended):**
```bash
docker pull docker.elastic.co/elasticsearch/elasticsearch:8.x.x
docker run -p 9200:9200 -e "discovery.type=single-node" elasticsearch:8.x.x
```

**Test installation:**
```bash
curl http://localhost:9200
```

Akan return JSON dengan info cluster.

## Konsep Dasar Elasticsearch

### 1. Index
- Seperti database di SQL
- Collection of documents
- Contoh: index "products", "users", "logs"

### 2. Document
- Seperti row di SQL
- Data dalam format JSON
- Setiap document punya unique ID

### 3. Field
- Seperti column di SQL
- Key-value pairs dalam document
- Bisa nested

### 4. Mapping
- Seperti schema di SQL
- Define tipe data untuk setiap field
- Dynamic atau explicit

## Basic Operations

### 1. Create Index
```bash
PUT /products
{
  "mappings": {
    "properties": {
      "name": { "type": "text" },
      "price": { "type": "float" },
      "category": { "type": "keyword" },
      "description": { "type": "text" },
      "created_at": { "type": "date" }
    }
  }
}
```

**Field Types:**
- `text`: full-text search (dianalisis)
- `keyword`: exact match (tidak dianalisis)
- `integer`, `float`: numeric
- `date`: timestamp
- `boolean`: true/false

### 2. Index Document (Insert)
```bash
POST /products/_doc
{
  "name": "Laptop ASUS",
  "price": 15000000,
  "category": "Electronics",
  "description": "Laptop gaming dengan spesifikasi tinggi"
}
```

**Dengan custom ID:**
```bash
PUT /products/_doc/1
{
  "name": "iPhone 15",
  "price": 20000000,
  "category": "Electronics"
}
```

### 3. Get Document
```bash
GET /products/_doc/1
```

### 4. Update Document
```bash
POST /products/_update/1
{
  "doc": {
    "price": 18000000
  }
}
```

### 5. Delete Document
```bash
DELETE /products/_doc/1
```

### 6. Search Documents
```bash
GET /products/_search
{
  "query": {
    "match_all": {}
  }
}
```

## Query DSL (Domain Specific Language)

### Match Query
Full-text search:
```json
{
  "query": {
    "match": {
      "name": "laptop gaming"
    }
  }
}
```
- Mencari documents yang mengandung "laptop" atau "gaming"
- Hasil di-rank berdasarkan relevance score

### Match Phrase
Search exact phrase:
```json
{
  "query": {
    "match_phrase": {
      "description": "gaming laptop"
    }
  }
}
```
- Harus exact order: "gaming laptop"
- Bukan "laptop gaming"

### Term Query
Exact match (untuk keyword fields):
```json
{
  "query": {
    "term": {
      "category": "Electronics"
    }
  }
}
```

### Range Query
Query dengan range values:
```json
{
  "query": {
    "range": {
      "price": {
        "gte": 10000000,
        "lte": 20000000
      }
    }
  }
}
```
- `gte`: greater than or equal
- `lte`: less than or equal
- `gt`: greater than
- `lt`: less than

### Bool Query
Combine multiple queries:
```json
{
  "query": {
    "bool": {
      "must": [
        { "match": { "name": "laptop" } }
      ],
      "filter": [
        { "range": { "price": { "lte": 15000000 } } }
      ],
      "should": [
        { "match": { "category": "Electronics" } }
      ],
      "must_not": [
        { "term": { "brand": "unknown" } }
      ]
    }
  }
}
```

**Clauses:**
- `must`: harus match (affect score)
- `filter`: harus match (tidak affect score, faster)
- `should`: optional match (boost score jika match)
- `must_not`: tidak boleh match

## Aggregations

### Metrics Aggregations

**Average:**
```json
{
  "aggs": {
    "avg_price": {
      "avg": { "field": "price" }
    }
  }
}
```

**Sum, Min, Max, Stats:**
```json
{
  "aggs": {
    "total_price": { "sum": { "field": "price" } },
    "min_price": { "min": { "field": "price" } },
    "max_price": { "max": { "field": "price" } },
    "price_stats": { "stats": { "field": "price" } }
  }
}
```

### Bucket Aggregations

**Terms Aggregation (GROUP BY):**
```json
{
  "aggs": {
    "categories": {
      "terms": { "field": "category" }
    }
  }
}
```
- Mengelompokkan berdasarkan category
- Menghitung count per category

**Range Aggregation:**
```json
{
  "aggs": {
    "price_ranges": {
      "range": {
        "field": "price",
        "ranges": [
          { "to": 5000000 },
          { "from": 5000000, "to": 15000000 },
          { "from": 15000000 }
        ]
      }
    }
  }
}
```

**Date Histogram:**
```json
{
  "aggs": {
    "sales_over_time": {
      "date_histogram": {
        "field": "created_at",
        "calendar_interval": "month"
      }
    }
  }
}
```

### Nested Aggregations
Aggregation dalam aggregation:
```json
{
  "aggs": {
    "categories": {
      "terms": { "field": "category" },
      "aggs": {
        "avg_price": {
          "avg": { "field": "price" }
        }
      }
    }
  }
}
```
- Group by category
- Hitung average price per category

## Advanced Features

### 1. Fuzzy Search
Toleransi typo:
```json
{
  "query": {
    "fuzzy": {
      "name": {
        "value": "laptap",
        "fuzziness": "AUTO"
      }
    }
  }
}
```
- "laptap" akan match dengan "laptop"
- Fuzziness: jumlah character changes yang diperbolehkan

### 2. Wildcard Search
```json
{
  "query": {
    "wildcard": {
      "name": "lap*"
    }
  }
}
```
- `*`: multiple characters
- `?`: single character

### 3. Highlighting
Highlight matching terms:
```json
{
  "query": {
    "match": { "description": "gaming" }
  },
  "highlight": {
    "fields": {
      "description": {}
    }
  }
}
```

### 4. Sorting
```json
{
  "query": { "match_all": {} },
  "sort": [
    { "price": "desc" },
    { "_score": "desc" }
  ]
}
```

### 5. Pagination
```json
{
  "query": { "match_all": {} },
  "from": 0,
  "size": 10
}
```
- `from`: offset
- `size`: limit

## Python Integration

### Menggunakan elasticsearch-py

**Installation:**
```bash
pip install elasticsearch
```

**Connection:**
```python
from elasticsearch import Elasticsearch

# Connect
es = Elasticsearch(["http://localhost:9200"])

# Check connection
print(es.ping())
```

**Index Document:**
```python
doc = {
    "name": "Laptop ASUS",
    "price": 15000000,
    "category": "Electronics"
}
es.index(index="products", id=1, document=doc)
```

**Search:**
```python
query = {
    "match": {
        "name": "laptop"
    }
}
result = es.search(index="products", query=query)

for hit in result['hits']['hits']:
    print(hit['_source'])
```

**Bulk Operations:**
```python
from elasticsearch.helpers import bulk

actions = [
    {
        "_index": "products",
        "_id": 1,
        "_source": {"name": "Product 1", "price": 10000}
    },
    {
        "_index": "products",
        "_id": 2,
        "_source": {"name": "Product 2", "price": 20000}
    }
]

bulk(es, actions)
```

## SQL vs Elasticsearch

| SQL | Elasticsearch |
|-----|---------------|
| Database | Index |
| Table | Type (deprecated) |
| Row | Document |
| Column | Field |
| Schema | Mapping |
| SELECT * FROM | GET /index/_search |
| INSERT | PUT/POST |
| UPDATE | POST /_update |
| DELETE | DELETE |
| WHERE | Query DSL |
| GROUP BY | Aggregations |
| ORDER BY | Sort |
| LIMIT | Size |
| OFFSET | From |

## Best Practices

### 1. Mapping Design
- Set explicit mapping untuk production
- Gunakan `keyword` untuk exact match fields
- Gunakan `text` untuk full-text search
- Disable indexing untuk fields yang tidak di-search

### 2. Query Optimization
- Gunakan `filter` context untuk boolean queries (faster, cacheable)
- Avoid wildcard queries di production (slow)
- Use appropriate analyzers

### 3. Indexing
- Bulk operations untuk insert banyak documents
- Refresh interval: default 1s, bisa dinaikkan untuk write-heavy
- Replica shards untuk high availability

### 4. Monitoring
- Monitor cluster health: `GET /_cluster/health`
- Monitor node stats: `GET /_nodes/stats`
- Use Kibana untuk visualization

## Common Use Cases

### 1. Log Analysis
```python
# Index logs
log = {
    "timestamp": "2024-02-05T10:00:00",
    "level": "ERROR",
    "message": "Connection timeout",
    "service": "api"
}
es.index(index="logs-2024.02.05", document=log)

# Search errors in last hour
query = {
    "bool": {
        "must": [
            {"term": {"level": "ERROR"}},
            {"range": {"timestamp": {"gte": "now-1h"}}}
        ]
    }
}
```

### 2. E-commerce Search
```python
# Product search with filters
query = {
    "bool": {
        "must": [
            {"match": {"name": "laptop"}}
        ],
        "filter": [
            {"range": {"price": {"lte": 20000000}}},
            {"term": {"category": "Electronics"}}
        ]
    }
}

# Aggregations for faceted search
aggs = {
    "price_ranges": {
        "range": {
            "field": "price",
            "ranges": [
                {"to": 5000000},
                {"from": 5000000, "to": 15000000},
                {"from": 15000000}
            ]
        }
    },
    "brands": {
        "terms": {"field": "brand"}
    }
}
```

### 3. Autocomplete
```python
# Mapping with completion type
mapping = {
    "properties": {
        "suggest": {
            "type": "completion"
        }
    }
}

# Suggest query
suggest = {
    "product_suggest": {
        "prefix": "lap",
        "completion": {
            "field": "suggest"
        }
    }
}
```

## Key Takeaways

1. **Elasticsearch untuk full-text search** dan real-time analytics
2. **Query DSL sangat flexible** untuk berbagai search scenarios
3. **Aggregations powerful** untuk analytics dan reporting
4. **Python integration mudah** dengan elasticsearch-py
5. **Performance optimization penting** terutama untuk production
6. **Monitoring cluster health** secara rutin
7. **Elasticsearch bukan replacement** untuk SQL database, tapi complement
