## microtask-6

Using the dev tools in Kibiter, create a query that counts the number of unique authors on a Git repository from 2018-01-01 until 2019-01-01.

The Dev Tools :wrench: option is present on the left (side bar) of the Kibana Dashboard, above Management :gear:. 

You can access it with URL, [localhost:5601/app/kibana#/dev_tools/console?_g=()](http://localhost:5601/app/kibana#/dev_tools/console?_g=()) , 
if you have the dev environment of grimoirelab setup locally as per the [microtask-4](/microtask-4).

The important fields we need to extract from the task requirement is
- backend: Git
- start: 2018-01-01
- end: 2019-01-01

The start and end dates can be queried by converting them to Unix Timestamp (in milliseconds). You can use [Epoch Converter](https://www.epochconverter.com/).

The query is

```
curl -XGET "http://localhost:9200/_search" -H 'Content-Type: application/json' -d'
{
  "size": 0,
  "aggs": {
    "1": {
      "terms": {
        "field": "author_name",
        "size": 1000,
        "order": {
          "_count": "desc"
        }
      }
    }
  },
  "docvalue_fields": [
    "grimoire_creation_date"
  ],
  "query": {
    "bool": {
      "must": [
        {
          "match_all": {}
        },
        {
          "range": {
            "grimoire_creation_date": {
              "gte": 1514764800000,
              "lte": 1546300800000,
              "format": "epoch_millis"
            }
          }
        }
      ]
    }
  }
}'
```

You can find the result of the query, tested in the [CHAOSS Dashboard](https://chaoss.biterg.io/app/kibana#/dev_tools/console?_g=()) in [result](result).
