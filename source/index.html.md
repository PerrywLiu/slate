---
title: API Reference

language_tabs: # must be one of https://github.com/rouge-ruby/rouge/wiki/List-of-supported-languages-and-lexers
  - shell
  - python

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://explorer.btc.com/zh-CN/btc'>Documentation Powered by Slate</a>

includes:
  - errors

search: true

code_clipboard: true

meta:
  - name: description
    content: Documentation for the btc API
---

# 介绍

使用 btc.com 浏览器提供的 API，实时获取btc链上区块、交易、地址信息。

## API结构

采用restful方式进行调用，路径如下：

 `https://$(Endpoint)/${method}/${versioin}/${model}/${path}`

 其中：

 1. Endpoint: gateway.toolstest.btc.com

 2. method: rest/api

 3. version: v1.0

 4. model: nodeapi
 
 5. path: 具体的API路径，参见下文定义


## 鉴权

调用API时，需要在Header中传递X-API-TOKEN进行鉴权。

·. X-API-TOKEN 是用户身份凭据，对应一个账户，请用户保管好自己的Token。

X-API-TOKEN可以到btc.com，登录后，在个人中心页面获取。

## 响应

所有相应类型均为 application/json，如下：


{

    "code": 0,

	  "data": {},

	  "msg": "success",

	  "timestamp": 1699966857,

}


响应中的code、data、msg、timestamp为固定字段，含义如下：

1. code: API响应状态码

2. msg: code对应的描述信息

3. data: 具体API响应的数据

4. timestamp: 请求对应的时间

# 地址相关
## 查询地址汇总信息

```shell
curl --request GET \
  --url https://gateway.toolstest.btc.com/rpc/api/v1.0/nodeapi/address/summary/3MTRYJy2ZQMymkSQb226BcHFmQpZsvgkmo \
  --header 'X-API-TOKEN: t648e371d1e70aeaad812ecaa9ff438b65043b1e745a0b50e1854eb8042dfa328' \
  --header 'content-type: application/json'
```


```python
import http.client

conn = http.client.HTTPSConnection("gateway.toolstest.btc.com")

payload = ""

headers = {
    'X-API-TOKEN': "t648e371d1e70aeaad812ecaa9ff438b65043b1e745a0b50e1854eb8042dfa328",
    'content-type': "application/json"
    }

conn.request("GET", "/rest/api/v1.0/nodeapi/address/summary/3MTRYJy2ZQMymkSQb226BcHFmQpZsvgkmo", payload, headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
```

> 响应示例

```
{
	"code": 0,
	"data": {
		"address": "3MTRYJy2ZQMymkSQb226BcHFmQpZsvgkmo",
		"createHeight": 676717,
		"createTime": 1616947766,
		"latestHeight": 816724,
		"lastTime": 1699961655,
		"totalReceive": 2007755545,
		"totalSend": 2007755545,
		"availableBalance": 0,
		"balance": 0,
		"txCount": 1358
	},
	"msg": "success",
	"timestamp": 1700006439,
	"trace_id": ""
}
```

HTTP请求

`GET https://gateway.toolstest.btc.com/rest/api/v1.0/nodeapi/address/summary/:addr`

### 请求参数

| 参数名 | 示例值 | 参数类型 | 是否必填 | 参数描述 |
| --- | --- | ---- | ---- | ---- |
| addr | 3MTRYJy2ZQMymkSQb226BcHFmQpZsvgkmo | String | 是 | 需要查询的地址 |

### 返回参数

| 参数名 | 参数类型 | 参数描述 |
| --- | --- | ---- | 
| address | String | 需要查询的地址 |
| createHeight | int | 地址创建高度 |
| createTime | int64 | 地址创建的时间戳 |
| latestHeight | int | 地址最近发生交易的高度 |
| latestTime | int64 | 地址最近发生交易的时间戳 |
| totalReceive | int64 | 地址总共收到的BTC数量，以聪为单位 |
| totalSend | int64 | 地址总共发出的BTC数量，以聪为单位 |
| availableBalance | int64 | 地址可用余额，以聪为单位 |
| balance | int64 | 地址总余额，以聪为单位 |
| txCount | int | 地址发生交易的总次数 |


## 查询地址交易流水


```shell
curl --request GET \
  --url https://gateway.toolstest.btc.com/rest/api/v1.0/nodeapi/address/txlist/1Kr6QSydW9bFQG1mXiPNNu6WpJGmUa9i1g/2/20 \
  --header 'X-API-TOKEN: t648e371d1e70aeaad812ecaa9ff438b65043b1e745a0b50e1854eb8042dfa328'
```


```python
import http.client

conn = http.client.HTTPSConnection("gateway.toolstest.btc.com")

headers = { 'X-API-TOKEN': "t648e371d1e70aeaad812ecaa9ff438b65043b1e745a0b50e1854eb8042dfa328" }

conn.request("GET", "/rest/api/v1.0/nodeapi/address/txlist/1Kr6QSydW9bFQG1mXiPNNu6WpJGmUa9i1g/2/20", headers=headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))

```

> 响应示例

```
{
	"code": 0,
	"data": {
		"currentPage": 2,
		"totalCount": 419367,
		"pageSize": 20,
		"list": [
			{
				"addr": "1Kr6QSydW9bFQG1mXiPNNu6WpJGmUa9i1g",
				"addrTxIndex": 419347,
				"txId": "14aa6a8744ee7ebfac4357b90bd465fd8b977e8ad4fe54a588a17a565ca5677e",
				"height": 816764,
				"txIndex": 22,
				"addrReceive": 2836409890,
				"addrSend": 0,
				"addrBalance": 200581162637,
				"timestamp": 1699987563
			},
			...
		]
	},
	"msg": "success",
	"timestamp": 1700001927,
	"trace_id": ""
}
```


HTTP请求

`GET https://gateway.toolstest.btc.com/rest/api/v1.0/nodeapi/address/txlist/:addr/:page/:pageSize`

### 请求参数

| 参数名 | 示例值 | 参数类型 | 是否必填 | 参数描述 | 默认值 |
| --- | --- | ---- | ---- | ---- | ---- |
| addr | 3MTRYJy2ZQMymkSQb226BcHFmQpZsvgkmo | String | 是 | 需要查询的地址 | 空 |
| page | 1 | int | 否 | 起始页码 | 1 |
| pageSize | 20 | int | 否 | 每页返回的条数 | 10 |

### 返回参数

| 参数名 | 参数类型 | 参数描述 |
| --- | --- | ---- | 
| currentPage | int | 当前页码 |
| pageSize | int | 每页返回的条数，如果返回的条数小于该值，说明已经返回全部数据 |
| totalCount | int | 该地址总的交易次数|
| list | array | 地址的交易流水明细|

#### 地址交易流水明细

| 参数名 | 参数类型 | 参数描述 |
| --- | --- | ---- | 
| addr | string | 请求的地址 |
| addrTxIndex | int | 地址第几次交易 |
| txId | string |交易hash |
| height | int | 交易所在的区块高度 |
| txIndex | int | 交易在区块中的索引 |
| addrReceive | int64 | 本次交易中，地址收到的BTC总额，以聪为单位 |
| addrSend | int64 | 本次交易中，地址发出的BTC总额，以聪为单位 |
| addrBalance | int64 | 本次交易结束后，地址的余额 |
| timestamp | int64 | 交易发生的时间戳 |



## 按起始高度查询地址交易流水


```shell
curl --request GET \
  --url https://gateway.toolstest.btc.com/rest/api/v1.0/nodeapi/address/txbyheight/3MTRYJy2ZQMymkSQb226BcHFmQpZsvgkmo/814100/814101/ \
  --header 'X-API-TOKEN: t648e371d1e70aeaad812ecaa9ff438b65043b1e745a0b50e1854eb8042dfa328'
```


```python
import http.client

conn = http.client.HTTPSConnection("gateway.toolstest.btc.com")

headers = { 'X-API-TOKEN': "t648e371d1e70aeaad812ecaa9ff438b65043b1e745a0b50e1854eb8042dfa328" }

conn.request("GET", "/rest/api/v1.0/nodeapi/address/txbyheight/3MTRYJy2ZQMymkSQb226BcHFmQpZsvgkmo/814100/814101/", headers=headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))

```

> 响应示例

