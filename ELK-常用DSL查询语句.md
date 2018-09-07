### A.ELK-����DSL��ѯ���
- [kibana���þۺϲ�ѯDSL����¼](https://www.cnblogs.com/xionggeclub/p/7976011.html)
- [ElasticSearch AggregationBuilders java api���þۻ��ѯ](https://www.cnblogs.com/xionggeclub/p/7975982.html)

```
--------
GET winlogbeat-2017.11.*/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "term": {
            "Appname": {
              "value": "FTP"
            }
          }
        },
        {
          "range": {
            "@timestamp": {
              "gte": "2017-11-26T00:00:00.000+08:00",
              "lte": "2017-11-26T23:59:59.000+08:00"
            }
          }
        }
      ],
      "should": [
        {
          "term": {
            "action": {
              "value": "LIST"
            }
          }
        },
        {
          "term": {
            "action": {
              "value": "RETR"
            }
          }
        },
        {
          "term": {
            "action": {
              "value": "STOR"
            }
          }
        },
        {
          "term": {
            "action": {
              "value": "DELE"
            }
          }
        }
      ],
      "minimum_number_should_match": 1,
      "must_not": [
        {
          "term": {
            "filedir": {
              "value": "/%{[filesub][1]}"
            }
          }
        },{
          "term": {
            "filedir": {
              "value": "-"
            }
          }
        },{
          "match": {
            "message": ".ok"
          }
        }
      ]
    }
  }
}
 
GET winlogbeat-2017.11.*/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "term": {
            "Appname": {
              "value": "FTP"
            }
          }
        },
        {
          "terms": {
            "action": [
              "RETR"
            ]
          }
        },
        {
          "range": {
            "@timestamp": {
              "gte": "2017-11-26T00:00:00.000+08:00",
              "lte": "2017-11-26T23:59:59.000+08:00"
            }
          }
        }
      ],
      "must_not": [
        {
          "term": {
            "filedir": {
              "value": "/%{[filesub][1]}"
            }
          }
        },{
          "term": {
            "filedir": {
              "value": "-"
            }
          }
        },{
          "match": {
            "message": ".ok"
          }
        }
      ]
    }
  }
}
------
GET winlogbeat-2017.11.25,winlogbeat-2017.11.26/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "term": {
            "Appname": {
              "value": "FTP"
            }
          }
        },
        {
          "terms": {
            "action": [
              "LIST",
              "DELE",
              "RETR",
              "STOR"
            ]
          }
        },
        {
          "range": {
            "@timestamp": {
              "gte": "2017-11-26T00:00:00.000+08:00",
              "lte": "2017-11-26T23:59:59.000+08:00"
            }
          }
        }
      ],
      "must_not": [
        {
          "term": {
            "filedir": {
              "value": "/%{[filesub][1]}"
            }
          }
        },{
          "term": {
            "filedir": {
              "value": "-"
            }
          }
        },{
          "match": {
            "message": ".ok"
          }
        }
      ]
    }
  },
  "size": 0,
  "aggs": {
    "ff": {
      "terms": {
        "field": "filedir",
        "size": 100
      }
    }
  }
}
------
GET winlogbeat-*/_search
{
  "size": 0,
  "query" : {
    "bool" : {
      "must" : [
        {
          "range" : {
            "@timestamp" : {
              "from" : 1511654400000,
              "to" : 1511740800000,
              "include_lower" : true,
              "include_upper" : true,
              "boost" : 1.0
            }
          }
        },
        {
          "term" : {
            "Appname" : {
              "value" : "FTP",
              "boost" : 1.0
            }
          }
        },
        {
          "terms" : {
            "action" : [
              "LIST",
              "STOR",
              "DELE",
              "RETR"
            ],
            "boost" : 1.0
          }
        }
      ],
      "must_not" : [
        {
          "match" : {
            "message" : {
              "query" : ".ok",
              "operator" : "OR",
              "prefix_length" : 0,
              "max_expansions" : 50,
              "fuzzy_transpositions" : true,
              "lenient" : false,
              "zero_terms_query" : "NONE",
              "boost" : 1.0
            }
          }
        },
        {
          "term" : {
            "filedir" : {
              "value" : "-",
              "boost" : 1.0
            }
          }
        },
        {
          "match" : {
            "filedir" : {
              "query" : "/%{[filesub][1]}",
              "operator" : "OR",
              "prefix_length" : 0,
              "max_expansions" : 50,
              "fuzzy_transpositions" : true,
              "lenient" : false,
              "zero_terms_query" : "NONE",
              "boost" : 1.0
            }
          }
        }
      ],
      "disable_coord" : false,
      "adjust_pure_negative" : true,
      "boost" : 1.0
    }
  },
  "aggregations" : {
    "filedir_count" : {
      "terms" : {
        "field" : "filedir",
        "size" : 10,
        "shard_size" : -1,
        "min_doc_count" : 1,
        "shard_min_doc_count" : 0,
        "show_term_doc_count_error" : false,
        "order" : [
          {
            "_count" : "desc"
          },
          {
            "_term" : "asc"
          }
        ]
      }
    }
  },
  "ext" : { }
}
---------
RPT_C001_20171125.txt
 
GET winlogbeat-2017.11.25,winlogbeat-2017.11.26/_search
{
  "size": 0,
  "query" : {
    "bool" : {
      "must" : [
        {
          "range" : {
            "@timestamp" : {
              "from" : "2017-11-26T00:00:00.000+08:00",
              "to" : "2017-11-26T23:59:59.000+08:00",
              "include_lower" : true,
              "include_upper" : true,
              "boost" : 1.0
            }
          }
        },
        {
          "term" : {
            "Appname" : {
              "value" : "FTP",
              "boost" : 1.0
            }
          }
        },
        {
          "terms" : {
            "action" : [
              "LIST",
              "STOR",
              "DELE",
              "RETR"
            ],
            "boost" : 1.0
          }
        }
      ],
      "must_not" : [
        {
          "match" : {
            "message" : {
              "query" : ".ok",
              "operator" : "OR",
              "prefix_length" : 0,
              "max_expansions" : 50,
              "fuzzy_transpositions" : true,
              "lenient" : false,
              "zero_terms_query" : "NONE",
              "boost" : 1.0
            }
          }
        },
        {
          "term" : {
            "filedir" : {
              "value" : "-",
              "boost" : 1.0
            }
          }
        },
        {
          "match" : {
            "filedir" : {
              "query" : "/%{[filesub][1]}",
              "operator" : "OR",
              "prefix_length" : 0,
              "max_expansions" : 50,
              "fuzzy_transpositions" : true,
              "lenient" : false,
              "zero_terms_query" : "NONE",
              "boost" : 1.0
            }
          }
        }
      ],
      "disable_coord" : false,
      "adjust_pure_negative" : true,
      "boost" : 1.0
    }
  },
  "aggregations" : {
    "aggTop" : {
      "terms" : {
        "field" : "filedir",
        "size" : 50,
        "shard_size" : -1,
        "min_doc_count" : 1,
        "shard_min_doc_count" : 0,
        "show_term_doc_count_error" : false,
        "order" : [
          {
            "_count" : "desc"
          },
          {
            "_term" : "asc"
          }
        ]
      }
    }
  },
  "ext" : { }
}
 
GET winlogbeat-2017.11.25,winlogbeat-2017.11.26/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "term": {
            "Appname": {
              "value": "FTP"
            }
          }
        },
        {
          "terms": {
            "action": [
              "LIST",
              "DELE",
              "RETR",
              "STOR"
            ]
          }
        },
        {
          "range": {
            "@timestamp": {
              "gte": "2017-11-26T00:00:00.000+08:00",
              "lte": "2017-11-26T23:59:59.000+08:00"
            }
          }
        }
      ],
      "must_not": [
        {
          "term": {
            "filedir": {
              "value": "/%{[filesub][1]}"
            }
          }
        },{
          "term": {
            "filedir": {
              "value": "-"
            }
          }
        },{
          "match": {
            "message": ".ok"
          }
        }
      ]
    }
  },
  "size": 0,
  "aggs": {
    "ff": {
      "terms": {
        "field": "filedir",
        "size": 100
      }
    }
  }
}
 
GET winlogbeat-2017.11.*/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "term": {
            "Appname": {
              "value": "FTP"
            }
          }
        },
        {
          "term": {
            "filedir": {
              "value": "/SJPT"
            }
          }
        },
        {
          "terms": {
            "action": [
              "LIST",
              "DELE",
              "RETR",
              "STOR"
            ]
          }
        },
        {
          "range": {
            "@timestamp": {
              "gte": "2017-11-26T00:00:00.000+08:00",
              "lte": "2017-11-26T23:59:59.000+08:00"
            }
          }
        }
      ],
      "must_not": [
        {
          "term": {
            "filedir": {
              "value": "/%{[filesub][1]}"
            }
          }
        },{
          "term": {
            "filedir": {
              "value": "-"
            }
          }
        },{
          "match": {
            "message": ".ok"
          }
        }
      ]
    }
  }
}
------
 
GET winlogbeat-2017.11.*/_search
{
  "size": 0,
  "query" : {
    "bool" : {
      "must" : [
        {
          "range" : {
            "@timestamp" : {
              "from" : "2017-11-26T00:00:00.000+08:00",
              "to" : "2017-11-26T23:59:59.000+08:00",
              "include_lower" : true,
              "include_upper" : true,
              "boost" : 1.0
            }
          }
        },
        {
          "term" : {
            "Appname" : {
              "value" : "FTP",
              "boost" : 1.0
            }
          }
        },
        {
          "term" : {
            "action" : {
              "value" : "LIST",
              "boost" : 1.0
            }
          }
        }
      ],
      "must_not" : [
        {
          "match" : {
            "message" : {
              "query" : ".ok",
              "operator" : "OR",
              "prefix_length" : 0,
              "max_expansions" : 50,
              "fuzzy_transpositions" : true,
              "lenient" : false,
              "zero_terms_query" : "NONE",
              "boost" : 1.0
            }
          }
        },
        {
          "term" : {
            "filedir" : {
              "value" : "-",
              "boost" : 1.0
            }
          }
        },
        {
          "match" : {
            "filedir" : {
              "query" : "/%{[filesub][1]}",
              "operator" : "OR",
              "prefix_length" : 0,
              "max_expansions" : 50,
              "fuzzy_transpositions" : true,
              "lenient" : false,
              "zero_terms_query" : "NONE",
              "boost" : 1.0
            }
          }
        }
      ],
      "disable_coord" : false,
      "adjust_pure_negative" : true,
      "boost" : 1.0
    }
  },
  "aggregations" : {
    "aggTop" : {
      "terms" : {
        "field" : "filedir",
        "size" : 50,
        "shard_size" : -1,
        "min_doc_count" : 1,
        "shard_min_doc_count" : 0,
        "show_term_doc_count_error" : false,
        "order" : [
          {
            "_count" : "desc"
          },
          {
            "_term" : "asc"
          }
        ]
      }
    },
    "aggList" : {
      "terms" : {
        "field" : "account",
        "size" : 50,
        "shard_size" : -1,
        "min_doc_count" : 1,
        "shard_min_doc_count" : 0,
        "show_term_doc_count_error" : false,
        "order" : [
          {
            "_count" : "desc"
          },
          {
            "_term" : "asc"
          }
        ]
      }
    }
  },
  "ext" : { }
}
 
 
GET winlogbeat-2017.11.*/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "term": {
            "Appname": {
              "value": "FTP"
            }
          }
        },
        {
          "term": {
            "account": {
              "value": "ICCCUAT\\uatjc06400"
            }
          }
        },
        {
          "terms": {
            "action": [
              "LIST"
            ]
          }
        },
        {
          "range": {
            "@timestamp": {
              "gte": "2017-11-26T00:00:00.000+08:00",
              "lte": "2017-11-26T23:59:59.000+08:00"
            }
          }
        }
      ],
      "must_not": [
        {
          "term": {
            "filedir": {
              "value": "/%{[filesub][1]}"
            }
          }
        },{
          "term": {
            "filedir": {
              "value": "-"
            }
          }
        },{
          "match": {
            "message": ".ok"
          }
        }
      ]
    }
  }
}
```
```
����Ա��ϢΪ����player������player type����5���ֶΣ����������䣬нˮ����ӣ�����λ�á�
index��mappingΪ��
//
"mappings": {
    "player": {
        "properties": {
            "name": {
                "index": "not_analyzed",
                "type": "string"
            },
            "age": {
                "type": "integer"
            },
            "salary": {
                "type": "integer"
            },
            "team": {
                "index": "not_analyzed",
                "type": "string"
            },
            "position": {
                "index": "not_analyzed",
                "type": "string"
            }
        },
        "_all": {
            "enabled": false
        }
    }
}
����


�����е�ȫ�����ݣ�


΢�Ž�ͼ_20160920171030.png

 
���ȣ���ʼ��Builder��

1
SearchRequestBuilder sbuilder = client.prepareSearch("player").setTypes("player");
����

����������˵�����־ۺϲ�����ʵ�ַ�������Ϊ��es��api�У����ֶ��ϵľۺϲ�����Ҫ�õ��Ӿۺ�(subAggregation)����ѧ�߿����Ҳ����������������ϱȽ��٣�������������������������죬������Դ��ų��׸����T_T������߻�����˵�����ֶξۺϵ�ʵ�ַ��������⣬�ۺϺ������Ҳ�ᵥ��˵����

group by/count
����Ҫ����ÿ����ӵ���Ա�������ʹ��SQL��䣬Ӧ������£�

select team, count(*) as player_count from player group by team;
ES��java api��
//
TermsBuilder teamAgg= AggregationBuilders.terms("player_count ").field("team");
sbuilder.addAggregation(teamAgg);
SearchResponse response = sbuilder.execute().actionGet();
����

 

group by���field
����Ҫ����ÿ�����ÿ��λ�õ���Ա�������ʹ��SQL��䣬Ӧ������£�

select team, position, count(*) as pos_count from player group by team, position;
ES��java api��
//
TermsBuilder teamAgg= AggregationBuilders.terms("player_count ").field("team");
TermsBuilder posAgg= AggregationBuilders.terms("pos_count").field("position");
sbuilder.addAggregation(teamAgg.subAggregation(posAgg));
SearchResponse response = sbuilder.execute().actionGet();
����

 

max/min/sum/avg
����Ҫ����ÿ������������/��С/��/ƽ������Ա���䣬���ʹ��SQL��䣬Ӧ������£�

select team, max(age) as max_age from player group by team;
ES��java api��
//
TermsBuilder teamAgg= AggregationBuilders.terms("player_count ").field("team");
MaxBuilder ageAgg= AggregationBuilders.max("max_age").field("age");
sbuilder.addAggregation(teamAgg.subAggregation(ageAgg));
SearchResponse response = sbuilder.execute().actionGet();
����

 

�Զ��field��max/min/sum/avg
����Ҫ����ÿ�������Ա��ƽ�����䣬ͬʱ��Ҫ��������н�����ʹ��SQL��䣬Ӧ������£�

select team, avg(age)as avg_age, sum(salary) as total_salary from player group by team;
ES��java api��

//
TermsBuilder teamAgg= AggregationBuilders.terms("team");
AvgBuilder ageAgg= AggregationBuilders.avg("avg_age").field("age");
SumBuilder salaryAgg= AggregationBuilders.avg("total_salary ").field("salary");
sbuilder.addAggregation(teamAgg.subAggregation(ageAgg).subAggregation(salaryAgg));
SearchResponse response = sbuilder.execute().actionGet();
����

�ۺϺ��Aggregation�������
����Ҫ����ÿ���������н������������н�������У����ʹ��SQL��䣬Ӧ������£�

select team, sum(salary) as total_salary from player group by team order by total_salary desc;
ES��java api��
//
TermsBuilder teamAgg= AggregationBuilders.terms("team").order(Order.aggregation("total_salary ", false);
SumBuilder salaryAgg= AggregationBuilders.avg("total_salary ").field("salary");
sbuilder.addAggregation(teamAgg.subAggregation(salaryAgg));
SearchResponse response = sbuilder.execute().actionGet();
����

��Ҫ�ر�ע����ǣ���������TermAggregation��ִ�еģ�Order.aggregation�����ĵ�һ��������aggregation�����֣��ڶ���������boolean�ͣ�true��ʾ����false��ʾ���� 

Aggregation�������������
Ĭ������£�searchִ�к󣬽�����10���ۺϽ��������뷴�ڸ���Ľ������Ҫ�ڹ���TermsBuilder ʱָ��size��

TermsBuilder teamAgg= AggregationBuilders.terms("team").size(15);
 

Aggregation����Ľ���/���
�õ�response��

Map<String, Aggregation> aggMap = response.getAggregations().asMap();
StringTerms teamAgg= (StringTerms) aggMap.get("keywordAgg");
Iterator<Bucket> teamBucketIt = teamAgg.getBuckets().iterator();
while (teamBucketIt .hasNext()) {
Bucket buck = teamBucketIt .next();
//�����
String team = buck.getKey();
//��¼��
long count = buck.getDocCount();
//�õ������Ӿۺ�
Map subaggmap = buck.getAggregations().asMap();
//avgֵ��ȡ����
double avg_age= ((InternalAvg) subaggmap.get("avg_age")).getValue();
//sumֵ��ȡ����
double total_salary = ((InternalSum) subaggmap.get("total_salary")).getValue();
//...
//max/min�Դ�����
}
����

 

�ܽ�
���ϣ��ۺϲ�����Ҫ�ǵ�����SearchRequestBuilder��addAggregation������ͨ���Ǵ���һ��TermsBuilder���Ӿۺϵ���TermsBuilder��subAggregation������������ӵ��Ӿۺ���TermsBuilder��SumBuilder��AvgBuilder��MaxBuilder��MinBuilder�ȳ����ľۺϲ�����
 
��ʵ����������SearchRequestBuilder���ڲ�������һ��˽�е� SearchSourceBuilderʵ���� SearchSourceBuilder�ڲ�����һ��List<AbstractAggregationBuilder>��ÿ�ε���addAggregationʱ����� SearchSourceBuilderʵ�������һ��AggregationBuilder��
ͬ���ģ�TermsBuilderҲ���ڲ�������һ��List<AbstractAggregationBuilder>������addAggregation���������Ը���addAggregation��ʱ�����һ��AggregationBuilder������Ȥ�Ķ���Ҳ�����Ķ�Դ���ʵ�֡�
 
�����ʲô���⣬��ӭһ�����ۣ����������ʲô���󣬻�ӭ����ָ����
```