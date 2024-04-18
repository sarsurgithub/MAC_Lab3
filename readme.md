# Rapport Lab03 elasticSearch

## D1

```
PUT /cacm_standard
{
    "mappings": {
        "properties": {
            "id": {
                "type": "keyword",
                "store": true,
                "index": false
            },
            "author": {
                "type": "keyword"
            },
            "title": {
                "type": "text",
                "fielddata": true
            },
            "date": {
                "type": "date"
            },
            "summary": {
                "type": "text",
                "index_options": "offsets",
                "fielddata": true
            }
        }
    }
}
```

```
POST /_reindex
{
    "source": {
        "index": "cacm_dynamic"
    },
    "dest": {
        "index": "cacm_standard"
    }
}
```

## D2

```
PUT /cacm_termvector
{
    "mappings": {
        "properties": {
            "id": {
                "type": "keyword",
                "store": true,
                "index": false
            },
            "author": {
                "type": "keyword"
            },
            "title": {
                "type": "text",
                "fielddata": true
            },
            "date": {
                "type": "date"
            },
            "summary": {
                "type": "text",
                "index_options": "offsets",
                "term_vector": "yes",
                "fielddata": true
            }
        }
    }
}
```

```
POST /_reindex
{
    "source": {
        "index": "cacm_dynamic"
    },
    "dest": {
        "index": "cacm_termvector"
    }
}
```
## D3

```
GET /cacm_termvector/_termvectors/G2hBV44BbRQEeoKD6n1I
```
```
{
  "_index": "cacm_termvector",
  "_id": "G2hBV44BbRQEeoKD6n1I",
  "_version": 1,
  "found": true,
  "took": 2,
  "term_vectors": {
    "summary": {
      "field_statistics": {
        "sum_doc_freq": 97730,
        "doc_count": 1585,
        "sum_ttf": 150220
      },
      "terms": {
        "a": {
          "term_freq": 1
        },
        "accelerates": {
          "term_freq": 1
        },
        "an": {
          "term_freq": 3
        },
        "and": {
          "term_freq": 1
        },
        "applied": {
          "term_freq": 1
        },
        "convergence": {
          "term_freq": 2
        },
        "converges": {
          "term_freq": 1
        },
        "discussed": {
          "term_freq": 1
        },
        "diverges": {
          "term_freq": 1
        },
        "equation": {
          "term_freq": 1
        },
        "example": {
          "term_freq": 1
        },
        "for": {
          "term_freq": 1
        },
        "given": {
          "term_freq": 1
        },
        "if": {
          "term_freq": 2
        },
        "illustrative": {
          "term_freq": 1
        },
        "induces": {
          "term_freq": 1
        },
        "is": {
          "term_freq": 2
        },
        "iteration": {
          "term_freq": 2
        },
        "iterative": {
          "term_freq": 1
        },
        "of": {
          "term_freq": 2
        },
        "procedure": {
          "term_freq": 1
        },
        "rate": {
          "term_freq": 1
        },
        "solution": {
          "term_freq": 1
        },
        "technique": {
          "term_freq": 1
        },
        "the": {
          "term_freq": 4
        },
        "to": {
          "term_freq": 1
        },
        "when": {
          "term_freq": 1
        },
        "which": {
          "term_freq": 1
        }
      }
    }
  }
}
```
## D4

In Elasticsearch, a term vector is a data structure that provides detailed information about the terms (words) within a specific field of a document. Term vectors contain statistics about each term, such as their frequency, positions, offsets, and more. They are useful for various tasks, including document analysis, relevance scoring, and text mining.

## D5

```
GET /_cat/indices/cacm_standard,cacm_termvector?v
```
```
health status index           uuid                   pri rep docs.count docs.deleted store.size pri.store.size dataset.size
yellow open   cacm_termvector MbFLDQdmTUWCdf4eFT2ObA   1   1       3202            0      2.1mb          2.1mb        2.1mb
yellow open   cacm_standard   U1GkWjLdRgC5z5U5uoEu1w   1   1       3202            0      1.6mb          1.6mb        1.6mb
```

The cacm_standard index has a smaller store size than the cacm_termvector index, which is expected since the term vector index stores more information about the terms in the documents. The term vector index has a store size of 2.1mb, while the standard index has a store size of 1.6mb.

## D6

```
GET /cacm_standard/_search
{
  "size": 0,
  "aggs": {
    "authors": {
      "terms": {
        "field": "author",
        "size": 1
      }
    }
  }
}
```

