---
layout: post
title:  "ElasticSearch - 02.Search, Aggregation"
categories: ELK
tags: elk elasticsearch
---

# Search
우선 샘플데이터 삽입
```
curl -X POST 'localhost:9200/_bulk' -H 'Content-Type: application/json' --data-binary @simple_basketball.json
```

확인
```
curl -XGET localhost:9200/basketball/record/_search?pretty
```

points=30 검색
```
curl -X GET 'localhost:9200/basketball/record/_search?q=points:30&pretty' -H 'Content-Type: application/json'
```

request body를 사용한 검색
```
curl -X GET 'localhost:9200/basketball/record/_search' -H 'Content-Type: application/json' -d' 
{
   "query" : {
     "term" : { "points" : 30 }
   }
}'
```

# Metric Aggregation
평균, 최대 최소와 같은 산술 조합 값을 구해준다.
simple_basketball.json 데이터를 삽입한다.

```bash
curl -X GET localhost:9200/_search?pretty -H 'Content-Type: application/json' --data-binary @avg_points_aggs.json
```
이렇게 평균을 구할 수 있는데 이 말고도 json 형태로 max, min, avg, sum 등을 구할 수 있따.


avg_points_aggs.json
```json
{
	"size" : 0,
	"aggs" : {
		"avg_score" : {
			"avg" : {
				"field" : "points"
			}
		}
	}
}
```

max_points_aggs.json
```json
{
	"size" : 0,
	"aggs" : {
		"max_score" : {
			"max" : {
				"field" : "points"
			}
		}
	}
}
```

min_points_aggs.json
```json
{
	"size" : 0,
	"aggs" : {
		"min_score" : {
			"min" : {
				"field" : "points"
			}
		}
	}
}
```

sum_points_aggs.json
```json
{
	"size" : 0,
	"aggs" : {
		"sum_score" : {
			"sum" : {
				"field" : "points"
			}
		}
	}
}
```

위의 4가지를 한번에 모두다 구하려면
stats_points_aggs.json
```json
{
	"size" : 0,
	"aggs" : {
		"stats_score" : {
			"stats" : {
				"field" : "points"
			}
		}
	}
}
```

```
curl -X GET localhost:9200/_search?pretty -H 'Content-Type: application/json' --data-binary @stats_points_aggs.json
{
  "took" : 6,
  "timed_out" : false,
  "_shards" : {
    "total" : 25,
    "successful" : 25,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : 27,
    "max_score" : 0.0,
    "hits" : [ ]
  },
  "aggregations" : {
    "stats_score" : {
      "count" : 2,
      "min" : 20.0,
      "max" : 30.0,
      "avg" : 25.0,
      "sum" : 50.0
    }
  }
}
```

# Bucket Aggregation
group by 라고 생각하면 됨. 팀별 수치 등을 구할 때

basketball index 생성
```
curl -X PUT localhost:9200/basketball
```

아래의 mapping 파일대로 type을 미리 매핑시켜둔다.
basketball_mapping.json
```json
{
	"record" : {
		"properties" : {
			"team" : {
				"type" : "text",
				"fielddata" : true
			},
			"name" : {
				"type" : "text",
				"fielddata" : true
			},
			"points" : {
				"type" : "long"
			},
			"rebounds" : {
				"type" : "long"
			},
			"assists" : {
				"type" : "long"
			},
			"blocks" : {
				"type" : "long"
			},
			"submit_date" : {
				"type" : "date",
				"format" : "yyyy-MM-dd"
			}
		}
	}
}
```

```
curl -X PUT 'localhost:9200/basketball/record/_mapping' -H 'Content-Type: application/json' -d @basketball_mapping.json
{"acknowledged":true}
```

이제 아래의 team data를 insert 한다.
twoteam_basketball.json
```json
{ "index" : { "_index" : "basketball", "_type" : "record", "_id" : "1" } }
{"team" : "Chicago","name" : "Michael Jordan", "points" : 30,"rebounds" : 3,"assists" : 4, "blocks" : 3, "submit_date" : "1996-10-11"}
{ "index" : { "_index" : "basketball", "_type" : "record", "_id" : "2" } }
{"team" : "Chicago","name" : "Michael Jordan","points" : 20,"rebounds" : 5,"assists" : 8, "blocks" : 4, "submit_date" : "1996-10-13"}
{ "index" : { "_index" : "basketball", "_type" : "record", "_id" : "3" } }
{"team" : "LA","name" : "Kobe Bryant","points" : 30,"rebounds" : 2,"assists" : 8, "blocks" : 5, "submit_date" : "2014-10-13"}
{ "index" : { "_index" : "basketball", "_type" : "record", "_id" : "4" } }
{"team" : "LA","name" : "Kobe Bryant","points" : 40,"rebounds" : 4,"assists" : 8, "blocks" : 6, "submit_date" : "2014-11-13"}
```

```json
curl -X POST 'localhost:9200/_bulk' -H 'Content-Type: application/json' --data-binary @twoteam_basketball.json
```

아래는 group by할 terms_aggs.json 파일 이다.
```json
{
	"size" : 0,
	"aggs" : {
		"players" : {
			"terms" : {
				"field" : "team"
			}
		}
	}
}
```

```
curl -X GET localhost:9200/_search?pretty -H 'Content-Type: application/json' --data-binary @terms_aggs.json
{
  "took" : 21,
  "timed_out" : false,
  "_shards" : {
    "total" : 30,
    "successful" : 30,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : 29,
    "max_score" : 0.0,
    "hits" : [ ]
  },
  "aggregations" : {
    "players" : {
      "doc_count_error_upper_bound" : 0,
      "sum_other_doc_count" : 0,
      "buckets" : [
        {
          "key" : "chicago",
          "doc_count" : 2
        },
        {
          "key" : "la",
          "doc_count" : 2
        }
      ]
    }
  }
}
```


stats_by_team.json
```json
{
	"size" : 0,
	"aggs" : {
		"team_stats" : {
			"terms" : {
				"field" : "team"
			},
			"aggs" : {
				"stats_score" : {
					"stats" : {
						"field" : "points"
					}
				}
			}
		}
	}
}
```
```
curl -X GET localhost:9200/_search?pretty -H 'Content-Type: application/json' --data-binary @stats_by_team.json
{
  "took" : 7,
  "timed_out" : false,
  "_shards" : {
    "total" : 30,
    "successful" : 30,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : 29,
    "max_score" : 0.0,
    "hits" : [ ]
  },
  "aggregations" : {
    "team_stats" : {
      "doc_count_error_upper_bound" : 0,
      "sum_other_doc_count" : 0,
      "buckets" : [
        {
          "key" : "chicago",
          "doc_count" : 2,
          "stats_score" : {
            "count" : 2,
            "min" : 20.0,
            "max" : 30.0,
            "avg" : 25.0,
            "sum" : 50.0
          }
        },
        {
          "key" : "la",
          "doc_count" : 2,
          "stats_score" : {
            "count" : 2,
            "min" : 30.0,
            "max" : 40.0,
            "avg" : 35.0,
            "sum" : 70.0
          }
        }
      ]
    }
  }
}
```