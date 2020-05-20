- 为某个索引的某个字段,设置ik_max_word分词
```
curl -XPUT http://192.168.5.59:3895/index名称 -d '
{
    "settings": {
        "index.analysis.analyzer.default.type": "ik_max_word"
    },
    "mappings": {
        "doc": {
            "properties": {
                "file_content": {
                    "type": "text",
                    "analyzer": "ik_max_word",
                    "search_analyzer": "ik_max_word"
                }
            }
        }
    }
}'
```
