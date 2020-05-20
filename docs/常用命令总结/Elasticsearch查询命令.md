> Elasticsearch的访问风格遵循Restful Api，所以其接口方法决定使用方法，PUT 插入数据，POST 带参请求数据，GET 无参请求数据，Delete 删除数据

- 为某个index的某个字段添加分词器

```
curl -XPUT http://192.168.5.59:9200/索引名称 -d '
{
    "settings": {
        "index.analysis.analyzer.default.type": "ik_max_word"
    },
    "mappings": {
        "doc": {
            "properties": {
                "字段名称": {
                    "type": "text",
                    "analyzer": "ik_max_word",
                    "search_analyzer": "ik_max_word"
                }
            }
        }
    }
}'
```

- 根据日期范围分组统计查询出同时满足一个条件的数据(**查询出子Json DATA中日志级别字段为Warn的数据，并将结果按照日期_type分组统计**)

```
curl -XGET http://192.168.5.59:9200/yingyonger/_search?pretty -d '
{
	"size": 0,  
	  "aggs": {  
		"group_by_state": {  
		  "terms": {          
			"field": "_type"
		  }  
		}  
	},
  "query": {
    "bool":{
      "must": [
			{"match": {"DATAS.CRLOG_LEVEL":"Warn"}}
		],
	  "filter":[
			{"range":{"_type":{"gte":"20180828","lte":"20180828"}}}
		]
    }
  }
}'
```

- 根据日期范围分组统计查询出同时满足两个条件的数据，其中一个条件多值(**查询出DB_SOURCE等于Oracle并且满足日志级别为Error或者Warn的数据，并将结果按照日期_type分组统计**)

```
curl -XGET http://192.168.5.59:9200/yingyonger/_search?pretty -d '
{
	"size": 0,  
	  "aggs": {  
		"group_by_state": {  
		  "terms": {          
			"field": "_type"
		  }  
		}  
	},
  "query": {
    "bool":{
      "should": [
			{"term": {"DATAS.CRLOG_LEVEL":"Error"}},
			{"term": {"DATAS.CRLOG_LEVEL":"Warn"}}
		],
	  "must":[
			{"match": {"DB_SOURCE":"Oracle"}}
		],
	  "filter":[
			{"range":{"_type":{"gte":"20180828","lte":"20180828"}}}
		]
    }
  }
}'
```

- term 查询(**对传入的值不会做任何处理,精准匹配型**)

> SELECT * FROM TABLE WHERE CITY= '杭州'

```
{
  "query": {
    "term": {
      "CITY": "杭州"
    }
  }
}
```

- terms查询(**查询多个值，其中一个满足就会返回数据**)

> SELECT * FROM TABLE WHERE TITLE REGEXP '大数据|机器学习|真牛逼'

```
{
  "query": {
    "terms":{
      "title":["大数据","机器学习","真牛逼"]
    }
  }
}
```

- multi_match 多个字段匹配

```
POST http://localhost:9200/test/_search
{
    "query":{
        "multi_match":{
            "query":"见",
            "fields":["name","introduce"]
        }
    }   
}
```


- from size(**控制查询的返回数量,用于分页**)

> SELECT * FROM TABLE WHERE TITLE REGEXP 'PYTHON' LIMIT 0, 2

```
{
  "query": {
    "match": {
      "title": "PYTHON"
    }
  },
  "from":0,
  "size":2
}
```

- match_all(**查询全部数据**)

> SELECT * FROM TABLE

```
{
  "query": {
    "match_all": {}
  }
}
```

- multi_match(**可以指定多个字段查询**)

> 比如查询TITLE和DESC这两个字段里包含Python的关键词文档 – 可以检索多个字段出现的查询词

```
{
  "query": {
    "multi_match": {
      "query": "Python",
      "fields": ["TITLE","DESC"]
    }
  }
}
```

- sort order(**查询并对结果排序操作**)

> SELECT * FROM TABLE ORDER BY ID DESC

```
{
  "query":{
    "match_all":{}
  },
  "sort":"ID"
}


{
  "query":{
    "match_all":{}
  },
  "sort": { "ID": { "order": "desc" }}
}
```

- range(**通用的范围查询,并且设置boost权重**)
    - gte: 大于等于
    - gt 大于
    - lte 小于等于
    - lt 小于

> SELECT * FROM TABLE WHERE 字段名称 >= 10 AND 字段名称 <= 20 AND boost = 1

```
{
  "query": {
    "range":{
      "字段名称": {
        "gte": 10,
        "lte": 20,
        "boost": 1
      }
    }
  }
}
```

- range(**范围时间查询中的now 为当前时间**)

```
{
  "query": {
    "range": {
      "add_time": {
        "gte": "2016-04-01",
        "lte": "now"
      }
    }
  }
}
```

- wildcard(**模糊查询,boost 可不设置表名该字段的权重值, 支持查询字符串里有通配符**)

```
{
  "query": {
    "wildcard": {
      "title": {
        "value": "pyth*n",
        "boost": 2
      }
    }
  }
}
```

- 多条件组合操作

```
bool:{
     "filter":[],  对字段进行过滤同时不参与打分
     "must":[],   数组里的所有查询都必须满足
     "should":[],数组里的查询满足一个和多个都可以
     "must_not":{}, 数组里的查询一个都不能满足
}
```

- filter(**简单的过滤查询**)

> SELECT * FROM TABLE WHERE SALARY != 20

```
{
  "query":{
    "bool":{
      "must":{
        "match_all":{}
      },
      "filter": {
        "term": {
          "SALARY": "20"
        }
      }
    }
  }
}
```

- filter(**过滤多个值**)

> SELECT * FROM TABLE WHERE SALARY != 20 AND SALARY != 30

```
{
  "query":{
    "bool":{
      "must":{
        "match_all":{}
      },
      "filter": {
        "term": {
          "SALARY": ["20","30"]
        }
      }
    }
  }
}
```

- should,must_not(**过滤组合查询表达式**)

> SELECT * FROM TABLE WHERE (SALARY = 20 OR WORK = 'Python') AND SALARY != 30

```
{
  "query": {
    "bool": {
      "should": [
        {"term": {"SALARY": 20}},
        {"term":{"WORK": "python"}}
      ],
      "must_not":{
        "term":{"SALARY":30}
      }
    }
  }
}
```

- should,must(**过滤组合查询表达式**)

> SELECT * FROM TABLE WHERE WORK="Python" OR (WORK = "Django" and SALARY=30)

```
{
  "query": {
    "bool": {
      "should": [
        {"term": {"WORK": "Python"}},
        {"bool": {
          "must":[
            {"term": {"WORK":"Django"}},
            {"term":{"SALARY":30}}
            ]
        }}
      ]
    }
  }
}
```

- filter，exists(**查询不是null的数据**)

> SELECT 字段名称 FROM TABLE WHERE 字段名称 IS NOT NULL

```
{
  "query": {
    "bool": {
      "filter": {
        "exists": {
          "field": "字段名称"
        }
      }
    }
  }
}
```

- must_not,exists(**查询为null的数据**)

> SELECT 字段名称 FROM TABLE WHERE 字段名称 IS NULL

```
{
  "query": {
    "bool":{
      "must_not":{
        "exists":{
          "field": "字段名称"
        }
      }
    }
  }
}
```

- 使用_source可以定制返回结果

> 返回部分字段
```
{
    "_source":["field1","field2"],
    "query":{
    
  }
}
```
