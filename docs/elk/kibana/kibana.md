# Kibana

***

## Update field

Kibana 5.6

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

## Filter in query bar

Filter in query bar (not in filter-pill) to get all records that id field start with a pattern:

```txt
Id:3*
```

***

## Index pattern scripted field

Index pattern scripted field to calculate time difference, in seconds, between two dates, language "painless", type "number", format pattern "00.":

```painless
(doc['gatewayDate'].value - doc['timestamp'].value)/1000
```

***
