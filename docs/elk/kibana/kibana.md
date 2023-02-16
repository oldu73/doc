# Kibana

---

## Search

Kibana 7.17

Be aware of `size`:

```txt
GET <index>/_search
{
    "query": {
        "match_all": {}
    },
    "size": 100
}
```

Another sample:

```txt
POST <index>/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "match": {
            "accountId": "<account id>"
          }
        },
        {
          "match": {
            "firstname": "<first name>"
          }
        }
      ]
    }
  }
}
```

---

## Update field

Kibana 7.17

Browse to Dev Tools, Console

to get:

```txt
GET <index>/_doc/<id>
```

to update:

```txt
POST <index>/_update/<id>
{
    "doc": {
        "<string field name>" : "value"
        or
        "<integer field name>" : 99
        or
        "<boolean field name>" : false
        or
        "location field name>" : {
          "lat" : 46.7552483,
          "lon" : 7.6519766
        }
        or
        "<timestamp field name>": "2023-02-16T07:01:39.584Z"
    }
}
```

---

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

---

## KQL Filter in query bar

Kibana Query Language, filter in query bar (not in filter-pill) to get all records that id field start with a pattern:

```kql
id:3*
```

Another sample:

```kql
accountId : <account name> and lastname : *0000* or firstname : *FIRST*
```

---

## Index pattern scripted field

Index pattern scripted field to calculate time difference, in seconds, between two dates, language "painless", type "number", format pattern "00.":

```painless
(doc['gatewayDate'].value - doc['timestamp'].value)/1000
```

---

## Delete doc

Kibana 7.17

- discover to get doc id
- dev tools e.g. `GET/DELETE /index/_doc/<id>`

---

## List connected agent

Kibana 7.17, dev tools:

```txt
GET _nodes/stats?filter_path=**.clients
```

---

## List queue

Kibana 7.17, dev tools:

```txt
GET _cat/thread_pool/search,write?v
```

---
