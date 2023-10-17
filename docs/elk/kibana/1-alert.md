# Kibana - Alert

---

## Watcher

### Kibana

Get MS Teams alert (web hook) based on scheduled (fired week days (not weekend)) filter matching documents (boolean fields) in Elasticsearch.

In Kibana, browse to `Stack Management - Alerts and Insights - Watcher`, click on `Create advanced watch .. (JSON)`.

In below sample (JSON), to notice `cron` for weekdays, `match_phrase` to filter by boolean field and `path` to `webhook` for MS Teams channel:

```JSON
{
  "trigger": {
    "schedule": {
      "cron": "0 0 6 ? * MON-FRI"
    }
  },
  "input": {
    "search": {
      "request": {
        "search_type": "query_then_fetch",
        "indices": [
          "<index>"
        ],
        "rest_total_hits_as_int": true,
        "body": {
          "size": 0,
          "query": {
            "bool": {
              "filter": [
                {
                  "match_phrase": {
                    "<String field name>": "<String content>"
                  }
                },
                {
                  "range": {
                    "timestamp": {
                      "gte": "now-24h",
                      "lte": "now"
                    }
                  }
                },
                {
                  "match_phrase": {
                    "<Boolean field name 1>": <true|false>
                  }
                },
                {
                  "match_phrase": {
                    "<Boolean field name 2>": <true|false>
                  }
                }
              ]
            }
          },
          "aggs": {
            "<misc_ids>": {
              "composite": {
                "sources": [
                  {
                    "<miscId>": {
                      "terms": {
                        "field": "<miscId>",
                        "order": "asc"
                      }
                    }
                  }
                ]
              }
            }
          }
        }
      }
    }
  },
  "condition": {
    "always": {}
  },
  "actions": {
    "send_teams_alert": {
      "webhook": {
        "scheme": "https",
        "host": "synchrotechit.webhook.office.com",
        "port": 443,
        "method": "post",
        "path": "<webhook>",
        "params": {},
        "headers": {
          "Content-Type": "application/json"
        },
        "body": """{"title": "<title content>", "text": "{{#ctx.payload.aggregations.<misc_ids>.buckets}}{{key.<miscId>}}<br>{{/ctx.payload.aggregations.<misc_ids>.buckets}}{{^ctx.payload.aggregations.<misc_ids>.buckets}}<No document found>{{/ctx.payload.aggregations.<misc_ids>.buckets}}"}"""
      }
    }
  }
}
```

### MS Teams

NEW - In a Teams channel click on 3 dots menu and select `Connectors` then `Incoming Webhook - Configure - Create`.

EDIT - In a Teams channel click on 3 dots menu and select `Connectors` then `Configured - expand Configured - Manage`.

Set a name and get path to set in Kibana Watcher JSON.

---