```
{
	"code": 0,
	"data": {
		"currentPage": 2,
		"totalCount": 419367,
		"pageSize": 20,
		"list": [
			{
				"addr": "1Kr6QSydW9bFQG1mXiPNNu6WpJGmUa9i1g",
				"addrTxIndex": 419347,
				"txId": "14aa6a8744ee7ebfac4357b90bd465fd8b977e8ad4fe54a588a17a565ca5677e",
				"height": 816764,
				"txIndex": 22,
				"addrReceive": 2836409890,
				"addrSend": 0,
				"addrBalance": 200581162637,
				"timestamp": 1699987563
			},
			...
		]
	},
	"msg": "success",
	"timestamp": 1700001927,
	"trace_id": ""
}
```

HTTP请求

`GET https://gateway.toolstest.btc.com/rest/api/v1.0/nodeapi/address/txbyheight/:addr/:startHeight/:endHeight/:page/:pageSize`

### 请求参数

| 参数名 | 示例值 | 参数类型 | 是否必填 | 参数描述 | 默认值 |
| --- | --- | ---- | ---- | ---- | ---- |
| addr | 3MTRYJy2ZQMymkSQb226BcHFmQpZsvgkmo | String | 是 | 需要查询的地址 | 空 |
| startHeight | 1 | int | 否 | 交易流水的起始高度 | 0 |
| endHeight | 100 | int | 否 | 交易流水的结束高度  | 6929999 |
| page | 1 | int | 否 | 起始页码 | 1 |
| pageSize | 20 | int | 否 | 每页返回的条数 | 10 |

### 返回参数

| 参数名 | 参数类型 | 参数描述 |
| --- | --- | ---- | 
| currentPage | int | 当前页码 |
| pageSize | int | 每页返回的条数，如果返回的条数小于该值，说明在当前起始高度已经返回全部数据 |
| totalCount | int | 该地址总的交易次数|
| list | array | 地址的交易流水明细|

#### 地址交易流水明细

| 参数名 | 参数类型 | 参数描述 |
| --- | --- | ---- | 
| addr | string | 请求的地址 |
| addrTxIndex | int | 地址第几次交易 |
| txId | string |交易hash |
| height | int | 交易所在的区块高度 |
| txIndex | int | 交易在区块中的索引 |
| addrReceive | int64 | 本次交易中，地址收到的BTC总额，以聪为单位 |
| addrSend | int64 | 本次交易中，地址发出的BTC总额，以聪为单位 |
| addrBalance | int64 | 本次交易结束后，地址的余额 |
| timestamp | int64 | 交易发生的时间戳 |



## 按起始时间查询地址交易流水

```shell
curl --request GET \
  --url https://gateway.toolstest.btc.com/rest/api/v1.0/nodeapi/address/txbytime/3MTRYJy2ZQMymkSQb226BcHFmQpZsvgkmo/1618200091/0/2/20 \
  --header 'X-API-TOKEN: t648e371d1e70aeaad812ecaa9ff438b65043b1e745a0b50e1854eb8042dfa328'
```


```python
import http.client

conn = http.client.HTTPSConnection("gateway.toolstest.btc.com")

headers = { 'X-API-TOKEN': "t648e371d1e70aeaad812ecaa9ff438b65043b1e745a0b50e1854eb8042dfa328" }

conn.request("GET", "/rest/api/v1.0/nodeapi/address/txbytime/3MTRYJy2ZQMymkSQb226BcHFmQpZsvgkmo/1618200091/0/2/20", headers=headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))

```

> 响应示例

```
{
	"code": 0,
	"data": {
		"currentPage": 2,
		"totalCount": 1358,
		"pageSize": 20,
		"list": [
			{
				"addr": "3MTRYJy2ZQMymkSQb226BcHFmQpZsvgkmo",
				"addrTxIndex": 1338,
				"txId": "8b28b6699e5a2b32caf026ebed7be369372b17ee9dbd30ab6fac08496e202974",
				"height": 814101,
				"txIndex": 2,
				"addrReceive": 0,
				"addrSend": 424071,
				"addrBalance": 0,
				"timestamp": 1698434974
			},
			
		]
	},
	"msg": "success",
	"timestamp": 1700006221,
	"trace_id": ""
}
```

HTTP请求

`GET https://gateway.toolstest.btc.com/rest/api/v1.0/nodeapi/address/txbytime/:addr/:startTime/:endTime/:page/:pageSize`

### 请求参数

| 参数名 | 示例值 | 参数类型 | 是否必填 | 参数描述 | 默认值 |
| --- | --- | ---- | ---- | ---- | ---- |
| addr | 3MTRYJy2ZQMymkSQb226BcHFmQpZsvgkmo | String | 是 | 需要查询的地址 | 空 |
| startTime | 1700003246 | int64 | 否 | 交易流水的起始时间 | 0 |
| endTime | 1700003246 | int64 | 否 | 交易流水的截止时间  | 当前时间 |
| page | 1 | int | 否 | 起始页码 | 1 |
| pageSize | 20 | int | 否 | 每页返回的条数 | 10 |

### 返回参数

| 参数名 | 参数类型 | 参数描述 |
| --- | --- | ---- | 
| currentPage | int | 当前页码 |
| pageSize | int | 每页返回的条数，如果返回的条数小于该值，说明在当前起始高度已经返回全部数据 |
| totalCount | int | 该地址总的交易次数|
| list | array | 地址的交易流水明细|

#### 地址交易流水明细

| 参数名 | 参数类型 | 参数描述 |
| --- | --- | ---- | 
| addr | string | 请求的地址 |
| addrTxIndex | int | 地址第几次交易 |
| txId | string |交易hash |
| height | int | 交易所在的区块高度 |
| txIndex | int | 交易在区块中的索引 |
| addrReceive | int64 | 本次交易中，地址收到的BTC总额，以聪为单位 |
| addrSend | int64 | 本次交易中，地址发出的BTC总额，以聪为单位 |
| addrBalance | int64 | 本次交易结束后，地址的余额 |
| timestamp | int64 | 交易发生的时间戳 |



## 查询地址的UTXO列表

```shell
curl --request GET \
  --url https://gateway.toolstest.btc.com/rest/api/v1.0/nodeapi/address/utxolist/1Kr6QSydW9bFQG1mXiPNNu6WpJGmUa9i1g/0/0 \
  --header 'X-API-TOKEN: t648e371d1e70aeaad812ecaa9ff438b65043b1e745a0b50e1854eb8042dfa328'
```


```python
import http.client

conn = http.client.HTTPSConnection("gateway.toolstest.btc.com")

headers = { 'X-API-TOKEN': "t648e371d1e70aeaad812ecaa9ff438b65043b1e745a0b50e1854eb8042dfa328" }

conn.request("GET", "/rest/api/v1.0/nodeapi/address/utxolist/1Kr6QSydW9bFQG1mXiPNNu6WpJGmUa9i1g/0/0", headers=headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))

```

> 响应示例

```
	{
	"code": 0,
	"data": {
		"currentPage": 1,
		"totalCount": 5,
		"pageSize": 10,
		"list": [
			{
				"addr": "1Kr6QSydW9bFQG1mXiPNNu6WpJGmUa9i1g",
				"height": 816787,
				"txId": "1661eac83926e48bf61f5adae79264ec16e958d16323d02609afa908e8b9a4bf",
				"index": 1,
				"amount": 94391404039,
				"timestamp": 1700002323,
				"isCoinbase": false
			},
      ...
		]
	},
	"msg": "success",
	"timestamp": 1700003856,
	"trace_id": ""
}
```

HTTP请求

`GET https://gateway.toolstest.btc.com/rest/api/v1.0/nodeapi/address/utxolist/:addr/:page/:pageSize`

### 请求参数

| 参数名 | 示例值 | 参数类型 | 是否必填 | 参数描述 | 默认值 |
| --- | --- | ---- | ---- | ---- | ---- |
| addr | 3MTRYJy2ZQMymkSQb226BcHFmQpZsvgkmo | String | 是 | 需要查询的地址 | 空 |
| page | 1 | int | 否 | 起始页码 | 1 |
| pageSize | 20 | int | 否 | 每页返回的条数 | 10 |

### 返回参数

| 参数名 | 参数类型 | 参数描述 |
| --- | --- | ---- | 
| currentPage | int | 当前页码 |
| pageSize | int | 每页返回的条数，如果返回的条数小于该值，说明在当前起始高度已经返回全部数据 |
| totalCount | int | 该地址总的交易次数|
| list | array | 地址的UTXO列表 |

#### 地址的UTXO列表

