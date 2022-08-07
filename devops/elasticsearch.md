# Elasticsearch commands     
```
curl --cacert elasticsearch-ca.pem -u elastic:<password> -XGET https://elasticsearch.<IP>.nip.io:9200
curl --cacert elasticsearch-ca.pem -u elastic:<password> -s -XGET https://elasticsearch.<IP>.nip.io:9200/_cluster/health | jq '.'    
```

###  НЕ ПОМНЮ что-это     
```
curl -s -XPUT "https://elasticsearch.<IP>.nip.io:9200/_ilm/policy/log_cleaner" \
--cacert elasticsearch-ca.pem -u elastic:<password> \
-H 'Content-Type: application/json' --data-binary @- << EOF |
{
  "policy": {
    "phases": {
      "delete": {
        "min_age": "30d",
        "actions": {
          "delete": {}
        }
      }
    }
  }
}
EOF
jq '.'

curl -s -XPUT "https://elasticsearch.<IP>.nip.io:9200/_template/logstash" \
--cacert elasticsearch-ca.pem -u elastic:<password> \
-H 'Content-Type: application/json' --data-binary @- << EOF |
{
  "index_patterns" : [
    "logstash-*"
  ],
  "mappings": {
    "dynamic_templates": [
      {
        "strings": {
          "match_mapping_type": "string",
          "mapping": {
            "type": "text",
            "fields": {
              "keyword": {
                "type":  "keyword",
                "ignore_above": 1024
              }
            }
          }
        }
      }
    ]
  },
  "settings": {
    "index" : {
      "number_of_replicas" : 0,
      "lifecycle" : {
        "name" : "log_cleaner"
        }
      }
  }
}
EOF
jq '.'
```

### Ремонт битых индексов    
```
curl --cacert elasticsearch-ca.pem -u elastic:<password> -s -XGET https://elasticsearch.<IP>.nip.io:9200/_cluster/health | jq '.'
curl --cacert elasticsearch-ca.pem -u elastic:<password> -s -XGET https://elasticsearch.<IP>.nip.io:9200/_cat/indices?v

curl --cacert elasticsearch-ca.pem -u elastic:<password> -s -XGET https://elasticsearch.<IP>.nip.io:9200/_cat/shards?v
curl --cacert elasticsearch-ca.pem -u elastic:<password> -s -XGET https://elasticsearch.<IP>.nip.io:9200/_cluster/allocation/explain?pretty

curl --cacert elasticsearch-ca.pem -u elastic:<password> -XPUT -H 'Content-Type: application/json' 'https://elasticsearch.<IP>.nip.io:9200/metrics-index_pattern_placeholder/_settings' -d '{ "number_of_replicas" : 0 } }'

curl --cacert elasticsearch-ca.pem -u elastic:<password> -XPUT -H 'Content-Type: application/json' 'https://elasticsearch.<IP>.nip.io:9200/logs-index_pattern_placeholder/_settings' -d '{ "number_of_replicas" : 0 } }'

curl --cacert elasticsearch-ca.pem -u elastic:<password> -XPUT -H 'Content-Type: application/json' 'https://elasticsearch.<IP>.nip.io:9200/metrics-index_pattern_placeholder/_settings' -d '{ "number_of_replicas" : 0 } }'
```