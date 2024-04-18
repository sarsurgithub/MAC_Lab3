# Rapport Lab03 elasticSearch

## D1

```
PUT /cacm_standard
{
    "mappings": {
        "properties": {
            "id": {
                "type": "keyword"
                "store": true,
                "index": false
            },
            "author": {
                "type": "text",
                "fielddata": true
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
                "type": "keyword"
                "store": true,
                "index": false
            },
            "author": {
                "type": "text",
                "fielddata": true
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
yellow open   cacm_termvector 2RL_rBYeTE63MY1LSWr-3A   1   1       3202            0      2.1mb          2.1mb        2.1mb
yellow open   cacm_standard   f8ArYHFFRemzvonsMii0ww   1   1       3202            0      1.7mb          1.7mb        1.7mb
```

The cacm_standard index has a smaller store size than the cacm_termvector index, which is expected since the term vector index stores more information about the terms in the documents. The term vector index has a store size of 2.1mb, while the standard index has a store size of 1.7mb.

## D6

## D7

## D8

## D9

## D10

## D11

## D12

## D13

## D14