| 参数名 | 参数类型 | 参数描述 |
| --- | --- | ---- | 
| addr | string | 请求的地址 |
| txId | string |交易hash |
| height | int | 交易所在的区块高度 |
| index | int | 交易在区块中的索引 |
| amount | int64 | UTXO的金额，以聪为单位 |
| timestamp | int64 | 交易发生的时间戳 |
| isCoinbase | bool | 是否是coinBase交易产生的UTXO |


## 查询地址未确认的交易流水


```shell
curl --request GET \
  --url https://gateway.toolstest.btc.com/rest/api/v1.0/nodeapi/address/unconfirm/3JWhLZb3kkYUQuuxFNX2FcerHV62dnXN9T/3/3 \
  --header 'X-API-TOKEN: t648e371d1e70aeaad812ecaa9ff438b65043b1e745a0b50e1854eb8042dfa328'
```


```python
import http.client

conn = http.client.HTTPSConnection("gateway.toolstest.btc.com")

headers = { 'X-API-TOKEN': "t648e371d1e70aeaad812ecaa9ff438b65043b1e745a0b50e1854eb8042dfa328" }

conn.request("GET", "/rest/api/v1.0/nodeapi/address/unconfirm/3JWhLZb3kkYUQuuxFNX2FcerHV62dnXN9T/3/3", headers=headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))

```

> 响应示例

```
	{
	"code": 0,
	"data": {
		"currentPage": 3,
		"totalCount": 8,
		"pageSize": 3,
		"list": [
			{
				"addr": "3JWhLZb3kkYUQuuxFNX2FcerHV62dnXN9T",
				"addrTxIndex": 0,
				"txId": "dfa605710ae77862016202e2db0395989f5dc72899c532fa3018a2e41b8e1b3c",
				"height": 0,
				"txIndex": 0,
				"addrReceive": 0,
				"addrSend": 45008,
				"addrBalance": 0,
				"timestamp": 1699101159
			},
			...
		]
	},
	"msg": "success",
	"timestamp": 1700006977,
	"trace_id": ""
}
```

HTTP请求

`GET https://gateway.toolstest.btc.com/rest/api/v1.0/nodeapi/address/unconfirm/:addr/:page/:pageSize`

### 请求参数

| 参数名 | 示例值 | 参数类型 | 是否必填 | 参数描述 | 默认值 |
| --- | --- | ---- | ---- | ---- | ---- |
| addr | 3MTRYJy2ZQMymkSQb226BcHFmQpZsvgkmo | String | 是 | 需要查询的地址 | 空 |
| page | 1 | int | 否 | 起始页码 | 1 |
| pageSize | 20 | int | 否 | 每页返回的条数 | 10 |

### 返回参数

| 参数名 | 参数类型 | 参数描述 |
| --- | --- | ---- | 
| currentPage | int | 当前页码 |
| pageSize | int | 每页返回的条数，如果返回的条数小于该值，说明在当前起始高度已经返回全部数据 |
| totalCount | int | 该地址总的交易次数|
| list | array | 地址的未确认交易明细 |

#### 地址的未确认交易明细

| 参数名 | 参数类型 | 参数描述 |
| --- | --- | ---- | 
| addr | string | 请求的地址 |
| addrTxIndex | int | 地址第几次交易 |
| txId | string |交易hash |
| height | int | 交易所在的区块高度，未确认交易为0 |
| txIndex | int | 交易在区块中的索引，未确认交易为0 |
| addrReceive | int64 | 本次交易中，地址收到的BTC总额，以聪为单位 |
| addrSend | int64 | 本次交易中，地址发出的BTC总额，以聪为单位 |
| addrBalance | int64 | 未确认交易为0 |
| timestamp | int64 | 交易发生的时间戳 |


# 交易相关
## 根据区块高度获取标准交易数据列表

```shell
curl --request GET \
  --url https://gateway.toolstest.btc.com/rest/api/v1.0/nodeapi/tx/standardlist/800000 \
  --header 'X-API-TOKEN: t648e371d1e70aeaad812ecaa9ff438b65043b1e745a0b50e1854eb8042dfa328' \
  --header 'content-type: application/json'
```


```python
import http.client

conn = http.client.HTTPSConnection("gateway.toolstest.btc.com")

payload = ""

headers = {
    'X-API-TOKEN': "t648e371d1e70aeaad812ecaa9ff438b65043b1e745a0b50e1854eb8042dfa328",
    'content-type': "application/json"
    }

conn.request("GET", "/rest/api/v1.0/nodeapi/tx/standardlist/800000", payload, headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))

```

> 响应示例

```
	{
	"code": 0,
	"data": {
		"hash": "00000000000000000002a7c4c1e48d76c5a37902165a270156b7a8d72728a054",
		"confirmations": 5,
		"height": 800000,
		"version": 874340352,
		"versionHex": "341d6000",
		"merkleroot": "91f01a00530c8c83617190048ea8b0814d506cf24dfdbcf8893f8f0cab7f0855",
		"time": 1690168629,
		"mediantime": 1690165851,
		"nonce": 106861918,
		"bits": "17053894",
		"difficulty": 53911173001054.59,
		"chainwork": "00000000000000000000000000000000000000004fc85ab3390629e495bf13d5",
		"nTx": 3721,
		"previousblockhash": "000000000000000000012117ad9f72c1c0e42227c2d042dca23e6b96bd9fbb55",
		"nextblockhash": "00000000000000000000e26b239cf19ec7ace5edd9694d51a3f6933247720947",
		"transactions": [
			{
				"txid": "b75ca3106ed100521aa50e3ec267a06431c6319538898b25e1b757a5736f5fb4",
				"hash": "4f684e6a3456df6e321ead86e56d37697340d81174e3da641846b3e23ff962a3",
				"version": 1,
				"size": 192,
				"vsize": 165,
				"weight": 660,
				"locktime": 0,
				"vin": [
					{
						"txid": "",
						"vout": 0,
						"scriptSig": {
							"asm": "",
							"hex": ""
						},
						"txinwitness": [
							"0000000000000000000000000000000000000000000000000000000000000000"
						],
						"sequence": 4294967295,
						"coinbase": "0300350c0120130909092009092009102cda1492140000000000",
						"prevout": {
							"value": 0,
							"n": 0,
							"address": "",
							"height": 0,
							"txid": "",
							"timestamp": 0,
							"is_coinbase": false
						}
					}
				],
				"vout": [
					{
						"value": 6.3868768,
						"n": 0,
						"scriptPubKey": {
							"asm": "OP_HASH160 c3f8f898ae5cab4f4c1d597ecb0f3a81a9b146c3 OP_EQUAL",
							"desc": "",
							"hex": "a914c3f8f898ae5cab4f4c1d597ecb0f3a81a9b146c387",
							"address": "3KZDwmJHB6QJ13QPXHaW7SS3yTESFPZoxb",
							"type": "scripthash"
						},
						"offset": 0,
						"is_miner": false
					},
					{
						"value": 0,
						"n": 1,
						"scriptPubKey": {
							"asm": "OP_RETURN aa21a9ed9fbe517a588ccaca585a868f3cf19cb6897e3c26f3351361fb28ac8509e69a7e",
							"desc": "",
							"hex": "6a24aa21a9ed9fbe517a588ccaca585a868f3cf19cb6897e3c26f3351361fb28ac8509e69a7e",
							"address": "nulldata-76163891097a51fa674e675e987e306b",
							"type": "nulldata"
						},
						"offset": 0,
						"is_miner": false
					}
				],
				"hex": "010000000001010000000000000000000000000000000000000000000000000000000000000000ffffffff1a0300350c0120130909092009092009102cda1492140000000000ffffffff02c09911260000000017a914c3f8f898ae5cab4f4c1d597ecb0f3a81a9b146c3870000000000000000266a24aa21a9ed9fbe517a588ccaca585a868f3cf19cb6897e3c26f3351361fb28ac8509e69a7e0120000000000000000000000000000000000000000000000000000000000000000000000000",
				"blockhash": "",
				"blocktime": 0,
				"rectime": 0,
				"height": 0,
				"is_confirm": false
			},
			......
		]
	},
	"msg": "success",
	"timestamp": 1700007450,
	"trace_id": ""
}
```

HTTP请求

`GET https://gateway.toolstest.btc.com/rest/api/v1.0/nodeapi/tx/standardlist/:height/:page/:pagesize`

### 请求参数

| 参数名 | 示例值 | 参数类型 | 是否必填 | 参数描述 | 默认值 |
| --- | --- | ---- | ---- | ---- | ---- |
| height | 800000 | int | 是 | 需要查询的高度 | 空 |
| page | 1 | int | 否 | 起始页码 | 1 |
| pageSize | 10 | int | 否 | 每页返回的条数 | 10 |

