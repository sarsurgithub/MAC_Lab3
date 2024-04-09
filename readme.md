# Rapport Lab03 elasticSearch

## D1

```
PUT /cacm_standard
{
    "mappings": {
        "properties": {
            "publication_id": {
                "type": "keyword"
            },
            "authors": {
                "type": "text",
                "fielddata": true
            },
            "title": {
                "type": "text",
                "fielddata": true
            },
            "publication_date": {
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
            "publication_id": {
                "type": "keyword"
            },
            "authors": {
                "type": "text",
                "fielddata": true
            },
            "title": {
                "type": "text",
                "fielddata": true
            },
            "publication_date": {
                "type": "date"
            },
            "summary": {
                "type": "text",
                "index_options": "offsets",
                "term_vector": "with_positions_offsets_payloads",
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
// TODO not working

```
GET /cacm_termvector/_termvectors/1
```
```
{
  "_index": "cacm_termvector",
  "_id": "97",
  "_version": 0,
  "found": false,
  "took": 1
}
```
## D4

// TODO What is a term vector?


## D5

```
GET /_cat/indices/cacm_standard,cacm_termvector?v
```
```
health status index           uuid                   pri rep docs.count docs.deleted store.size pri.store.size dataset.size
yellow open   cacm_termvector 9Q8GyiBJTBGRiQkmPDnI2g   1   1       3202            0      2.5mb          2.5mb        2.5mb
yellow open   cacm_standard   D5b8EgV_Rd-Tr44646DjpA   1   1       3202            0      1.8mb          1.8mb        1.8mb
```

// TODO discuss the size of the indices

## D6

## D7

## D8

## D9

## D10

## D11

## D12

## D13

## D14