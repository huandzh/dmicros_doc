# Dmicros用户指南

欢迎阅读！

Dmicros API是提供数据服务的工具API，为用户提供可用于可视化的数据和在线模型计算。

API的地址是[dmicros.iamhd.top](https://dmicros.iamhd.top).

## 前言

API遵循RESTFul标准，使用pyeve软件包开发，本份指南中将只包括Dmicros独有的设置，其他未尽事项（如错误返回格式等）请您参考[pyeve文档](http://python-eve.org)。

如果您有任何问题，都可以到[本份文档的托管项目](https://coding.net/u/huand/p/dmicros_doc/topic)进行提问和讨论。

以下均使用`curl`命令来示例请求。示例中的具体数据不是真实数据，主要供参考格式。

## 获取API令牌（token）

请通过[邮件联系API管理员: hd@iamhd.top](mailto:hd@iamhd.top)获取API令牌，API的每项服务都需要使用令牌才能访问，每项服务均有自己的权限要求。


## 1. API 服务状态

该API返回可用的服务项目。

**HTTP Request:**

`GET https://dmicros.iamhd.top`

**示例**

```shell
curl https://dmicros.iamhd.top  -H "Authorization:<your access token>"
```

**返回：**

```json
{
  "_links": {
    "child": [
      {
        "href": "vdatas",
        "title": "vdatas"
      },
      {
        "href": "clusteds",
        "title": "clusteds"
      },
      {
        "href": "clustedBatchs",
        "title": "clustedBatchs"
      },
      {
        "href": "tokens",
        "title": "tokens"
      },
      {
        "href": "quotas",
        "title": "quotas"
      }
    ]
  }
}
```

## 2. vdatas API : 可视化数据

该API返回可用于可视化的设置和数据，相应的数据可以应用到[Echarts](http://www.echartsjs.com)上进行可视化。

### 2.1 vdatas 列表

该API将返回您有权查看的可视化数据列表。

**HTTP Request:**

`GET https://dmicros.iamhd.top/vdatas`

**示例**

```shell
curl https://dmicros.iamhd.top/vdatas  -H "Authorization:<your access token>"
```

**返回：**

```json
{
  "_items": [
    {
      "_id": "5a6ee4d3c78ef30313bcbad5",
      "_created": "Mon, 29 Jan 2018 09:09:39 GMT",
      "_etag": "a617765a5f35132c19e1138f6bf60fd7c1597335",
      "_updated": "Mon, 29 Jan 2018 09:09:39 GMT",
      "data": <your visualization data>,
      "description": "用红蓝分布图表示人群分布差异，红色代表该人群在英朗中占比高于轿车A级人群",
      "name": "cluster_yinglang",
      "vtype": "consumerCluster",
      "tags": [
        "人群分类"
      ],
      "title": "英朗和轿车A级人群分布差异",
      "_links": {
        "self": {
          "title": "Vdata",
          "href": "vdatas/5a6ee4d3c78ef30313bcbad5"
        }
      }
    }
  ],
  "_links": {
    "parent": {
      "title": "home",
      "href": "/"
    },
    "self": {
      "title": "vdatas",
      "href": "vdatas"
    }
  },
  "_meta": {
    "page": 1,
    "max_results": 25,
    "total": 1
  }
}
```

### 2.2 vdatas 单项

该API将返回单项可视化数据，地址结尾可以使用`数据id`或`数据名称`，下面以上节列表示例中的文档为例。

**HTTP Request:**

`GET https://dmicros.iamhd.top/vdatas/5a6ee4d3c78ef30313bcbad5`

或

`GET https://dmicros.iamhd.top/vdatas/cluster_yinglang`

**示例**

```shell
curl https://dmicros.iamhd.top/vdatas/5a6ee4d3c78ef30313bcbad5  -H "Authorization:<your access token>"
```

或

```shell
curl https://dmicros.iamhd.top/vdatas/cluster_yinglang  -H "Authorization:<your access token>"
```

**返回：**

```json
{
  "_id": "5a6ee4d3c78ef30313bcbad5",
  "_created": "Mon, 29 Jan 2018 09:09:39 GMT",
  "_etag": "a617765a5f35132c19e1138f6bf60fd7c1597335",
  "_updated": "Mon, 29 Jan 2018 09:09:39 GMT",
  "data": <your visualization data>,
  "description": "用红蓝分布图表示人群分布差异，红色代表该人群在英朗中占比高于轿车A级人群",
  "name": "cluster_yinglang",
  "vtype": "consumerCluster",
  "tags": [
    "人群分类"
  ],
  "title": "英朗和轿车A级人群分布差异",
  "_links": {
    "parent": {
      "title": "home",
      "href": "/"
    },
    "self": {
      "title": "Vdata",
      "href": "vdatas/5a6ee4d3c78ef30313bcbad5"
    },
    "collection": {
      "title": "vdatas",
      "href": "vdatas"
    }
  }
}
```

## 3. quotas API : 配额

该API将返回服务资源配额及使用记录。会消费配额的服务有：

* **创建`clusteds`**
* **创建`clustedBatchs`**


**HTTP Request:**

`GET https://dmicros.iamhd.top/quotas`

**示例**

```shell
curl https://dmicros.iamhd.top/quotas  -H "Authorization:<your access token>"
```

**返回：**

```json
{
  "_items": [
    {
      "_id": "5a862f11c3666e1e5654fce9",
      "token": "<your access token>",
      "resource": "clusteds",
      "quota": 4933,
      "quota_acc": 5000,
      "consumptions": [
        ...
        2,
        2,
        1
      ],
      "_updated": "Fri, 16 Feb 2018 01:08:33 GMT",
      "_created": "Fri, 16 Feb 2018 01:08:33 GMT",
      "_etag": "09e12f466cb3ac1d25251a460126412688c3f174",
      "datetimes": [
	    ...
        "Thu, 22 Feb 2018 18:11:38 GMT",
        "Fri, 23 Feb 2018 10:19:16 GMT",
        "Sat, 24 Feb 2018 12:59:30 GMT"
      ],
      "_links": {
        "self": {
          "title": "Quota",
          "href": "quotas/5a862f11c3666e1e5654fce9"
        }
      }
    }
  ],
  "_links": {
    "parent": {
      "title": "home",
      "href": "/"
    },
    "self": {
      "title": "quotas",
      "href": "quotas"
    }
  },
  "_meta": {
    "page": 1,
    "max_results": 25,
    "total": 1
  }
}
```

其中：

* `"resource": "clusteds"` : 对应的服务资源
* `"quota": 4933` : 配额余额
* `"quota_acc": 5000` : 配额总量
* `"consumptions": [..., 2, 1]` : 消费数量记录
* `"datetimes": [ ..., "Thu, 22 Feb 2018 18:11:38 GMT",... ]` : 消费时间记录

## 4. clusteds API : 人群分类结果

该API返回人群分类结果，一般来说您需要分两步来使用：

 1. 先创建人群分类结果，存储到服务器上
 2. 读取服务器上的结果

其中创建的过程需要**消费该项资源的配额**，如果您没有足够的配额，服务器将会返回402错误。

### 4.1 POST : 创建人群分类结果

该API将创建人群分类结果，输入的数据格式为json：

```json
{
  "raw": "1,RAV4,5,,6,7,14,1,7,3,5,7,5,6,5,6,6,5,5,3,4,6,6,5,2,6,4,5,6,5,6,5,5,7,7,6,2,7,6,5,5,7,5,7,5,6,6,3,6"
}
```

字段`raw`的值即人群分类语句模块问卷测试记录，每一问题得分间用半角逗号`,`隔开。

该API也支持批量创建，输入格式为json array：

```json
[
    {
    "raw": "1,RAV4,5,,6,7,14,1,7,3,5,7,5,6,5,6,6,5,5,3,4,6,6,5,2,6,4,5,6,5,6,5,5,7,7,6,2,7,6,5,5,7,5,7,5,6,6,3,6"
    },
	...
]
```

**HTTP Request:**

`POST https://dmicros.iamhd.top/clusteds`

**示例**

```shell
curl -XPOST https://dmicros.iamhd.top/clusteds  -H "Authorization:<your access token>" -H "Content-Type:application/json" -d '{"raw":"1,RAV4,5,,6,7,14,1,7,3,5,7,5,6,5,6,6,5,5,3,4,6,6,5,2,6,4,5,6,5,6,5,5,7,7,6,2,7,6,5,5,7,5,7,5,6,6,3,6"}'
```

**返回：**

返回中的`summary`包括计算结果，你也可以使用`"href": "clusteds/5a90f132c78ef35c4730d0df"`取回结果。

```json
{
  "_updated": "Sat, 24 Feb 2018 04:59:30 GMT",
  "_created": "Sat, 24 Feb 2018 04:59:30 GMT",
  "_etag": "1b55669cac50b77df2148c8647865aa47dc6ef58",
  "_id": "5a90f132c78ef35c4730d0df",
  "_links": {
    "self": {
      "title": "Clusted",
      "href": "clusteds/5a90f132c78ef35c4730d0df"
    }
  },
  "summary": "1,RAV4,6,3,3,1",
  "_status": "OK"
}
```

### 4.2 GET : 读取已创建的人群分类结果

该API将读取已经创建的人群分类结果，结果在键值`summary`中。

该API支持通过`_id`和`hashed`码两种方式获取：

* `_id`和包括_id的`href`在创建时的返回结果中
* `hashed`码是输入`raw`的md5 hexdigest，因此可以直接在客户端用同样的方法计算得出

**HTTP Request:**

用`hashed`码获取结果

`GET https://dmicros.iamhd.top/clusteds/3e69a5afc5083b3553fa533d8c411bdf`

或

用`_id`获取结果

`GET https://dmicros.iamhd.top/clusteds/5a90f132c78ef35c4730d0df`

**示例**

```shell
curl https://dmicros.iamhd.top/clusteds/3e69a5afc5083b3553fa533d8c411bdf  -H "Authorization:<your access token>"
```

或
```shell
curl https://dmicros.iamhd.top/clusteds/5a90f132c78ef35c4730d0df  -H "Authorization:<your access token>"
```


**返回：**

计算结果如`"summary": "1,RAV4,6,3,3,1"`

```json
{
  "_id": "5a8353053f824d0e121193f5",
  "raw": "1,RAV4,5,,6,7,14,1,7,3,5,7,5,6,5,6,6,5,5,3,4,6,6,5,2,6,4,5,6,5,6,5,5,7,7,6,2,7,6,5,5,7,5,7,5,6,6,3,6",
  "summary": "1,RAV4,6,3,3,1",
  "hashed": "3e69a5afc5083b3553fa533d8c411bdf",
  "_updated": "Tue, 13 Feb 2018 21:05:09 GMT",
  "_created": "Tue, 13 Feb 2018 21:05:09 GMT",
  "_etag": "4778c0630108c0ae91a93b035ce7ab9c3e9bcc42",
  "_links": {
    "parent": {
      "title": "home",
      "href": "/"
    },
    "self": {
      "title": "Clusted",
      "href": "clusteds/5a8353053f824d0e121193f5"
    },
    "collection": {
      "title": "clusteds",
      "href": "clusteds"
    }
  }
}
```

## 5. clustedBatchs API : 批次人群分类结果

该API返回批量人群分类结果，一般来说您需要分两步来使用：

 1. 先创建批量人群分类结果，服务会批量存储人群分类结果和该批次的相关信息到服务器上
 2. 读取服务器上的结果

其中创建的过程需要**消费`clusteds`资源的配额**，如果您没有足够的配额，服务器将会返回402错误。

### 5.1 POST : 创建批次人群分类结果

该API将创建批次人群分类结果，输入的数据格式为json：

```json
{
  "name": "test",
  "description": "just for testing",
  "allowed_users": [
    "vw",
    "sgm"
  ],
  "samples": [
    {
      "raw": "1,RAV4,5,,6,7,14,1,7,3,5,7,5,6,5,6,6,5,5,3,4,6,6,5,2,6,4,5,6,5,6,5,5,7,7,6,2,7,6,5,5,7,5,7,5,6,6,3,6"
    },
    {
      "raw": "2,指南者,5,,4,7,16,1,2,7,6,6,5,6,5,3,5,4,2,4,4,5,5,4,4,4,5,5,6,7,3,5,7,2,5,6,4,5,5,7,3,5,4,5,7,6,6,1,6"
    }
  ],
  "extras": {
    "location": "beijing"
  }
}
```

其中：

* `"name": "test"` (可选) : 该批次名称
* `"description": "just for testing"` (可选) : 该批次描述
* `"allowed_users": ["vw", "sgm"]` (可选) : 该批次允许哪些用户查看，创建时服务会自动将当前用户列入
* `"extras": {"location": "beijing"}` (可选，自定义) : 该批次有哪些其他属性，内容键值可自定义
* `"samples"` **(必选)** : 该批次有哪些输入，格式和`clusteds`服务输入格式相同，一个批次至少要提供2个及以上的输入

**HTTP Request:**

`POST https://dmicros.iamhd.top/clustedBatchs`

**示例**

```shell
curl -XPOST https://dmicros.iamhd.top/clustedBatchs  -H "Authorization:<your access token>" -H "Content-Type:application/json" -d '{"name":"test","description":"just for testing", "extras":{"location":"beijing"}, "allowed_users":["vw","sgm"],"samples":[{"raw":"1,RAV4,5,,6,7,14,1,7,3,5,7,5,6,5,6,6,5,5,3,4,6,6,5,2,6,4,5,6,5,6,5,5,7,7,6,2,7,6,5,5,7,5,7,5,6,6,3,6"},{"raw":"2,\u6307\u5357\u8005,5,,4,7,16,1,2,7,6,6,5,6,5,3,5,4,2,4,4,5,5,4,4,4,5,5,6,7,3,5,7,2,5,6,4,5,5,7,3,5,4,5,7,6,6,1,6"}]}'
```

考虑到输入内容较多，如果将输入储存在`cbatch.json`文件中的话：

```shell
curl -XPOST https://dmicros.iamhd.top/clustedBatchs  -H "Authorization:<your access token>" -H "Content-Type:application/json" --data-binary "@cbatch.json"
```

**返回：**


```json
{
  "_updated": "Sat, 24 Feb 2018 06:25:05 GMT",
  "_created": "Sat, 24 Feb 2018 06:25:05 GMT",
  "_etag": "f6116bcc07b7c746ae8279ff39a1abd4271d4c4e",
  "_id": "5a910541c78ef35c4730d0e5",
  "_links": {
    "self": {
      "title": "Clustedbatch",
      "href": "clustedBatchs/5a910541c78ef35c4730d0e5"
    }
  },
  "_status": "OK"
}
```

### 5.2 GET 列表: 读取已创建批次人群分类结果列表

该API将返回有权查看的全部`clustedsBatchs`

**HTTP Request:**

`GET https://dmicros.iamhd.top/clustedBatchs`

**示例**

```shell
curl https://dmicros.iamhd.top/clustedBatchs  -H "Authorization:<your access token>"
```

**返回：**

```json
{
  "_items": [
    {
      "_id": "5a8f7a24c78ef349f9b84d93",
      "name": "test1",
      "allowed_users": [
        "vw",
        "sgm"
      ],
      "samples": [
        "5a8f7a24c78ef349f9b84d91",
        "5a8f7a24c78ef349f9b84d92"
      ],
      "_updated": "Fri, 23 Feb 2018 03:15:05 GMT",
      "_created": "Fri, 23 Feb 2018 02:19:16 GMT",
      "_etag": "5666b15caf569fe2ba52539f751c386f67bbf018",
      "_links": {
        "self": {
          "title": "Clustedbatch",
          "href": "clustedBatchs/5a8f7a24c78ef349f9b84d93"
        }
      }
    },
	...
  ],
  "_links": {
    "parent": {
      "title": "home",
      "href": "/"
    },
    "self": {
      "title": "clustedBatchs",
      "href": "clustedBatchs"
    }
  },
  "_meta": {
    "page": 1,
    "max_results": 25,
    "total": 6
  }
}
```

### 5.3 GET 单项详情: 读取已创建批次人群分类结果列表

该API将返回单项`clustedsBatchs`资源的详情，需提供`?embedded={"samples":1}`参数，这样返回的结果里`samples`字段中包括嵌入文档。

**HTTP Request:**

`GET https://dmicros.iamhd.top/clustedBatchs/5a910541c78ef35c4730d05e?embedded={"samples":1}`

**示例**

```shell
curl -g 'https://dmicros.iamhd.top/clustedBatchs/5a910541c78ef35c4730d0e5?embedded={"samples":1}'  -H "Authorization:<your access token>"
```

Tips:由于参数中包括`{}`，可能需要在请求发出时对地址进行一定处理，如`-g`选项。

**返回：**

```shell
{
  "_id": "5a910541c78ef35c4730d0e5",
  "name": "test",
  "description": "just for testing",
  "extras": {
    "location": "beijing"
  },
  "allowed_users": [
    "vw",
    "sgm"
  ],
  "samples": [
    {
      "_id": "5a910541c78ef35c4730d0e3",
      "raw": "1,RAV4,5,,6,7,14,1,7,3,5,7,5,6,5,6,6,5,5,3,4,6,6,5,2,6,4,5,6,5,6,5,5,7,7,6,2,7,6,5,5,7,5,7,5,6,6,3,6",
      "summary": "1,RAV4,6,3,3,1",
      "hashed": "3e69a5afc5083b3553fa533d8c411bdf",
      "_updated": "Sat, 24 Feb 2018 06:25:05 GMT",
      "_created": "Sat, 24 Feb 2018 06:25:05 GMT",
      "_etag": "f86acfae597124c5a6301be7e612387670f4c914"
    },
    {
      "_id": "5a910541c78ef35c4730d0e4",
      "raw": "2,指南者,5,,4,7,16,1,2,7,6,6,5,6,5,3,5,4,2,4,4,5,5,4,4,4,5,5,6,7,3,5,7,2,5,6,4,5,5,7,3,5,4,5,7,6,6,1,6",
      "summary": "2,指南者,3,2,4,1",
      "hashed": "4b62de12dd74929d8dc177741496ce2b",
      "_updated": "Sat, 24 Feb 2018 06:25:05 GMT",
      "_created": "Sat, 24 Feb 2018 06:25:05 GMT",
      "_etag": "fad8f8f70d6682e24d665cd5eb97dd3ae72a7caa"
    }
  ],
  "_updated": "Sat, 24 Feb 2018 06:25:05 GMT",
  "_created": "Sat, 24 Feb 2018 06:25:05 GMT",
  "_etag": "f6116bcc07b7c746ae8279ff39a1abd4271d4c4e",
  "_links": {
    "parent": {
      "title": "home",
      "href": "/"
    },
    "self": {
      "title": "Clustedbatch",
      "href": "clustedBatchs/5a910541c78ef35c4730d0e5"
    },
    "collection": {
      "title": "clustedBatchs",
      "href": "clustedBatchs"
    }
  }
}
```

### 5.4 GET 单项概要: 读取已创建批次人群分类结果列表

该API将返回单项`clustedsBatchs`资源概要，`samples`字段仅包括嵌入文档的`_id`，不包括详情。

**HTTP Request:**

`GET https://dmicros.iamhd.top/clustedBatchs`

**示例**

```shell
curl https://dmicros.iamhd.top/clustedBatchs/5a910541c78ef35c4730d0e5  -H "Authorization:<your access token>"
```

**返回：**

```shell
{
  "_id": "5a910541c78ef35c4730d0e5",
  "name": "test",
  "description": "just for testing",
  "extras": {
    "location": "beijing"
  },
  "allowed_users": [
    "vw",
    "sgm"
  ],
  "samples": [
    "5a910541c78ef35c4730d0e3",
    "5a910541c78ef35c4730d0e4"
  ],
  "_updated": "Sat, 24 Feb 2018 06:25:05 GMT",
  "_created": "Sat, 24 Feb 2018 06:25:05 GMT",
  "_etag": "f6116bcc07b7c746ae8279ff39a1abd4271d4c4e",
  "_links": {
    "parent": {
      "title": "home",
      "href": "/"
    },
    "self": {
      "title": "Clustedbatch",
      "href": "clustedBatchs/5a910541c78ef35c4730d0e5"
    },
    "collection": {
      "title": "clustedBatchs",
      "href": "clustedBatchs"
    }
  }
}
```

---
Hosted by **[Coding Pages](https://pages.coding.me)**