### 返回参数

| 参数名 | 参数类型 | 参数描述 |
| --- | --- | ---- | 
| hash | string | 区块hash |
| height | int | 区块高度 | 
| version | int | 区块版本|
| versionHex | string | 区块版本16进制表示 |
| merkleroot | string | merkle根 |
| time | int64 | 产生区块的时间戳 |
| nonce | uint32 | 产生区块的随机数 |
| Bits | string |  |
| difficulty | float64 | |
| nTx | int | 区块包含的交易总数 | 
| previousblockhash | string | 上一个区块hash | 
| nextblockhash | string | 下一个区块hash | 
| transactions | array | 交易列表 | 

#### 交易列表
| 参数名 | 参数类型 | 参数描述 |
| --- | --- | ---- | 
| hash | string | 交易hash |
| txid | string | 交易id |
| version | int | |
| size | int | |
| viszie | int | |
| weight | int | |
| locktime | int | |
| vin | array | 交易的输入信息|
| vout | array | 交易的输出信息|
| Hex | string |  交易的16进制信息 | 



## 根据区块高度获取填充交易输入地址的数据列表

标准交易中，输入部分只包含引用UTXO的txid 和 index，如果需要获取具体的地址信息，需要重新查询，本接口包含所有交易的输入地址

```shell
curl --request GET \
  --url https://gateway.toolstest.btc.com/rest/api/v1.0/nodeapi/tx/fillvin/c2e8fd13d6bea9d54aeceb83dbbeff631adf4003dce37175fce524e7e28795c2 \
  --header 'X-API-TOKEN: t648e371d1e70aeaad812ecaa9ff438b65043b1e745a0b50e1854eb8042dfa328'
```


```python
import http.client

conn = http.client.HTTPSConnection("gateway.toolstest.btc.com")

headers = { 'X-API-TOKEN': "t648e371d1e70aeaad812ecaa9ff438b65043b1e745a0b50e1854eb8042dfa328" }

conn.request("GET", "/rest/api/v1.0/nodeapi/tx/fillvin/c2e8fd13d6bea9d54aeceb83dbbeff631adf4003dce37175fce524e7e28795c2", headers=headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
```

> 响应示例

```
	{
	"code": 0,
	"data": {
		"hash": "00000000000000000002a7c4c1e48d76c5a37902165a270156b7a8d72728a054",
		"confirmations": 5,
		"height": 800000,
		"version": 874340352,
		"versionHex": "341d6000",
		"merkleroot": "91f01a00530c8c83617190048ea8b0814d506cf24dfdbcf8893f8f0cab7f0855",
		"time": 1690168629,
		"mediantime": 1690165851,
		"nonce": 106861918,
		"bits": "17053894",
		"difficulty": 53911173001054.59,
		"chainwork": "00000000000000000000000000000000000000004fc85ab3390629e495bf13d5",
		"nTx": 3721,
		"previousblockhash": "000000000000000000012117ad9f72c1c0e42227c2d042dca23e6b96bd9fbb55",
		"nextblockhash": "00000000000000000000e26b239cf19ec7ace5edd9694d51a3f6933247720947",
		"transactions": [
			{
					"txid": "d41f5de48325e79070ccd3a23005f7a3b405f3ce1faa4df09f6d71770497e9d5",
					"hash": "94574a056707bda53ab7e08ddef0ca29100cb42647f5f76b3877cc8f4b694b56",
					"version": 2,
					"size": 235,
					"vsize": 153,
					"weight": 610,
					"locktime": 0,
					"vin": [
						{
							"txid": "a992dbddbeb7382e3defc6914f970ea769ef813e69a923afa336976f2cbf0465",
							"vout": 1,
							"scriptSig": {
								"asm": "",
								"hex": ""
							},
							"txinwitness": [
								"3045022100f404e977e0a3dee1e9da7708db6ce6f3cbe80e6ffbbb6364bd2c725af200520a02201faca96001ac7f82fcea71e03b29deeaac6525c3bb8abe3b3c64544af16b698501",
								"025b1b8e6cd2ebc837fc57928c688b9b4d192f9001d03d1831510a6e511ca3fa5e"
							],
							"sequence": 4294967295,
							"coinbase": "",
							"prevout": {
								"value": 604308,
								"n": 1,
								"address": "bc1qvndmep839uexn899qy865cvddgj4txm0nkjua9",
								"height": 799999,
								"txid": "a992dbddbeb7382e3defc6914f970ea769ef813e69a923afa336976f2cbf0465",
								"timestamp": 1690168304,
								"is_coinbase": false
							}
						}
					],
					"vout": [
						{
							"value": 0.00143332,
							"n": 0,
							"scriptPubKey": {
								"asm": "1 2d618c1f73d5133fdc97d545bfbf55b4cba2ab2a9d41e4596b1df6b8ea9d9348",
								"desc": "",
								"hex": "51202d618c1f73d5133fdc97d545bfbf55b4cba2ab2a9d41e4596b1df6b8ea9d9348",
								"address": "bc1p94scc8mn65fnlhyh64zml064kn9692e2n4q7gkttrhmt365ajdyq0m2mzh",
								"type": "witness_v1_taproot"
							},
							"offset": 0,
							"is_miner": false
						},
						{
							"value": 0.00291851,
							"n": 1,
							"scriptPubKey": {
								"asm": "0 64dbbc84f12f32699ca5010faa618d6a25559b6f",
								"desc": "",
								"hex": "001464dbbc84f12f32699ca5010faa618d6a25559b6f",
								"address": "bc1qvndmep839uexn899qy865cvddgj4txm0nkjua9",
								"type": "witness_v0_keyhash"
							},
							"offset": 0,
							"is_miner": false
						}
					],
					"hex": "020000000001016504bf2c6f9736a3af23a9693e81ef69a70e974f91c6ef3d2e38b7bedddb92a90100000000ffffffff02e42f0200000000002251202d618c1f73d5133fdc97d545bfbf55b4cba2ab2a9d41e4596b1df6b8ea9d93480b7404000000000016001464dbbc84f12f32699ca5010faa618d6a25559b6f02483045022100f404e977e0a3dee1e9da7708db6ce6f3cbe80e6ffbbb6364bd2c725af200520a02201faca96001ac7f82fcea71e03b29deeaac6525c3bb8abe3b3c64544af16b69850121025b1b8e6cd2ebc837fc57928c688b9b4d192f9001d03d1831510a6e511ca3fa5e00000000",
					"blockhash": "",
					"blocktime": 0,
					"rectime": 0,
					"height": 0,
					"is_confirm": false
				},
			......
		]
	},
	"msg": "success",
	"timestamp": 1700007450,
	"trace_id": ""
}
```

HTTP请求

`GET https://gateway.toolstest.btc.com/rest/api/v1.0/nodeapi/tx/fillvinlist/:height/:page/:pagesize`

### 请求参数

| 参数名 | 示例值 | 参数类型 | 是否必填 | 参数描述 | 默认值 |
| --- | --- | ---- | ---- | ---- | ---- |
| height | 800000 | int | 是 | 需要查询的高度 | 空 |
| page | 1 | int | 否 | 起始页码 | 1 |
| pageSize | 10 | int | 否 | 每页返回的条数 | 10 |

### 返回参数

| 参数名 | 参数类型 | 参数描述 |
| --- | --- | ---- | 
| hash | string | 区块hash |
| height | int | 区块高度 | 
| version | int | 区块版本|
| versionHex | string | 区块版本16进制表示 |
| merkleroot | string | merkle根 |
| time | int64 | 产生区块的时间戳 |
| nonce | uint32 | 产生区块的随机数 |
| Bits | string |  |
| difficulty | float64 | |
| nTx | int | 区块包含的交易总数 | 
| previousblockhash | string | 上一个区块hash | 
| nextblockhash | string | 下一个区块hash | 
| transactions | array | 交易列表 | 

#### 交易列表
| 参数名 | 参数类型 | 参数描述 |
| --- | --- | ---- | 
| hash | string | 交易hash |
| txid | string | 交易id |
| version | int | |
| size | int | |
| viszie | int | |
| weight | int | |
| locktime | int | |
| vin | array | 交易的输入信息|
| vout | array | 交易的输出信息|
| Hex | string |  交易的16进制信息 | 


## 根据交易ID获取标准交易数据

