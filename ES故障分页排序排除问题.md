### 问题现象

从ES查询出来的分页数据存在数据重复和错乱问题

#### 问题原因

因为使用了from/size模式进行分页查询，且使用了filtler过滤，导致没有相关性的评分，而且每次都是from/size 其相关性都是一致的，所以在灵界点会产生数据重复且错乱问题，以下是我们json,请求参数

```json
{
	"from": 430000,
	"size": 10000,
	"query": {
		"bool": {
			"must": [{
				"bool": {
					"filter": [{
						"term": {
							"trace_status": {
								"value": "1",
								"boost": 1
							}
						}
					}],
					"adjust_pure_negative": true,
					"boost": 1
				}
			}, {
				"bool": {
					"filter": [{
						"term": {
							"event_code": {
								"value": "WRITE_OFF_POINT",
								"boost": 1
							}
						}
					}],
					"adjust_pure_negative": true,
					"boost": 1
				}
			}, {
				"bool": {
					"filter": [{
						"term": {
							"saas_id": {
								"value": "2000001",
								"boost": 1
							}
						}
					}],
					"adjust_pure_negative": true,
					"boost": 1
				}
			}, {
				"bool": {
					"filter": [{
						"range": {
							"execute_time": {
								"from": "1625068800000",
								"to": "1627747199000",
								"include_lower": true,
								"include_upper": true,
								"boost": 1
							}
						}
					}],
					"adjust_pure_negative": true,
					"boost": 1
				}
			}, {
				"bool": {
					"must": [{
						"terms": {
							"settlement_party_id": ["20", "1001", "1002", "1003", "1004", "1005", "1006", "1007", "1008", "1009", "1010", "1011", "1012", "1013", "1014", "1015", "1016", "1017", "1018", "1019", "2001", "5001", "5002", "5004", "5005", "5006"],
							"boost": 1
						}
					}],
					"adjust_pure_negative": true,
					"boost": 1
				}
			}],
			"adjust_pure_negative": true,
			"boost": 1
		}
	},
	"_source": {
		"includes": ["update_time", "changedPoint", "deal_expire_time", "remain_quantity", "orderType", "out_shop_id", "brand_names", "shop_name", "shop_keeper", "province", "city", "district", "address", "description", "region_names", "dealer_names", "prize_pool_name", "settlement_party", "settlement_party_id", "extend", "create_time", "execute_time"],
		"excludes": []
	},
	"sort": [{
		"execute_time": {
			"order": "asc"
		}
	}]
}
```

#### 解决方案

1.简单解决，在排序这里再加一个唯一的排序字段例如：

```json
"sort": [{
		"execute_time": {
			"order": "asc"
		}
	},
	{
		"trace_no": {
			"order": "asc"
		}
	}
]
```

2.使用scroll这种模式来分页，这种的好处是对于深分页较为友好，但是对于搜索较差

3.通过将 track_scores 设置为 true，任然计算相关性,好处是改动较小，但是任然需要计算相关性，性能略微有些差

```json
GET /_search
{
    "track_scores": true,
    "sort" : [
        { "post_date" : {"order" : "desc"} },
        { "name" : "desc" },
        { "age" : "desc" }
    ],
    "query" : {
        "term" : { "user" : "kimchy" }
    }
}
```

### collapse + cardinality 去重统计分页精度问题

#### collapse

​	主要是去重，

