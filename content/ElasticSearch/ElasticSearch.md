---
title: "Fi"
date: 2026-02-09T23:21:15+08:00
draft: true
---

> 分布式搜索引擎

## 倒排索引
![[Pasted image 20250804172923.png]]
![[Pasted image 20250804172518.png]]
### 查询原理
![[Pasted image 20250804172459.png]]
## RESTful&JSON
- 在Web中资源通过URI定位
- 使用JSON为数据标准格式
## 分词器
> 把全文本转换成一系列单词**term/token**的过程 -分词
### 组成部分
#### character filter
接收原字符流 通过添加 删除 或者替换操作改变原字符流 可含有多个
> 例如:去除html标签 罗马数字转换 
#### tokenizer
文本拆分成多个词 在一个分词器中,有且只有一个tokenizer
>例如拆分英文，通过空格能将句子拆分成一个个的词，但是对于中文来说，无法使用这种方式来实现
#### token filters
将切分的单词添加 删除 或者改变 可以有零个或者多个`token filters`.
>例如将所有英文单词小写，或者将英文中的停词`a`删除等，在`token filters`中，不允许将`token(分出的词)`的`position`或者`offset`改变


### 自定义词库
![[Pasted image 20250805170653.png]]
## 索引库操作
### mapping常用属性
- **type**：字段数据类型
	- 字符串：text（可分词文本）keyword：精确值
	- 数值：long integer short byte double float
	- 布尔：boolean
	- 日期：date
	- 对象：object
- **index**：是否创建索引 （默认为true）
- **analyzer**：分词器
- properties：字段的子字段
### 创建索引库
```
PUT /索引库名称
{
	"mappings":{
		"properties":{
			"info":{
				"type":"text",
				"analyzer":"il_smart"
			},
			"email":{
				"type":"keyword",
				"index":"false"
			},
			"name":{
				"type":object,
				"properties":{
					"firstname":{
						"type":keyword
					},
					"lastname":{
						"type":"keyword"
					}
				}
			}
		}
	}
}
```
### 查询 删除 修改（仅限新增）索引库
#### 查询
```
GET /索引库名称
```
#### 删除
```
DELETE /索引库名称
```
#### 修改（新增字段）
```
PUT /索引表名称/_mapping
{
	"properties":{
		"age":{
			"type":"long"
		}
	}
}
```
### 文档操作
>每次写操作会新增版本
#### 新增
```
POST /索引库名称/_doc/文档id
{
	"字段"：“value”
	"字段"：{
		"字段"："value",
		"字段"："value"
	}
}
```
#### 查询
```
GET /索引库名称/_doc/文档id
```
#### 删除
```
DELETE /索引库名称/_doc/文档id
```
#### 修改
1. 全量修改（id不存在则为新增）
```
PUT /索引库名称/_doc/文档id
{
	"字段"：“value”
	"字段"：{
		"字段"："value",
		"字段"："value"
	}
}
```
2. 增量修改
```
POST /索引库名称/_update/文档id
{
	"doc":{
		"指定字段"："value"
	}
}
```

## DSL查询
- 查询所有：查询出所有数据（match_all）
- 全文检索：利用分词器对用户输入内容进行分词，然后利用倒排索引库查询
	- match_query
	- multi_match
- 精确查询
### 语法
```
GET /indexName/_search
{
	"query":{
		"查询类型":{
			"查询条件":"条件值"
		}
	}
}
```
### 全文检索查询
1. 分词
2. 查询
#### match查询：对用户输入内容进行分词查询
```
GET /indexName/_search
{
	"query":{
		"match":{
			"FIELD":"TEXT"
		}
	}
}
```
#### multi_match查询允许查询多个字段
```
GET /indexName/_search
{
	"query":{
		"multi_match":{
			"query":"TEXT",
			"FIELDs":["FIELD1","FIELD2"]
		}
	}
}
```
### 精确查询
#### term：根据词条精确值查询
```
GET /indexName/_search
{
	"query":{
		"term":{
			"FiELD":{
				"value":"TEXT"
			}
		}
	}
}
```
#### range查询：根据范围查询
> gte 大于等于 
> lte 小于等于
> gt 大于 
> lt 小于
```
GET /indexName/_search
{
	"query":{
		"range":{
			"FiELD":{
				"gte":VALUE,
				"lte":VALUE
			}
		}
	}
}
```
### 地理查询
>根据经纬度查询