```shell
curl --request GET \
  --url https://gateway.toolstest.btc.com/rest/api/v1.0/nodeapi/tx/standard/674fea60d8992b3e630aa7a5c75277c6ed80d55ec8abb80716e8892c45517561 \
  --header 'X-API-TOKEN: t648e371d1e70aeaad812ecaa9ff438b65043b1e745a0b50e1854eb8042dfa328'
```


```python
import http.client

conn = http.client.HTTPSConnection("gateway.toolstest.btc.com")

headers = { 'X-API-TOKEN': "t648e371d1e70aeaad812ecaa9ff438b65043b1e745a0b50e1854eb8042dfa328" }

conn.request("GET", "/rest/api/v1.0/nodeapi/tx/standard/674fea60d8992b3e630aa7a5c75277c6ed80d55ec8abb80716e8892c45517561", headers=headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
```

> 响应示例

```
	{
	"code": 0,
	"data": {
		"txInfo": {
			"txid": "674fea60d8992b3e630aa7a5c75277c6ed80d55ec8abb80716e8892c45517561",
			"hash": "776e989be029ac705fa12d35d1ab7025a54728c27702570fd6164f61aab05eb4",
			"version": 2,
			"size": 223,
			"vsize": 142,
			"weight": 565,
			"locktime": 0,
			"vin": [
				{
					"txid": "d8228ff9af23aa81997c81faaa7eecdb64ce98cf7f13948afbf5624284e3ecd4",
					"vout": 14,
					"scriptSig": {
						"asm": "",
						"hex": ""
					},
					"txinwitness": [
						"304402204ae30cceaefd01a0df2d39a7be3686909820fc912a11aef4839bba290851e9fc0220077e15f59e6a773c6cfb80ac3b781c0b6b505374ebdc2eb6b544beaa2fe48f3401",
						"02f6caaab827b73590d0b8927c8fac0f205127dc2df9bbf3d5fa754ad8038b8a56"
					],
					"sequence": 2147483648,
					"coinbase": "",
					"prevout": {
						"value": 0,
						"n": 0,
						"address": "",
						"height": 0,
						"txid": "",
						"timestamp": 0,
						"is_coinbase": false
					}
				}
			],
			"vout": [
				{
					"value": 0.00003,
					"n": 0,
					"scriptPubKey": {
						"asm": "OP_HASH160 23e522dfc6656a8fda3d47b4fa53f7585ac758cd OP_EQUAL",
						"desc": "addr(34xp4vRoCGJym3xR7yCVPFHoCNxv4Twseo)#98w9hwdk",
						"hex": "a91423e522dfc6656a8fda3d47b4fa53f7585ac758cd87",
						"address": "34xp4vRoCGJym3xR7yCVPFHoCNxv4Twseo",
						"type": "scripthash"
					},
					"offset": 0,
					"is_miner": false
				},
				{
					"value": 0.00089978,
					"n": 1,
					"scriptPubKey": {
						"asm": "0 5f24b63f2d8c25a0ad96ffe4d6153302847a0da6",
						"desc": "addr(bc1qtujtv0ed3sj6ptvklljdv9fnq2z85rdx35lcex)#jws389gw",
						"hex": "00145f24b63f2d8c25a0ad96ffe4d6153302847a0da6",
						"address": "bc1qtujtv0ed3sj6ptvklljdv9fnq2z85rdx35lcex",
						"type": "witness_v0_keyhash"
					},
					"offset": 0,
					"is_miner": false
				}
			],
			"hex": "02000000000101d4ece3844262f5fb8a94137fcf98ce64dbec7eaafa817c9981aa23aff98f22d80e000000000000008002b80b00000000000017a91423e522dfc6656a8fda3d47b4fa53f7585ac758cd877a5f0100000000001600145f24b63f2d8c25a0ad96ffe4d6153302847a0da60247304402204ae30cceaefd01a0df2d39a7be3686909820fc912a11aef4839bba290851e9fc0220077e15f59e6a773c6cfb80ac3b781c0b6b505374ebdc2eb6b544beaa2fe48f34012102f6caaab827b73590d0b8927c8fac0f205127dc2df9bbf3d5fa754ad8038b8a5600000000",
			"blockhash": "00000000000000000002961e45b5e789877ad5e53dc941b37f53aab48ed599f2",
			"blocktime": 1698613380,
			"rectime": 0,
			"height": 0,
			"is_confirm": false
		}
	},
	"msg": "success",
	"timestamp": 1700009121,
	"trace_id": ""
}
```

HTTP请求

`GET https://gateway.toolstest.btc.com/rest/api/v1.0/nodeapi/tx/standard/:txId`

### 请求参数

| 参数名 | 示例值 | 参数类型 | 是否必填 | 参数描述 | 默认值 |
| --- | --- | ---- | ---- | ---- | ---- |
| txId |  | string | 是 | 需要查询的交易ID | 空 |

### 返回参数

| 参数名 | 参数类型 | 参数描述 |
| --- | --- | ---- | 
| hash | string | 交易hash |
| txid | string | 交易id |
| version | int | |
| size | int | |
| viszie | int | |
| weight | int | |
| locktime | int | |
| vin | array | 交易的输入信息|
| vout | array | 交易的输出信息|
| Hex | string |  交易的16进制信息 | 



## 根据交易ID获取填充后的交易数据

此接口会将交易中的输入地址和金额进行填充，方便查询

```shell
curl --request GET \
  --url https://gateway.toolstest.btc.com/rest/api/v1.0/nodeapi/tx/fillvin/c2e8fd13d6bea9d54aeceb83dbbeff631adf4003dce37175fce524e7e28795c2 \
  --header 'X-API-TOKEN: t648e371d1e70aeaad812ecaa9ff438b65043b1e745a0b50e1854eb8042dfa328'
```


```python
import http.client

conn = http.client.HTTPSConnection("gateway.toolstest.btc.com")

headers = { 'X-API-TOKEN': "t648e371d1e70aeaad812ecaa9ff438b65043b1e745a0b50e1854eb8042dfa328" }

conn.request("GET", "/rest/api/v1.0/nodeapi/tx/fillvin/c2e8fd13d6bea9d54aeceb83dbbeff631adf4003dce37175fce524e7e28795c2", headers=headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))

```

> 响应示例

```
{
	"code": 0,
	"data": {
		"txInfo": {
			"txid": "c2e8fd13d6bea9d54aeceb83dbbeff631adf4003dce37175fce524e7e28795c2",
			"hash": "6685065bbce58799a0a9681fe0a16d9477341f51773f5d8423682d936adb6f0f",
			"version": 2,
			"size": 192,
			"vsize": 110,
			"weight": 438,
			"locktime": 0,
			"vin": [
				{
					"txid": "1eca064e992d7c27a163ff5f7797da368f81f19752656e34a0ad5438e067245e",
					"vout": 158,
					"scriptSig": {
						"asm": "",
						"hex": ""
					},
					"txinwitness": [
						"3045022100baea2e0ac4615d49c1b1ec3476c9ee14f7bd239229a424dd882ffe12f820ed4e022060e81963a39aaf14e59ba0b11fa3d12650cf3da4f7f12dda53dab45c33124b2c01",
						"02f00a8a664bab6f0c0240b3e4682d16b0c44a1d5aec8bdc31c3e136b2ba4e7920"
					],
					"sequence": 4294967293,
					"coinbase": "",
					"prevout": {
						"value": 188729,
						"n": 158,
						"address": "bc1q2v78vfdldam5r4hft2w63u0ngxt222f4znl3zd",
						"height": 815072,
						"txid": "1eca064e992d7c27a163ff5f7797da368f81f19752656e34a0ad5438e067245e",
						"timestamp": 1698987608,
						"is_coinbase": false
					}
				}
			],
			"vout": [
				{
					"value": 0.00133699,
					"n": 0,
					"scriptPubKey": {
						"asm": "0 3fc890e2cb4f4609aad7cb93412c1ec074c3c63e",
						"desc": "addr(bc1q8lyfpcktfarqn2khewf5ztq7cp6v83379gfy2l)#awh9zx84",
						"hex": "00143fc890e2cb4f4609aad7cb93412c1ec074c3c63e",
						"address": "bc1q8lyfpcktfarqn2khewf5ztq7cp6v83379gfy2l",
						"type": "witness_v0_keyhash"
					},
					"offset": 0,
					"is_miner": false
				}
			],
			"hex": "020000000001015e2467e03854ada0346e655297f1818f36da97775fff63a1277c2d994e06ca1e9e00000000fdffffff01430a0200000000001600143fc890e2cb4f4609aad7cb93412c1ec074c3c63e02483045022100baea2e0ac4615d49c1b1ec3476c9ee14f7bd239229a424dd882ffe12f820ed4e022060e81963a39aaf14e59ba0b11fa3d12650cf3da4f7f12dda53dab45c33124b2c012102f00a8a664bab6f0c0240b3e4682d16b0c44a1d5aec8bdc31c3e136b2ba4e792000000000",
			"blockhash": "",
			"blocktime": 0,
			"rectime": 0,
			"height": 815073,
			"is_confirm": true
		}
	},
	"msg": "success",
	"timestamp": 1700008863,
	"trace_id": ""
}
```

