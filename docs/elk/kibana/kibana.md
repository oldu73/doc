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
        "field" : "value"
        or
        "field" : 99
        or
        "field" : false
        or
        "location" : {
          "lat" : 46.7552483,
          "lon" : 7.6519766
        }
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

## Filter in query bar

Filter in query bar (not in filter-pill) to get all records that id field start with a pattern:

```txt
id:3*
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

## KQL

Kibana Query Language, e.g.:

```kql
accountId : <account name> and lastname : *0000* or firstname : *FIRST*
```

---