```
{
  "took": 164,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 3202,
      "relation": "eq"
    },
    "max_score": null,
    "hits": []
  },
  "aggregations": {
    "authors": {
      "doc_count_error_upper_bound": 0,
      "sum_other_doc_count": 3083,
      "buckets": [
        {
          "key": "Thacher Jr., H. C.;",
          "doc_count": 36
        }
      ]
    }
  }
}
```
The author with the most documents in the cacm_standard index is "Thacher Jr., H. C.;", with 36 documents.

## D7
```
GET /cacm_standard/_search
{
  "size": 0,
  "aggs": {
    "top_titles": {
      "terms": {
        "field": "title",
        "size": 10,
        "order": {
          "_count": "desc"
        }
      }
    }
  }
}
```

```
{
  "took": 232,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 3202,
      "relation": "eq"
    },
    "max_score": null,
    "hits": []
  },
  "aggregations": {
    "top_titles": {
      "doc_count_error_upper_bound": 0,
      "sum_other_doc_count": 17307,
      "buckets": [
        {
          "key": "of",
          "doc_count": 1138
        },
        {
          "key": "algorithm",
          "doc_count": 975
        },
        {
          "key": "a",
          "doc_count": 895
        },
        {
          "key": "for",
          "doc_count": 714
        },
        {
          "key": "the",
          "doc_count": 645
        },
        {
          "key": "and",
          "doc_count": 434
        },
        {
          "key": "in",
          "doc_count": 416
        },
        {
          "key": "on",
          "doc_count": 340
        },
        {
          "key": "an",
          "doc_count": 275
        },
        {
          "key": "computer",
          "doc_count": 275
        }
      ]
    }
  }
}
```

| Term      | Frequency |
|-----------|-----------|
| of        | 1138      |
| algorithm | 975       |
| a         | 895       |
| for       | 714       |
| the       | 645       |
| and       | 434       |
| in        | 416       |
| on        | 340       |
| an        | 275       |
| computer  | 275       |

## D8

### Whitespace Analyzer
```
PUT /cacm_whitespace
{
  "settings": {
    "analysis": {
      "analyzer": {
        "whitespace": {
          "type": "whitespace"
        }
      }
    }
  },
  "mappings": {
    "properties": {
      "id": {
        "type": "keyword",
        "store": true,
        "index": false
      },
      "author": {
        "type": "keyword"
      },
      "title": {
        "type": "text",
        "fielddata": true
      },
      "date": {
        "type": "date"
      },
      "summary": {
        "type": "text",
        "index_options": "offsets",
        "fielddata": true
      }
    }
  }
}
```
```
POST /_reindex
{
  "source": {
    "index": "cacm_dynamic"
  },
  "dest": {
    "index": "cacm_whitespace"
  }
}
```
```
{
  "took": 1312,
  "timed_out": false,
  "total": 3202,
  "updated": 0,
  "created": 3202,
  "deleted": 0,
  "batches": 4,
  "version_conflicts": 0,
  "noops": 0,
  "retries": {
    "bulk": 0,
    "search": 0
  },
  "throttled_millis": 0,
  "requests_per_second": -1,
  "throttled_until_millis": 0,
  "failures": []
}
```

### English Analyzer
```
PUT /cacm_english
{
  "settings": {
    "analysis": {
      "analyzer": {
        "english": {
          "type": "english"
        }
      }
    }
  },
  "mappings": {
    "properties": {
      "id": {
        "type": "keyword",
        "store": true,
        "index": false
      },
      "author": {
        "type": "keyword"
      },
      "title": {
        "type": "text",
        "fielddata": true
      },
      "date": {
        "type": "date"
      },
      "summary": {
        "type": "text",
        "index_options": "offsets",
        "fielddata": true
      }
    }
  }
}
```
```
POST /_reindex
{
  "source": {
    "index": "cacm_dynamic"
  },
  "dest": {
    "index": "cacm_english"
  }
}
```
```
{
  "took": 940,
  "timed_out": false,
  "total": 3202,
  "updated": 0,
  "created": 3202,
  "deleted": 0,
  "batches": 4,
  "version_conflicts": 0,
  "noops": 0,
  "retries": {
    "bulk": 0,
    "search": 0
  },
  "throttled_millis": 0,
  "requests_per_second": -1,
  "throttled_until_millis": 0,
  "failures": []
}
```