HTTP请求

`GET https://gateway.toolstest.btc.com/rest/api/v1.0/nodeapi/tx/fillvin/:txId`

### 请求参数

| 参数名 | 示例值 | 参数类型 | 是否必填 | 参数描述 | 默认值 |
| --- | --- | ---- | ---- | ---- | ---- |
| txId |  | string | 是 | 需要查询的交易ID | 空 |

### 返回参数

| 参数名 | 参数类型 | 参数描述 |
| --- | --- | ---- | 
| hash | string | 交易hash |
| txid | string | 交易id |
| version | int | |
| size | int | |
| viszie | int | |
| weight | int | |
| locktime | int | |
| vin | array | 交易的输入信息|
| vout | array | 交易的输出信息|
| Hex | string |  交易的16进制信息 | 
 

## 查询交易原始数据

```shell
curl --request GET \
  --url https://gateway.toolstest.btc.com/rest/api/v1.0/nodeapi/tx/rawtx/674fea60d8992b3e630aa7a5c75277c6ed80d55ec8abb80716e8892c45517561 \
  --header 'X-API-TOKEN: t648e371d1e70aeaad812ecaa9ff438b65043b1e745a0b50e1854eb8042dfa328'
```


```python
import http.client

conn = http.client.HTTPSConnection("gateway.toolstest.btc.com")

headers = { 'X-API-TOKEN': "t648e371d1e70aeaad812ecaa9ff438b65043b1e745a0b50e1854eb8042dfa328" }

conn.request("GET", "/rest/api/v1.0/nodeapi/tx/rawtx/674fea60d8992b3e630aa7a5c75277c6ed80d55ec8abb80716e8892c45517561", headers=headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))

```

> 响应示例

```
{
	"code": 0,
	"data": {
		"rawtx": "02000000000101d4ece3844262f5fb8a94137fcf98ce64dbec7eaafa817c9981aa23aff98f22d80e000000000000008002b80b00000000000017a91423e522dfc6656a8fda3d47b4fa53f7585ac758cd877a5f0100000000001600145f24b63f2d8c25a0ad96ffe4d6153302847a0da60247304402204ae30cceaefd01a0df2d39a7be3686909820fc912a11aef4839bba290851e9fc0220077e15f59e6a773c6cfb80ac3b781c0b6b505374ebdc2eb6b544beaa2fe48f34012102f6caaab827b73590d0b8927c8fac0f205127dc2df9bbf3d5fa754ad8038b8a5600000000"
	},
	"msg": "success",
	"timestamp": 1700014786,
	"trace_id": ""
}
```

HTTP请求

`GET https://gateway.toolstest.btc.com/rest/api/v1.0/nodeapi/tx/rawtx/:txId`

### 请求参数

| 参数名 | 示例值 | 参数类型 | 是否必填 | 参数描述 | 默认值 |
| --- | --- | ---- | ---- | ---- | ---- |
| txId |  | string | 是 | 需要查询的交易ID | 空 |

### 返回参数

| 参数名 | 参数类型 | 参数描述 |
| --- | --- | ---- | 
| rawtx | string | 交易原始数据 |

 
## 查询交易状态
  
  判断交易是否已经上链

```shell
curl --request GET \
  --url https://gateway.toolstest.btc.com/rest/api/v1.0/nodeapi/tx/status/c2e8fd13d6bea9d54aeceb83dbbeff631adf4003dce37175fce524e7e28795c2 \
  --header 'X-API-TOKEN: t648e371d1e70aeaad812ecaa9ff438b65043b1e745a0b50e1854eb8042dfa328'
```


```python
import http.client

conn = http.client.HTTPSConnection("gateway.toolstest.btc.com")

headers = { 'X-API-TOKEN': "t648e371d1e70aeaad812ecaa9ff438b65043b1e745a0b50e1854eb8042dfa328" }

conn.request("GET", "/rest/api/v1.0/nodeapi/tx/status/c2e8fd13d6bea9d54aeceb83dbbeff631adf4003dce37175fce524e7e28795c2", headers=headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))

```

> 响应示例

```
{
	"code": 0,
	"data": {
		"status": 1
	},
	"msg": "success",
	"timestamp": 1700015048,
	"trace_id": ""
}
```

HTTP请求

`GET https://gateway.toolstest.btc.com/rest/api/v1.0/nodeapi/tx/status/:txId`

### 请求参数

| 参数名 | 示例值 | 参数类型 | 是否必填 | 参数描述 | 默认值 |
| --- | --- | ---- | ---- | ---- | ---- |
| txId |  | string | 是 | 需要查询的交易ID | 空 |

### 返回参数

| 参数名 | 参数类型 | 参数描述 |
| --- | --- | ---- | 
| status | int | 1:已上链 0:未上链 |


## 查询某个特定UTXO是否被花费
  

```shell
curl --request GET \
  --url https://gateway.toolstest.btc.com/rest/api/v1.0/nodeapi/tx/utxo-status/038e09e7afd9a82d74f58e58753fed32e7f4302b6c957957968f564bfa408eac/1 \
  --header 'X-API-TOKEN: t648e371d1e70aeaad812ecaa9ff438b65043b1e745a0b50e1854eb8042dfa328'
```


```python
import http.client

conn = http.client.HTTPSConnection("gateway.toolstest.btc.com")

headers = { 'X-API-TOKEN': "t648e371d1e70aeaad812ecaa9ff438b65043b1e745a0b50e1854eb8042dfa328" }

conn.request("GET", "/rest/api/v1.0/nodeapi/tx/utxo-status/038e09e7afd9a82d74f58e58753fed32e7f4302b6c957957968f564bfa408eac/1", headers=headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))

```

> 响应示例

```
{
	"code": 0,
	"data": {
		"addr": "bc1qwqdg6squsna38e46795at95yu9atm8azzmyvckulcc7kytlcckxswvvzej",
		"height": 815069,
		"tx_id": "038e09e7afd9a82d74f58e58753fed32e7f4302b6c957957968f564bfa408eac",
		"output_index": 1,
		"amount": 12283240,
		"timestamp": 1698985176,
		"is_coinbase": false,
		"spend_tx_id": "606198df944eafb5c74ff13a8196affa598586fa1ba9130845457f43b82c8c65",
		"spend_input_index": 0,
		"spend_height": 815072,
		"spend_timestamp": 1698987608
	},
	"msg": "success",
	"timestamp": 1700015246,
	"trace_id": ""
}
```

HTTP请求

`GET https://gateway.toolstest.btc.com/rest/api/v1.0/nodeapi/tx/utxo-status/:txId/:index`

### 请求参数

| 参数名 | 示例值 | 参数类型 | 是否必填 | 参数描述 | 默认值 |
| --- | --- | ---- | ---- | ---- | ---- |
| txId |  | string | 是 | 需要查询的交易ID |  |
| index|  | int | 是 | 交易的第几笔输出| |

### 返回参数

| 参数名 | 参数类型 | 参数描述 |
| --- | --- | ---- | 
| addr | string | 拥有此UTXO的地址 |
| height | int | 产生此UTXO的高度 |
| tx_id | string | 产生此UTXO的交易hash |
| output_index | int | 此UTXO位于交易的第几条输出 |
| amount | int64 | 此UTXO的金额 |
| timestamp | int64 | 产生此UTXO的时间 |
| is_coinbase | bool | 是否是coinBase交易产生的UTXO的 |
| spend_tx_id | string | 话费此UTXO的交易hash |
| spend_input_index | string | 花费此UTXO时位于交易的第几条输入 |
| spend_height | int | 花费此UTXO的高度 |
| spend_timestamp | int64 | 花费此UTXO的时间戳 |



## 查询某个交易产生的UTXO花费情况
  

```shell
curl --request GET \
  --url https://gateway.toolstest.btc.com/rest/api/v1.0/nodeapi/tx/utxo-status-list/038e09e7afd9a82d74f58e58753fed32e7f4302b6c957957968f564bfa408eac \
  --header 'X-API-TOKEN: t648e371d1e70aeaad812ecaa9ff438b65043b1e745a0b50e1854eb8042dfa328'
```


