---
layout: post
title:  "ElasticSearch - 01.index_document_CRUD_mapping"
categories: ELK
tags: elk elasticsearch
---

# Elasticserarch
```
curl -X GET http://localhost:9200?pretty
{
  "name" : "Vett6lF",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "-GeFznOrQkqjg7qpyNysiw",
  "version" : {
    "number" : "6.4.2",
    "build_flavor" : "default",
    "build_type" : "tar",
    "build_hash" : "04711c2",
    "build_date" : "2018-09-26T13:34:09.098244Z",
    "build_snapshot" : false,
    "lucene_version" : "7.4.0",
    "minimum_wire_compatibility_version" : "5.6.0",
    "minimum_index_compatibility_version" : "5.0.0"
  },
  "tagline" : "You Know, for Search"
}
```

## 인덱스 확인
```
curl -X GET http://localhost:9200/classes?pretty
```

## index 생성
```
curl -X PUT http://localhost:9200/classes
```

## index 삭제
```
curl -X DELETE http://localhost:9200/classes
```

## Document 생성
```
curl -XPOST http://locahost:9200/classes/class/1/ -H ‘Content-Type: application/json’ -d ’{“title”:"Algorithm","professor":"John"}'
```

### 확인
```
curl -X GET http://localhost:9200/classes/class/1?pretty
```

## Document 생성 with file
```
curl -X POST http://localhost:9200/classes/class/1/ -H 'Content-Type:application/json' -d @test.json
```

## update
```
curl -XPOST http://localhost:9200/classes/class/1/_update?pretty -H 'Content-Type: application/json' -d'
{"doc":{"unit":1}}'
```

### update one field with script
```
curl -XPOST http://localhost:9200/classes/class/1/_update? -H 'Content-Type: application/json' -d'
{"script":"ctx._source.unit +=5"}'
```

## bulk update
```
curl -XPOST http://localhost:9200/_bulk?pretty -H 'Content-Type: application/json' --data-binary @classes.json
```

## bulk mapping
```
curl -X PUT "localhost:9200/classes/_mapping/class" -H 'Content-Type: application/json' -d @classesRating_mapping.json
```

ElasticSearch 6.x 부터는 mapping type 중 string 이 text 로 변경되었다.

classesRating_mapping.json
```
{
	"properties" : {
		"title" : {
			"type" : "text"
		},
		"professor" : {
			"type" : "text"
		},
		"major" : {
			"type" : "text"
		},
		"semester" : {
			"type" : "text"
		},
		"student_count" : {
			"type" : "integer"
		},
		"unit" : {
			"type" : "integer"
		},
		"rating" : {
			"type" : "integer"
		},
		"submit_date" : {
			"type" : "date",
			"format" : "yyyy-MM-dd"
		},
		"school_location" : {
			"type" : "geo_point"
		}
	}
}
```
이렇게 type mapping을 한 후 다시 bulk insert를 해주면 정확한 type대로 데이터를 넣는다.

