# ElasticSearch

***

## Update field

### Plugin Head

Update boolean field with Chrome plugin ElasticSearch Head

local: http://localhost:9200/

Other request tab

URL http://localhost:9200/index/doc/id

to check

empty GET

to update

request _update POST

{"doc":{"deleted":false}}

### Kibana 5.6

Browse to Dev Tools, Console

to get:
```
GET _index/_type/_id
```

to update:
```
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