```python
import http.client

conn = http.client.HTTPSConnection("gateway.toolstest.btc.com")

headers = { 'X-API-TOKEN': "t648e371d1e70aeaad812ecaa9ff438b65043b1e745a0b50e1854eb8042dfa328" }

conn.request("GET", "/rest/api/v1.0/nodeapi/tx/utxo-status-list/038e09e7afd9a82d74f58e58753fed32e7f4302b6c957957968f564bfa408eac", headers=headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))

```

> 响应示例

```
{
	"code": 0,
	"data": [
		{
			"addr": "3GQx7yJLjn9HVqwKV2EhgBWAQq6pxi7vcN",
			"height": 815069,
			"tx_id": "038e09e7afd9a82d74f58e58753fed32e7f4302b6c957957968f564bfa408eac",
			"output_index": 0,
			"amount": 5400000,
			"timestamp": 1698985176,
			"is_coinbase": false,
			"spend_tx_id": "46b29e7a85a91296dad41fa743275ecfd1a72668726c21aca9c08eeb803e06f5",
			"spend_input_index": 11,
			"spend_height": 815118,
			"spend_timestamp": 1699014849
		},
		{
			"addr": "bc1qwqdg6squsna38e46795at95yu9atm8azzmyvckulcc7kytlcckxswvvzej",
			"height": 815069,
			"tx_id": "038e09e7afd9a82d74f58e58753fed32e7f4302b6c957957968f564bfa408eac",
			"output_index": 1,
			"amount": 12283240,
			"timestamp": 1698985176,
			"is_coinbase": false,
			"spend_tx_id": "606198df944eafb5c74ff13a8196affa598586fa1ba9130845457f43b82c8c65",
			"spend_input_index": 0,
			"spend_height": 815072,
			"spend_timestamp": 1698987608
		}
	],
	"msg": "success",
	"timestamp": 1700016016,
	"trace_id": ""
}
```

HTTP请求

`GET https://gateway.toolstest.btc.com/rest/api/v1.0/nodeapi/tx/utxo-status-list/:txId/`

### 请求参数

| 参数名 | 示例值 | 参数类型 | 是否必填 | 参数描述 | 默认值 |
| --- | --- | ---- | ---- | ---- | ---- |
| txId |  | string | 是 | 需要查询的交易ID |  |


### 返回参数

| 参数名 | 参数类型 | 参数描述 |
| --- | --- | ---- | 
| addr | string | 拥有此UTXO的地址 |
| height | int | 产生此UTXO的高度 |
| tx_id | string | 产生此UTXO的交易hash |
| output_index | int | 此UTXO位于交易的第几条输出 |
| amount | int64 | 此UTXO的金额 |
| timestamp | int64 | 产生此UTXO的时间 |
| is_coinbase | bool | 是否是coinBase交易产生的UTXO的 |
| spend_tx_id | string | 话费此UTXO的交易hash |
| spend_input_index | string | 花费此UTXO时位于交易的第几条输入 |
| spend_height | int | 花费此UTXO的高度 |
| spend_timestamp | int64 | 花费此UTXO的时间戳 |

# 区块相关

## 获取区块数据

```shell
curl --request GET \
  --url https://gateway.toolstest.btc.com/rest/api/v1.0/nodeapi/block/info/80 \
  --header 'X-API-TOKEN: t648e371d1e70aeaad812ecaa9ff438b65043b1e745a0b50e1854eb8042dfa328'
```


```python
import http.client

conn = http.client.HTTPSConnection("gateway.toolstest.btc.com")

headers = { 'X-API-TOKEN': "t648e371d1e70aeaad812ecaa9ff438b65043b1e745a0b50e1854eb8042dfa328" }

conn.request("GET", "/rest/api/v1.0/nodeapi/block/info/80", headers=headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))

```

> 响应示例

```
{
	"code": 0,
	"data": {
		"blockInfo": {
			"hash": "00000000be62325a98e47d3306f275efe45d39d8cd5df28c795c96b8920ad48f",
			"confirmations": 795887,
			"height": 80,
			"version": 1,
			"versionHex": "00000001",
			"merkleroot": "5ed287fa7b07229b53b15c6ad95ab49c1c222aa0fcfa2dd6603fa8492fae54c6",
			"time": 1231646077,
			"mediantime": 1231631822,
			"nonce": 320393266,
			"bits": "1d00ffff",
			"difficulty": 1,
			"chainwork": "0000000000000000000000000000000000000000000000000000005100510051",
			"nTx": 1,
			"previousblockhash": "0000000086e318e8c348dad73199bb6fac8cc1effb9c872a7dda49c5caca0021",
			"nextblockhash": "000000007d076a5289751e363ecb01604355730e69de270f04f79d969ff1cf86",
			"transactions": [
				{
					"txid": "5ed287fa7b07229b53b15c6ad95ab49c1c222aa0fcfa2dd6603fa8492fae54c6",
					"hash": "5ed287fa7b07229b53b15c6ad95ab49c1c222aa0fcfa2dd6603fa8492fae54c6",
					"version": 1,
					"size": 134,
					"vsize": 134,
					"weight": 536,
					"locktime": 0,
					"vin": [
						{
							"txid": "",
							"vout": 0,
							"scriptSig": {
								"asm": "",
								"hex": ""
							},
							"txinwitness": null,
							"sequence": 4294967295,
							"coinbase": "04ffff001d0104",
							"prevout": {
								"value": 0,
								"n": 0,
								"address": "",
								"height": 0,
								"txid": "",
								"timestamp": 0,
								"is_coinbase": true
							}
						}
					],
					"vout": [
						{
							"value": 50,
							"n": 0,
							"scriptPubKey": {
								"asm": "04e869b280ddc2eb25995d46264e9b94d99214f0c3c3eec3b285c54bba7658edce94a3dca09c2206ec7a81aa1aef82bbf43c548f19731c37744c2bd97742e84173 OP_CHECKSIG",
								"desc": "",
								"hex": "4104e869b280ddc2eb25995d46264e9b94d99214f0c3c3eec3b285c54bba7658edce94a3dca09c2206ec7a81aa1aef82bbf43c548f19731c37744c2bd97742e84173ac",
								"address": "1BwWdLV5wbnZvSYfNA8zaEMqEDDjvA99wX",
								"type": "pubkey"
							},
							"offset": 0,
							"is_miner": false
						}
					],
					"hex": "",
					"blockhash": "",
					"blocktime": 0,
					"rectime": 0,
					"height": 0,
					"is_confirm": false
				}
			]
		}
	},
	"msg": "success",
	"timestamp": 1700016220,
	"trace_id": ""
}
```

HTTP请求

`GET https://gateway.toolstest.btc.com/rest/api/v1.0/nodeapi/block/info/:height`

### 请求参数

| 参数名 | 示例值 | 参数类型 | 是否必填 | 参数描述 | 默认值 |
| --- | --- | ---- | ---- | ---- | ---- |
| height | 80 | int | 是 | 需要查询的区块高度 |  |


### 返回参数

| 参数名 | 参数类型 | 参数描述 |
| --- | --- | ---- | 
| hash | string | 区块hash |
| height | int | 区块高度 | 
| version | int | 区块版本|
| versionHex | string | 区块版本16进制表示 |
| merkleroot | string | merkle根 |
| time | int64 | 产生区块的时间戳 |
| nonce | uint32 | 产生区块的随机数 |
| Bits | string |  |
| difficulty | float64 | |
| nTx | int | 区块包含的交易总数 | 
| previousblockhash | string | 上一个区块hash | 
| nextblockhash | string | 下一个区块hash | 
| transactions | array | 交易列表 | 

#### 交易列表
| 参数名 | 参数类型 | 参数描述 |
| --- | --- | ---- | 
| hash | string | 交易hash |
| txid | string | 交易id |
| version | int | |
| size | int | |
| viszie | int | |
| weight | int | |
| locktime | int | |
| vin | array | 交易的输入信息|
| vout | array | 交易的输出信息|
| Hex | string |  交易的16进制信息 | 



## 获取区块交易列表

```shell
curl --request GET \
  --url https://gateway.toolstest.btc.com/rest/api/v1.0/nodeapi/block/txlist/800000 \
  --header 'X-API-TOKEN: t648e371d1e70aeaad812ecaa9ff438b65043b1e745a0b50e1854eb8042dfa328'
```

