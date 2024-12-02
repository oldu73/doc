# Kibana - Misc

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

## Percolate query

Match a document to the registered percolator queries ([src](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-percolate-query.html)):

```json
GET /<index name>/_search
{
  "query": {
    "percolate": {
      "field": "query",
      "document": {
        "<field1 id>": "<uuid>",
        "<field2 ids>": ["<uuid>"],
        "<field3 ids>": ["<uuid>"],
        "<event>": "<event name>"
      }
    }
  }
}
```

---

## TSVB time serie analysis

[Guide](https://www.elastic.co/guide/en/kibana/master/tsvb.html).

Kibana TSVB for time series analysis in Visualize Library:

- choose index pattern (data source) in "Panel options" tab.  

### Gaps

Gaps on chart between points.

In Kibana's Time Series Visual Builder (TSVB), gaps can occur when there is no data available for a particular time period. This can happen for various reasons, such as missing or incomplete data, or the data source not collecting data for that time period.

When there is a gap in the data, TSVB shows it as a missing area in the chart. By default, TSVB does not interpolate data points for the gaps, so the chart will have discontinuities. However, you can choose to fill the gaps with a specific value or interpolate the data points to create a smooth chart.

To fill the gaps with a specific value, you can use the `default` option in the `Model` tab of the visualization editor. This option sets a default value for the data points in the gaps.

To interpolate data points for the gaps, you can use the `Interpolate` option in the `Model` tab. This option calculates a value for the missing data points by estimating the value based on the surrounding data points. This creates a smooth chart without any gaps.

In summary, TSVB gaps occur when there is no data available for a particular time period. You can choose to fill the gaps with a specific value or interpolate the data points to create a smooth chart.

---

## Double display

Custom `Double` number representation.

By default, some decimals may appear missing (rounding) even though in reality they are stored correctly in Elasticsearch.

[SRC: Numeral Formatting](https://www.elastic.co/guide/en/kibana/7.17/numeral.html)

In Kibana:

- Browse to `Stack Management` - `Index patterns` click on your index.  
- Edit your field, enable `Set format`, choose `Number`.  
- To notice if you let mouse over the field in preview pan, you already may observe raw value (no rounding) stored in Elasticsearch.  
- Enter numeral pattern, e.g. `0.######` click on save.  
- Browse back to `Discover` window and their you may observe new numeral format applied on displayed field.

---

## KQL filter

In visualization, to filter documents against all, the tricks is to set 2 filters one with '*' and the other one with desired matching pattern, e.g.:

```txt
: *
: <field 1>: <content 1> AND NOT <field 2>: <content 2>
```

---
