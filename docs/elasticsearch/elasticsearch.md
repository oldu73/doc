# ElasticSearch

***

## Update field

### Plugin Head

Update boolean field with Chrome plugin ElasticSearch Head

local: <http://localhost:9200/>

Other request tab

URL <http://localhost:9200/index/doc/id>

to check

empty GET

to update

request _update POST

{"doc":{"deleted":false}}

### Kibana 5.6

Browse to Dev Tools, Console

to get:

```txt
GET _index/_type/_id
```

to update:

```txt
POST _index/_type/_id/_update
{
    "doc": {
        "field":"value"
        or
        "field":99
        or
        "field":false
    }
}
```

***

## Retrieve index

Retrieve to which index belongs an id, in Kibana 5.6 Dev Tools:

```txt
GET /*/_search
{
  "query": {
    "match": {
      "_id": "db37..-..-..-..-..9a117d83"
    }
  }
}
```

***