```python
import http.client

conn = http.client.HTTPSConnection("gateway.toolstest.btc.com")

headers = { 'X-API-TOKEN': "t648e371d1e70aeaad812ecaa9ff438b65043b1e745a0b50e1854eb8042dfa328" }

conn.request("GET", "/rest/api/v1.0/nodeapi/block/txlist/800000", headers=headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))

```

> 响应示例

```
{
	"code": 0,
	"data": {
		"txCount": 1,
		"txIdList": [
			"5b340f8c400f59467ecbda0c7f43bdba6e021668424949a02eff377b17b04448"
		]
	},
	"msg": "success",
	"timestamp": 1700016577,
	"trace_id": ""
}
```

HTTP请求

`GET https://gateway.toolstest.btc.com/rest/api/v1.0/nodeapi/block/txlist/:height`

### 请求参数

| 参数名 | 示例值 | 参数类型 | 是否必填 | 参数描述 | 默认值 |
| --- | --- | ---- | ---- | ---- | ---- |
| height | 800 | int | 是 | 需要查询的区块高度 |  |


### 返回参数

| 参数名 | 参数类型 | 参数描述 |
| --- | --- | ---- | 
| txcount | int | 区块交易条数 | 
| txIdList | array | 交易id列表|



## 根据高度获取blockHeader

```shell
curl --request GET \
  --url https://gateway.toolstest.btc.com/rest/api/v1.0/nodeapi/block/header-height/800000 \
  --header 'X-API-TOKEN: t648e371d1e70aeaad812ecaa9ff438b65043b1e745a0b50e1854eb8042dfa328'
```

```python
import http.client

conn = http.client.HTTPSConnection("gateway.toolstest.btc.com")

headers = { 'X-API-TOKEN': "t648e371d1e70aeaad812ecaa9ff438b65043b1e745a0b50e1854eb8042dfa328" }

conn.request("GET", "/rest/api/v1.0/nodeapi/block/header-height/800000", headers=headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))

```

> 响应示例

```
{
	"code": 0,
	"data": {
		"hash": "00000000000000000002a7c4c1e48d76c5a37902165a270156b7a8d72728a054",
		"confirmations": 16811,
		"height": 800000,
		"version": 874340352,
		"versionHex": "341d6000",
		"merkleroot": "91f01a00530c8c83617190048ea8b0814d506cf24dfdbcf8893f8f0cab7f0855",
		"time": 1690168629,
		"mediantime": 1690165851,
		"nonce": 106861918,
		"bits": "17053894",
		"difficulty": 53911173001054.59,
		"chainwork": "00000000000000000000000000000000000000004fc85ab3390629e495bf13d5",
		"nTx": 3721,
		"previousblockhash": "000000000000000000012117ad9f72c1c0e42227c2d042dca23e6b96bd9fbb55",
		"nextblockhash": "00000000000000000000e26b239cf19ec7ace5edd9694d51a3f6933247720947"
	},
	"msg": "success",
	"timestamp": 1700016672,
	"trace_id": ""
}
```

HTTP请求

`GET https://gateway.toolstest.btc.com/rest/api/v1.0/nodeapi/block/header-height/:height`

### 请求参数

| 参数名 | 示例值 | 参数类型 | 是否必填 | 参数描述 | 默认值 |
| --- | --- | ---- | ---- | ---- | ---- |
| height | 800 | int | 是 | 需要查询的区块高度 |  |


### 返回参数

| 参数名 | 参数类型 | 参数描述 |
| --- | --- | ---- | 
| hash | string | 区块hash |
| height | int | 区块高度 | 
| version | int | 区块版本|
| versionHex | string | 区块版本16进制表示 |
| merkleroot | string | merkle根 |
| time | int64 | 产生区块的时间戳 |
| nonce | uint32 | 产生区块的随机数 |
| Bits | string |  |
| difficulty | float64 | |
| nTx | int | 区块包含的交易总数 | 
| previousblockhash | string | 上一个区块hash | 
| nextblockhash | string | 下一个区块hash | 
| transactions | array | 交易列表 | 



## 根据区块hash获取blockHeader

```shell
curl --request GET \
  --url https://gateway.toolstest.btc.com/rest/api/v1.0/nodeapi/block/header-hash/00000000000000000002a7c4c1e48d76c5a37902165a270156b7a8d72728a054 \
  --header 'X-API-TOKEN: t648e371d1e70aeaad812ecaa9ff438b65043b1e745a0b50e1854eb8042dfa328'
```

```python
import http.client

conn = http.client.HTTPSConnection("gateway.toolstest.btc.com")

headers = { 'X-API-TOKEN': "t648e371d1e70aeaad812ecaa9ff438b65043b1e745a0b50e1854eb8042dfa328" }

conn.request("GET", "/rest/api/v1.0/nodeapi/block/header-hash/00000000000000000002a7c4c1e48d76c5a37902165a270156b7a8d72728a054", headers=headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))

```

> 响应示例

```
{
	"code": 0,
	"data": {
		"hash": "00000000000000000002a7c4c1e48d76c5a37902165a270156b7a8d72728a054",
		"confirmations": 16811,
		"height": 800000,
		"version": 874340352,
		"versionHex": "341d6000",
		"merkleroot": "91f01a00530c8c83617190048ea8b0814d506cf24dfdbcf8893f8f0cab7f0855",
		"time": 1690168629,
		"mediantime": 1690165851,
		"nonce": 106861918,
		"bits": "17053894",
		"difficulty": 53911173001054.59,
		"chainwork": "00000000000000000000000000000000000000004fc85ab3390629e495bf13d5",
		"nTx": 3721,
		"previousblockhash": "000000000000000000012117ad9f72c1c0e42227c2d042dca23e6b96bd9fbb55",
		"nextblockhash": "00000000000000000000e26b239cf19ec7ace5edd9694d51a3f6933247720947"
	},
	"msg": "success",
	"timestamp": 1700016853,
	"trace_id": ""
}
```

HTTP请求

`GET https://gateway.toolstest.btc.com/rest/api/v1.0/nodeapi/block/header-hash/:hash`

### 请求参数

| 参数名 | 示例值 | 参数类型 | 是否必填 | 参数描述 | 默认值 |
| --- | --- | ---- | ---- | ---- | ---- |
| hash | 00000000000000000002a7c4c1e48d76c5a37902165a270156b7a8d72728a054 | string | 是 | 需要查询的区块hash |  |


### 返回参数

| 参数名 | 参数类型 | 参数描述 |
| --- | --- | ---- | 
| hash | string | 区块hash |
| height | int | 区块高度 | 
| version | int | 区块版本|
| versionHex | string | 区块版本16进制表示 |
| merkleroot | string | merkle根 |
| time | int64 | 产生区块的时间戳 |
| nonce | uint32 | 产生区块的随机数 |
| Bits | string |  |
| difficulty | float64 | |
| nTx | int | 区块包含的交易总数 | 
| previousblockhash | string | 上一个区块hash | 
| nextblockhash | string | 下一个区块hash | 
| transactions | array | 交易列表 | 



## 根据区块高度获取区块hash

```shell
curl --request GET \
  --url https://gateway.toolstest.btc.com/rest/api/v1.0/nodeapi/block/hash/800000 \
  --header 'X-API-TOKEN: t648e371d1e70aeaad812ecaa9ff438b65043b1e745a0b50e1854eb8042dfa328'
```

```python
import http.client

conn = http.client.HTTPSConnection("gateway.toolstest.btc.com")

headers = { 'X-API-TOKEN': "t648e371d1e70aeaad812ecaa9ff438b65043b1e745a0b50e1854eb8042dfa328" }

conn.request("GET", "/rest/api/v1.0/nodeapi/block/hash/800000", headers=headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))

```

> 响应示例

```
{
	"code": 0,
	"data": {
		"hash": "00000000000000000002a7c4c1e48d76c5a37902165a270156b7a8d72728a054"
	},
	"msg": "success",
	"timestamp": 1700016967,
	"trace_id": ""
}
```

HTTP请求

`GET https://gateway.toolstest.btc.com/rest/api/v1.0/nodeapi/block/hash/:height`

### 请求参数

| 参数名 | 示例值 | 参数类型 | 是否必填 | 参数描述 | 默认值 |
| --- | --- | ---- | ---- | ---- | ---- |
| height | 800 | int | 是 | 需要查询的区块高度 |  |


### 返回参数

| 参数名 | 参数类型 | 参数描述 |
| --- | --- | ---- | 
| hash | string | 区块hash |