### Custom (Standard + Shingles 1 & 2) Analyzer
```
PUT /cacm_custom_shingles12
{
  "settings": {
    "analysis": {
      "analyzer": {
        "custom_shingles12": {
          "type": "custom",
          "tokenizer": "standard",
          "filter": ["lowercase", "shingle_filter_12"]
        }
      },
      "filter": {
        "shingle_filter_12": {
          "type": "shingle",
          "max_shingle_size": 2,
          "output_unigrams": true
        }
      }
    }
  },
  "mappings": {
    "properties": {
      "id": {
        "type": "keyword",
        "store": true,
        "index": false
      },
      "author": {
        "type": "keyword"
      },
      "title": {
        "type": "text",
        "fielddata": true
      },
      "date": {
        "type": "date"
      },
      "summary": {
        "type": "text",
        "index_options": "offsets",
        "fielddata": true
      }
    }
  }
}
```
```
POST /_reindex
{
  "source": {
    "index": "cacm_dynamic"
  },
  "dest": {
    "index": "cacm_custom_shingles12"
  }
}
```
```
{
  "took": 1052,
  "timed_out": false,
  "total": 3202,
  "updated": 0,
  "created": 3202,
  "deleted": 0,
  "batches": 4,
  "version_conflicts": 0,
  "noops": 0,
  "retries": {
    "bulk": 0,
    "search": 0
  },
  "throttled_millis": 0,
  "requests_per_second": -1,
  "throttled_until_millis": 0,
  "failures": []
}
```

### Custom (Standard + Shingles 1 & 3) Analyzer
```
PUT /cacm_custom_shingles13
{
  "settings": {
    "analysis": {
      "analyzer": {
        "custom_shingles13": {
          "type": "custom",
          "tokenizer": "standard",
          "filter": ["lowercase", "shingle_filter_13"]
        }
      },
      "filter": {
        "shingle_filter_13": {
          "type": "shingle",
          "max_shingle_size": 3,
          "min_shingle_size": 3,
          "output_unigrams": false
        }
      }
    }
  },
  "mappings": {
    "properties": {
      "id": {
        "type": "keyword",
        "store": true,
        "index": false
      },
      "author": {
        "type": "keyword"
      },
      "title": {
        "type": "text",
        "fielddata": true
      },
      "date": {
        "type": "date"
      },
      "summary": {
        "type": "text",
        "index_options": "offsets",
        "fielddata": true
      }
    }
  }
}
```
```
POST /_reindex
{
  "source": {
    "index": "cacm_dynamic"
  },
  "dest": {
    "index": "cacm_custom_shingles13"
  }
}
```
```
{
  "took": 1379,
  "timed_out": false,
  "total": 3202,
  "updated": 0,
  "created": 3202,
  "deleted": 0,
  "batches": 4,
  "version_conflicts": 0,
  "noops": 0,
  "retries": {
    "bulk": 0,
    "search": 0
  },
  "throttled_millis": 0,
  "requests_per_second": -1,
  "throttled_until_millis": 0,
  "failures": []
}
```

### Stopwords Analyzer
```
PUT /cacm_custom_stopwords
{
  "settings": {
    "analysis": {
      "filter": {
        "english_stop": {
          "type": "stop",
          "stopwords_path": "data/common_words.txt"
        }
      },
      "analyzer": {
        "custom_stopwords": {
          "type": "custom",
          "tokenizer": "standard",
          "filter": ["english_stop"]
        }
      }
    }
  },
  "mappings": {
    "properties": {
      "id": {
        "type": "keyword",
        "store": true,
        "index": false
      },
      "author": {
        "type": "keyword"
      },
      "title": {
        "type": "text",
        "fielddata": true
      },
      "date": {
        "type": "date"
      },
      "summary": {
        "type": "text",
        "index_options": "offsets",
        "fielddata": true
      }
    }
  }
}
```
```
POST /_reindex
{
  "source": {
    "index": "cacm_dynamic"
  },
  "dest": {
    "index": "cacm_custom_stopwords"
  }
}
```
```
{
  "took": 307,
  "timed_out": false,
  "total": 3202,
  "updated": 0,
  "created": 3202,
  "deleted": 0,
  "batches": 4,
  "version_conflicts": 0,
  "noops": 0,
  "retries": {
    "bulk": 0,
    "search": 0
  },
  "throttled_millis": 0,
  "requests_per_second": -1,
  "throttled_until_millis": 0,
  "failures": []
}
```

## D9

## D10

## D11

## D12

## D13

## D14