#### 矩形区域内
```
GET /indexName/_search
{
	"query":{
		"geo_bounding_box":{
			"FiELD":{
				"top_left":{
					"lat":31.1,
					"lon":121.5
				},
				"bottom_right":{
					"lat":30.9,
					"lon":121.7
				}
			}
		}
	}
}
```
#### 指定中心点小于某距离
```
GET /indexName/_search
{
	"query":{
		"geo_distance":{
			"distance":"15km",
			"FIELD":"31.21,121.5"
		}
	}
}
```
### 复合查询
>将简单查询组合
#### Function Score Query
>算分函数查询 可以控制文档相关性算分 控制排名
![[Pasted image 20250806133730.png]]
#### Boolean Query
- **must**：与 必须匹配每个子查询
- **should**：或 选择性匹配
- **must_not**：必需不匹配 不参与算分
- **filter**:必需，不参与算分
### 检索结果排序
>对结果进行排序
>keyword类型 数值类型 地理坐标类型 日期类型
#### 对精确值
```
GET /indexName/_search
{
	"query":{
		"match_all":{}
	},
	"sort":[
		{
			//同时排序两种字段
			"FIELD":"desc",
			"FIELD":"asc"
			
		}
	]
}
```
#### 对地理坐标
```
GET /indexName/_search
{
	"query":{
		"match_all":{}
	},
	"sort":[
		{
			"_geo_distance":{
				"FIELD":"纬度，经度",
				"FIELD":{
					"lat":纬度,
					"lon":精度
				}
				"order":"asc",
				"unit":"km" 
			
			}
		}
	]
}
```
### 搜索结果分页
>底层上逻辑查询所有 然后根据from size截取数据
from
```
GET /indexName/_search
{
	"query":{
		"match_all":{}
	},
	"sort":[
		{
			//同时排序两种字段
			"FIELD":"desc",
			"FIELD":"asc"
			
		}
	],
	"from":0,
	"size":10
}
```
#### 深度分页
>由于集群化 导致难以快速查询大量数据
##### search after
分页时从上一次排序中开始查询下一页数据
##### scroll
将排序数据形成快照，保存在内存中
### 搜索结果高亮处理（结果添加标签）
```
GET /indexName/_search
{
	"query":{
		"查询类型":{
			"查询条件":"条件值"
		}
	},
	"highlight":{
		"fields":{
			"FIELD":{
				//默认采用完全匹配才添加标签
				"require_field_match":"false",
				"pre_tags":"<em>",
				"post_tags":"</em>"
			}
		}
	}
}
```
## RestClient操作索引库
>ES提供的组装DSL语句的客户端
![[Pasted image 20250805174448.png]]
### 模板操作
![[Pasted image 20250805175650.png]]

## RestClient操作文档
### 新增操作
![[Pasted image 20250805180845.png]]
### 查询操作
![[Pasted image 20250805181112.png]]
### 修改操作
![[Pasted image 20250805181357.png]]
### 删除操作
![[Pasted image 20250805181452.png]]
### 批量处理
![[Pasted image 20250805181830.png]]
### 条件查询
>基于模板查询 json格式解析数据即可
![[Pasted image 20250806144850.png]]
#### 全文检索查询
![[Pasted image 20250806145719.png]]
![[Pasted image 20250806145643.png]]
![[Pasted image 20250806145623.png]]
### 排序与分页
![[Pasted image 20250806150011.png]]
### 高亮
#### 封装条件
![[Pasted image 20250806150202.png]]
#### 高亮结果解析
![[Pasted image 20250806150441.png]]
## 聚合 aggregations
>实现对文档数据的**统计 分析 运算**
### Bucket 桶聚合
#### TermAggregation Date Histogram
>按照字段/日期阶梯分组
- 统计
![[Pasted image 20250806152929.png]]
![[Pasted image 20250806153117.png]]
### Metrics  度量聚合
>按照文档字段值分组
> 用于计算一些值 最大 最小 平均
![[Pasted image 20250806153723.png]]
![[Pasted image 20250806154012.png]]
### pipeline 管道聚合
>对其他聚合的结果为基础做聚合
## RestClient聚合
#### 封装条件
![[Pasted image 20250806154205.png]]


**聚合条件可多次aggregation**

#### 解析结果
![[Pasted image 20250806154537.png]]
### 带过滤条件的聚合
>聚合查询中添加query参数即可
### 自动补全查询
#### 拼音分词器
- **analyzer:pinyin**
- 汉字自动拆解为拼音
- 字段汉字首字母
#### 自定义分词器
- characterfilters:对文本进行处理：删除/替换特殊字符
- tokenizer：将文本按照一定规则拆分为词条term
- tokenizer filter:对词条进行进一步处理 大小写转换 同义词处理 拼音转换
创建索引库时，通过settings来配置自定义的analyzer
```
PUT /indexName
{
	"settings":{
		"analysis":{
			"analyzer":{//自定义分词器
				"my_analyzer":{//分词器名称
					"tokenizer":“ik_max_word",
					"filter":"pinyin"
				}
			},
			"filter":{//自定义filter
				"my_filter":{//通过github属性表修改 
					"type":pinyin",
					.......//
				}
			}
		}
	}
	"mapping":{
		"properties":{
			"FIELD":{
				"type":"text",
				"analyzer":"my_analyzer", //录入需要广泛扩充
				"search_analyzer":""ik_smart //搜索需要精确用户实际内容
			}
		}
	}
}
```
#### 自动补全查询
- 参与补全查询的字段必须是**completion**类型
- 字段内容一般是用来补全的多个词条形成的数组
```
GET /indexName/_search
{
	"suggest":{
		"MY_suggest":{ //自定义补全
			"text":"VALUE",//关键字
			"completion":{
				"field":"FIELD",查询字段
				"skip_duplicates":"true, //是否跳过重复
				"size":10 //展示数量
			}
		}
	}
}
```
### RestClient自动补全
![[Pasted image 20250807123724.png]]![[Pasted image 20250807124027.png]]
## 数据同步
>MySQL与ElasticSearch之间进行数据同步
###  同步调用
![[Pasted image 20250807132025.png]]
### 异步通知
![[Pasted image 20250807132351.png]]
### 监听binlog
![[Pasted image 20250807132423.png]]
## ES集群