## 基本信息
* 本篇列出REST接口的baseurl **https://api.55.com**
* 所有接口的响应都是JSON格式
* 所有时间、时间戳均为UNIX时间，单位为毫秒

- - - -

### 推送或通知说明
推送和通知使用基于http 的 SSE协议，详细内容请参考 [Server-Sent Events](https://www.w3.org/TR/eventsource/)  
注释为空为心跳信息，心跳周期为15s。  
推荐客户端：  
* [Java](https://github.com/heremaps/oksse)  
* [PHP](https://github.com/obsh/sse-client)  
* [Python](https://github.com/mpetazzoni/sseclient)
* [Go](https://github.com/r3labs/sse)
* [Javascript Browser](https://developer.mozilla.org/en-US/docs/Web/API/EventSource)  

示例：
```
GET /quote/realTime.stream HTTP/1.1
Host: api.55.com

event:_RESULT
data:{
data:  "symbol" : "EOSBTC",
data:  "last" : 0.0009249,
data:  "open" : 0.0009439,
data:  "volume" : 6665919.22,
data:  "high" : 0.0009486,
data:  "low" : 0.0009078,
data:  "previousClose" : 0.0009439,
data:  "previousCloseDate" : 1552262399603,
data:  "dateTime" : 1552295305521,
data:  "hour24Volume" : 19751238.72
data:}

event:_RESULT
data:{
data:  "symbol" : "EOSBTC",
data:  "last" : 0.0009247,
data:  "open" : 0.0009439,
data:  "volume" : 6665927.79,
data:  "high" : 0.0009486,
data:  "low" : 0.0009078,
data:  "previousClose" : 0.0009439,
data:  "previousCloseDate" : 1552262399603,
data:  "dateTime" : 1552295307245,
data:  "hour24Volume" : 19751247.29
data:}

:
:
```

- - - -

## 行情接口
### 接口列表
| 接口数据类型 | 请求方法                       | 类型 | 描述                                                                                          |
|--------------|--------------------------------|------|-----------------------------------------------------------------------------------------------|
| 市场行情     | GET quote/symbol.list          | GET  | [交易对列表查询](#交易对列表查询)                                                             |
| 市场行情     | GET quote/realTime.last        | GET  | [实时行情查询](#实时行情查询)                                                                 |
| 市场行情     | GET quote/realTime.stream      | GET  | [实时行情推送](#实时行情推送)                                                                 |
| 市场行情     | GET quote/tradeHistory.list    | GET  | [交易历史查询](#交易历史查询)                                                                 |
| 市场行情     | GET quote/tradeHistory.stream  | GET  | [交易历史推送](#交易历史推送)                                                                 |
| 市场行情     | GET quote/depth.list           | GET  | [深度行情查询](#深度行情查询)                                                                 |
| 市场行情     | GET quote/marketDepth.stream   | GET  | [深度行情推送](#深度行情推送)                                                                 |
| 市场行情     | GET quote/summarized.timeRange | GET  | [交易日级别以下K线数据（分钟K、小时K）查询](#交易日级别以下K线数据（分钟K、小时K）查询)       |
| 市场行情     | GET quote/summarized.trailing  | GET  | [交易日级别以下K线数据（分钟K、小时K）追溯](#交易日级别以下K线数据（分钟K、小时K）追溯)       |
| 市场行情     | GET quote/summarized.stream    | GET  | [交易日级别以下K线数据（分钟K、小时K）推送](#交易日级别以下K线数据（分钟K、小时K）推送)       |
| 市场行情     | GET quote/historical.timeRange | GET  | [交易日级别及以上K线数据（日K、周K、月K）查询](#交易日级别及以上K线数据（日K、周K、月K）查询) |
| 市场行情     | GET quote/historical.trailing  | GET  | [交易日级别及以上K线数据（日K、周K、月K）追溯](#交易日级别及以上K线数据（日K、周K、月K）追溯) |
| 市场行情     | GET quote/historical.stream    | GET  | [交易日级别及以上K线数据（日K、周K、月K）推送](#交易日级别及以上K线数据（日K、周K、月K）推送) |

- - - -

### 交易对列表查询
```
GET: /quote/symbol.list
```
#### 请求参数
无
    
#### 响应数据
| 属性       | 类型   | 说明     |
|------------|--------|----------|
| symbol     | String | 交易对   |
| baseAsset  | String | 交易资产 |
| quoteAsset | String | 计价资产 |

#### 请求响应示例
```
HTTP GET: https://api.55.com/quote/symbol.list
```

```json5
[ 
  {
    "symbol" : "AAPLTUSDT",
    "baseAsset" : "AAPLT",
    "quoteAsset" : "USDT"
  },
  {
    "symbol" : "BATBTC",
    "baseAsset" : "BAT",
    "quoteAsset" : "BTC"
  }
]
```
- - - -
### 实时行情查询
```
GET: /quote/realTime.last
```

#### 请求参数
| 属性   | 类型   | 说明   |
|--------|--------|--------|
| symbol | String | 交易对 |

>  **symbol为空时返回所有交易对最新行情数据**
    
#### 响应数据
| 属性              | 类型       | 说明               |
|-------------------|------------|--------------------|
| symbol            | String     | 交易对             |
| last              | BigDecimal | 最新价格           |
| open              | BigDecimal | 当前交易日开盘价   |
| high              | BigDecimal | 当前交易日最高价   |
| low               | BigDecimal | 当前交易日最低价   |
| volume            | BigDecimal | 当前交易日成交量   |
| hour24Volume      | BigDecimal | 24小时滚动成交量   |
| dateTime          | Date       | 快照时间           |
| previousClose     | BigDecimal | 上一个交易日收盘价 |
| previousCloseDate | Date       | 上一个交易日日期   |

#### 请求响应示例
```
HTTP GET: https://api.55.com/quote/realTime.last?symbol=FFBTC&symbol=BTCUSDT
```

```json5
{
  "BTCUSDT" : {
    "status" : "SUCCESS",
    "message" : null,
    "data" : {
      "symbol" : "BTCUSDT",
      "last" : 3884.45,
      "open" : 3916.95,
      "volume" : 7497.6178,
      "high" : 3937.39,
      "low" : 3863.39,
      "previousClose" : 3916.95,
      "previousCloseDate" : 1552262398744,
      "dateTime" : 1552295492755,
      "hour24Volume" : 19142.1758
    }
  },
  "FFBTC" : {
    "status" : "SUCCESS",
    "message" : null,
    "data" : {
      "symbol" : "FFBTC",
      "last" : 0.00001483,
      "open" : 0.000014757,
      "volume" : 1883751,
      "high" : 0.000014893,
      "low" : 0.000014669,
      "previousClose" : 0.000014757,
      "previousCloseDate" : 1552262398543,
      "dateTime" : 1552295493519,
      "hour24Volume" : 5211762
    }
  }
}
```
> 备注：5站行情快照是以天为基础的成交快照，每天UTC 0点 open，high，low，volume，previousClose，previousCloseDate字段 将重新计算
- - - -

### 实时行情推送

实时行情推送
```
GET：/quote/realTime.stream
```

#### 请求信息
| 属性               | 类型    | 说明             |
|--------------------|---------|------------------|
| symbol             | String  | 交易对（可多个） |
| [symbol]_startTime | Long    | 追溯快照开始时间 |
| [symbol]_least     | Integer | 追溯快照条目数   |

#### 响应信息
| 属性              | 类型       | 说明               |
|-------------------|------------|--------------------|
| symbol            | String     | 交易对             |
| last              | BigDecimal | 最新价格           |
| open              | BigDecimal | 当前交易日开盘价   |
| high              | BigDecimal | 当前交易日最高价   |
| low               | BigDecimal | 当前交易日最低价   |
| volume            | BigDecimal | 当前交易日成交量   |
| hour24Volume      | BigDecimal | 24小时滚动成交量   |
| dateTime          | Date       | 快照时间           |
| previousClose     | BigDecimal | 上一个交易日收盘价 |
| previousCloseDate | Date       | 上一个交易日日期   |

#### 请求响应示例
```
HTTP GET：https://api.55.com/quote/realTime.stream?symbol=EOSBTC&EOSBTC_least=1&EOSBTC_startTime=1552295280000
```

````
event:_RESULT
data:{
data:  "symbol" : "EOSBTC",
data:  "last" : 0.0009249,
data:  "open" : 0.0009439,
data:  "volume" : 6665919.22,
data:  "high" : 0.0009486,
data:  "low" : 0.0009078,
data:  "previousClose" : 0.0009439,
data:  "previousCloseDate" : 1552262399603,
data:  "dateTime" : 1552295305521,
data:  "hour24Volume" : 19751238.72
data:}

event:_RESULT
data:{
data:  "symbol" : "EOSBTC",
data:  "last" : 0.0009247,
data:  "open" : 0.0009439,
data:  "volume" : 6665927.79,
data:  "high" : 0.0009486,
data:  "low" : 0.0009078,
data:  "previousClose" : 0.0009439,
data:  "previousCloseDate" : 1552262399603,
data:  "dateTime" : 1552295307245,
data:  "hour24Volume" : 19751247.29
data:}

:
````
- - - -

### 交易历史查询
```
GET: /quote/tradeHistory.list
```
#### 请求参数
| 属性   | 类型    | 说明       |
|--------|---------|------------|
| symbol | String  | 交易对代码 |
| least  | Integer | 追溯条目数 |
    
#### 响应数据
| 属性      | 类型        | 说明       |
|-----------|-------------|------------|
| symbol    | String      | 交易对代码 |
| amount    | BigDecimal  | 成交数量   |
| price     | BigDecimal  | 成交价     |
| direction | buy 或 sell | 成交方向   |
| tradeTime | Long        | 成交时间   |

#### 请求响应示例
```
HTTP GET：https://api.55.com/quote/tradeHistory.list?symbol=ETCBTC&least=5
```
```json5
{
  "status" : "SUCCESS",
  "message" : null,
  "data" : [ {
    "symbol" : "ETCBTC",
    "amount" : 6.26,
    "price" : 0.001125,
    "direction" : "buy",
    "tradeTime" : 1544692454726
  }, {
    "symbol" : "ETCBTC",
    "amount" : 1.40,
    "price" : 0.001125,
    "direction" : "buy",
    "tradeTime" : 1544692449717
  }, 
  {
    "symbol" : "ETCBTC",
    "amount" : 8.24,
    "price" : 0.001121,
    "direction" : "buy",
    "tradeTime" : 1544692250470
  } ]
}
```
- - - -

### 交易历史推送
```
GET: /quote/tradeHistory.stream
``` 
#### 请求参数
| 属性   | 类型    | 说明       |
|--------|---------|------------|
| symbol | String  | 交易对代码 |
| least  | Integer | 追溯条目数 |
    
#### 响应数据
| 属性      | 类型        | 说明       |
|-----------|-------------|------------|
| symbol    | String      | 交易对代码 |
| amount    | BigDecimal  | 成交数量   |
| price     | BigDecimal  | 成交价     |
| direction | buy 或 sell | 成交方向   |
| tradeTime | Long        | 成交时间   |

#### 请求响应示例
```
HTTP GET：https://api.55.com/quote/tradeHistory.stream?symbol=ETCBTC&least=5
```
````
event:_RESULT
data:{
data:  "symbol" : "BTCUSDT",
data:  "amount" : 0.0980,
data:  "price" : 3844.02,
data:  "direction" : "buy",
data:  "tradeTime" : 1552356466306
data:}

event:_RESULT
data:{
data:  "symbol" : "BTCUSDT",
data:  "amount" : 0.0679,
data:  "price" : 3836.98,
data:  "direction" : "sell",
data:  "tradeTime" : 1552356470311
data:}

event:_RESULT
data:{
data:  "symbol" : "BTCUSDT",
data:  "amount" : 0.0783,
data:  "price" : 3836.98,
data:  "direction" : "buy",
data:  "tradeTime" : 1552356511348
data:}

:
````
- - - -

### 深度行情查询
```
GET: /quote/depth.list
```
#### 请求参数
| 属性   | 类型   | 说明       |
|--------|--------|------------|
| symbol | String | 交易对代码 |
    
#### 响应数据
| 属性      | 类型                                                  | 说明         |
|-----------|-------------------------------------------------------|--------------|
| symbol    | String                                                | 交易对代码   |
| updatedAt | Date                                                  | 更新时间     |
| bids      | List Of [MarketDepthPosition](#market-depth-position) | 委买数据列表 |
| asks      | List Of [MarketDepthPosition](#market-depth-position) | 委卖数据列表 |

#### 请求响应示例
```
HTTP GET：https://api.55.com/quote/depth.list?symbol=FFUSDT
```
```json5
{
  "status" : "SUCCESS",
  "message" : null,
  "data" : {
    "symbol" : "FFUSDT",
    "updatedAt" : 1543899236435,
    "bids" : [ {
      "price" : 0.01268,
      "quantity" : 440.0
    }, {
      "price" : 0.0126,
      "quantity" : 320.0
    }, {
      "price" : 0.01245,
      "quantity" : 20
    }, {
      "price" : 0.01229,
      "quantity" : 23
    }, {
      "price" : 0.01218,
      "quantity" : 1860
    }],
    "asks" : [ {
      "price" : 0.01293,
      "quantity" : 29
    }, {
      "price" : 0.01307,
      "quantity" : 27
    }, {
      "price" : 0.01323,
      "quantity" : 31
    }, {
      "price" : 0.01339,
      "quantity" : 26
    }, {
      "price" : 0.01355,
      "quantity" : 31
    }]
  }
}
```
- - - -

### 深度行情推送
```
GET: /quote/marketDepth.stream
```
#### 请求参数
| 属性   | 类型   | 说明       |
|--------|--------|------------|
| symbol | String | 交易对代码 |
    
#### 响应数据
| 属性      | 类型                                                  | 说明         |
|-----------|-------------------------------------------------------|--------------|
| symbol    | String                                                | 交易对代码   |
| updatedAt | Date                                                  | 更新时间     |
| bids      | List Of [MarketDepthPosition](#market-depth-position) | 委买数据列表 |
| asks      | List Of [MarketDepthPosition](#market-depth-position) | 委卖数据列表 |

#### 请求响应示例
```
HTTP GET：https://api.55.com/quote/marketDepth.stream?symbol=EOSBTC
```
````
event:_RESULT
data:{
data:  "symbol" : "EOSBTC",
data:  "updatedAt" : 1552357491143,
data:  "bids" : [ {
data:    "price" : 0.0009171,
data:    "quantity" : 3.44
data:  }, {
data:    "price" : 0.0009165,
data:    "quantity" : 52.24
data:  }, {
data:    "price" : 0.0009156,
data:    "quantity" : 8.82
data:  }, {
data:    "price" : 0.0008924,
data:    "quantity" : 17.47
data:  }, {
data:    "price" : 0.0008912,
data:    "quantity" : 7.77
data:  }, {
data:    "price" : 0.0007084,
data:    "quantity" : 4.67
data:  } ],
data:  "asks" : [ {
data:    "price" : 0.0009183,
data:    "quantity" : 6.9
data:  }, {
data:    "price" : 0.0009195,
data:    "quantity" : 51.07
data:  }, {
data:    "price" : 0.0009202,
data:    "quantity" : 17.91
data:  }, {
data:    "price" : 0.0009212,
data:    "quantity" : 96.17
data:  }, {
data:    "price" : 0.0009434,
data:    "quantity" : 8.35
data:  }, {
data:    "price" : 0.0009449,
data:    "quantity" : 8.17
data:  } ]
data:}

:
````
- - - -

### 交易日级别以下K线数据（分钟K、小时K）查询
```
GET: /quote/summarized.timeRange
```
#### 请求参数
| 属性          | 类型                                                         | 说明                 |
|---------------|--------------------------------------------------------------|----------------------|
| symbol        | String                                                       | 交易对代码           |
| interval      | enum [summarized-quote-interval](#summarized-quote-interval) | K线类型              |
| startDateTime | Long                                                         | 开始时间（忽略精度） |
| endDateTime   | Long                                                         | 结束时间（忽略精度） |

> startDateTime与endDateTime查询需要忽略精度，若查询1分钟K则忽略秒和毫秒的精度。  
例： 查询2018年12月04日13:05到2018年12月04日13:10之间的分钟K  
    2018年12月04日13:05 => startDateTime:1543899900000  
    2018年12月04日13:10 => endDateTime:1543900200000  

    
#### 响应数据
| 属性          | 类型       | 说明     |
|---------------|------------|----------|
| symbol        | String     | 交易对   |
| open          | BigDecimal | 开盘价   |
| high          | BigDecimal | 最高价   |
| low           | BigDecimal | 最低价   |
| close         | BigDecimal | 收盘价   |
| volume        | BigDecimal | 成交量   |
| openDateTime  | Date       | 开始时间 |
| closeDateTime | Date       | 结束时间 |

#### 请求响应示例
```
HTTP GET：https://api.55.com/quote/summarized.timeRange?symbol=FFUSDT&interval=MINUTE_1&startDateTime=1543108140000&endDateTime=1543640940000
```
```json5
{
  "status": "SUCCESS",
  "message": null,
  "data": [
    {
      "symbol": "FFUSDT",
      "open": 0.013,
      "high": 0.01301,
      "low": 0.01299,
      "close": 0.01301,
      "volume": 193.0,
      "openDateTime": 1543640880000,
      "closeDateTime": 1543640940000
    },
    {
      "symbol": "FFUSDT",
      "open": 0.013,
      "high": 0.013,
      "low": 0.013,
      "close": 0.013,
      "volume": 71.0,
      "openDateTime": 1543640820000,
      "closeDateTime": 1543640880000
    },
    {
      "symbol": "FFUSDT",
      "open": 0.01298,
      "high": 0.01301,
      "low": 0.01298,
      "close": 0.01301,
      "volume": 181.0,
      "openDateTime": 1543640760000,
      "closeDateTime": 1543640820000
    },
    {
      "symbol": "FFUSDT",
      "open": 0.01299,
      "high": 0.01299,
      "low": 0.01299,
      "close": 0.01299,
      "volume": 54.0,
      "openDateTime": 1543640700000,
      "closeDateTime": 1543640760000
    },
    {
      "symbol": "FFUSDT",
      "open": 0.01304,
      "high": 0.01306,
      "low": 0.01304,
      "close": 0.01306,
      "volume": 173.0,
      "openDateTime": 1543640640000,
      "closeDateTime": 1543640700000
    }
    /* 更多数据省略 */
  ]
}
```
- - - -

### 交易日级别以下K线数据（分钟K、小时K）追溯
```
GET: /quote/summarized.trailing
```
#### 请求参数
| 属性         | 类型                                                         | 说明                 |
|--------------|--------------------------------------------------------------|----------------------|
| symbol       | String                                                       | 交易对代码           |
| interval     | enum [summarized-quote-interval](#summarized-quote-interval) | K线类型              |
| endDateTime  | Long                                                         | 结束时间（忽略精度） |
| periods      | int                                                          | 追溯时间单位个数     |
| dateTimeUnit | enum [date-time-unit](#date-time-unit)                       | 追溯时间单位         |

> 忽略精度参考【交易日级别以下K线数据查询#请求参数】

#### 响应数据
| 属性          | 类型       | 说明     |
|---------------|------------|----------|
| symbol        | String     | 交易对   |
| open          | BigDecimal | 开盘价   |
| high          | BigDecimal | 最高价   |
| low           | BigDecimal | 最低价   |
| close         | BigDecimal | 收盘价   |
| volume        | BigDecimal | 成交量   |
| openDateTime  | Date       | 开始时间 |
| closeDateTime | Date       | 结束时间 |

#### 请求响应示例
```
HTTP GET：https://api.55.com/quote/summarized.trailing?symbol=ETHUSDT&endDateTime=1552176000000&interval=MINUTE_1&dateTimeUnit=MINUTE_1&periods=2
```
```json5
{
    "status": "SUCCESS",
    "message": null,
    "data": [
        {
            "symbol": "ETHUSDT",
            "open": 137.27,
            "high": 137.31,
            "low": 137.16,
            "close": 137.22,
            "volume": 366.8362,
            "openDateTime": 1552176000000,
            "closeDateTime": 1552176060000
        },
        {
            "symbol": "ETHUSDT",
            "open": 137.33,
            "high": 137.4,
            "low": 137.21,
            "close": 137.24,
            "volume": 584.4181,
            "openDateTime": 1552175940000,
            "closeDateTime": 1552176000000
        },
        {
            "symbol": "ETHUSDT",
            "open": 137.32,
            "high": 137.4,
            "low": 137.3,
            "close": 137.34,
            "volume": 510.7989,
            "openDateTime": 1552175880000,
            "closeDateTime": 1552175940000
        }
    ]
}
```
- - - -

### 交易日级别以下K线数据（分钟K、小时K）推送
```
GET: /quote/summarized.stream
```
#### 请求参数
| 属性     | 类型                                                         | 说明    |
|----------|--------------------------------------------------------------|---------|
| symbol   | String                                                       | 交易对  |
| interval | enum [summarized-quote-interval](#summarized-quote-interval) | K线类型 |

#### 响应数据
| 属性          | 类型                                                         | 说明     |
|---------------|--------------------------------------------------------------|----------|
| interval      | enum [summarized-quote-interval](#summarized-quote-interval) | K线类型  |
| symbol        | String                                                       | 交易对   |
| open          | BigDecimal                                                   | 开盘价   |
| high          | BigDecimal                                                   | 最高价   |
| low           | BigDecimal                                                   | 最低价   |
| close         | BigDecimal                                                   | 收盘价   |
| volume        | BigDecimal                                                   | 成交量   |
| openDateTime  | Date                                                         | 开始时间 |
| closeDateTime | Date                                                         | 结束时间 |

#### 请求响应示例
```
HTTP GET：http://www.55:com/quote/summarized.stream?symbol=BTCUSDT&interval=MINUTE_1
```
````
event:_RESULT
data:{
data:  "interval" : "MINUTE_1",
data:  "symbol" : "BTCUSDT",
data:  "open" : 3836.98,
data:  "high" : 3842.74,
data:  "low" : 3836.98,
data:  "close" : 3836.98,
data:  "volume" : 0.2484,
data:  "openDateTime" : 1552361820000,
data:  "closeDateTime" : 1552361880000
data:}

event:_RESULT
data:{
data:  "interval" : "MINUTE_1",
data:  "symbol" : "BTCUSDT",
data:  "open" : 3835.06,
data:  "high" : 3835.06,
data:  "low" : 3835.06,
data:  "close" : 3835.06,
data:  "volume" : 0.0401,
data:  "openDateTime" : 1552361880000,
data:  "closeDateTime" : 1552361940000
data:}

:
````

- - - -
### 交易日级别及以上K线数据（日K、周K、月K）查询
```
GET: /quote/historical.timeRange
```
#### 请求参数
| 属性      | 类型                                                         | 说明                 |
|-----------|--------------------------------------------------------------|----------------------|
| symbol    | String                                                       | 交易对代码           |
| interval  | enum [historical-quote-interval](#historical-quote-interval) | K线类型              |
| startDate | Long                                                         | 开始日期（忽略精度） |
| endDate   | Long                                                         | 结束日期（忽略精度） |

> 忽略精度参考【交易日级别以下K线数据查询#请求参数】 

#### 响应数据
| 属性              | 类型       | 说明               |
|-------------------|------------|--------------------|
| symbol            | String     | 交易对             |
| open              | BigDecimal | 开盘价             |
| high              | BigDecimal | 最高价             |
| low               | BigDecimal | 最低价             |
| close             | BigDecimal | 收盘价             |
| volume            | BigDecimal | 成交量             |
| openDate          | Date       | 开始日期           |
| closeDate         | Date       | 结束日期           |
| previousClose     | BigDecimal | 上一个交易日收盘价 |
| previousCloseDate | Date       | 上一个交易日日期   |
| change            | BigDecimal | 涨跌量             |
| changePercent     | BigDecimal | 涨跌幅             |

#### 请求响应示例
```
HTTP GET：https://api.55.com/quote/historical.timeRange?symbol=FFUSDT&interval=DAY&startDate=1541289600000&endDate=1543881600000
```
```json5
{
  "status": "SUCCESS",
  "message": null,
  "data": [
    {
      "symbol": "FFUSDT",
      "open": 0.0137,
      "high": 0.0137,
      "low": 0.01266,
      "close": 0.01298,
      "volume": 291416.0,
      "openDate": 1543795200000,
      "closeDate": 1543881600000,
      "previousClose": 0.01366,
      "previousCloseDate": 1543795200000,
      "change": -0.00068,
      "changePercent": -4.9800
    },
    {
      "symbol": "FFUSDT",
      "open": 0.01378,
      "high": 0.01411,
      "low": 0.01335,
      "close": 0.01366,
      "volume": 287871.0,
      "openDate": 1543708800000,
      "closeDate": 1543795200000,
      "previousClose": 0.01378,
      "previousCloseDate": 1543708800000,
      "change": -0.00012,
      "changePercent": -0.8700
    },
    /* 更多数据省略 */
  ]
}
```
- - - -

### 交易日级别及以上K线数据（日K、周K、月K）追溯
```
GET: /quote/historical.trailing
```
#### 请求参数
| 属性         | 类型                                                         | 说明                 |
|--------------|--------------------------------------------------------------|----------------------|
| symbol       | String                                                       | 交易对代码           |
| interval     | enum [historical-quote-interval](#historical-quote-interval) | K线类型              |
| endDate      | Long                                                         | 结束日期（忽略精度） |
| periods      | int                                                          | 追溯时间单位个数     |
| dateTimeUnit | enum [date-time-unit](#date-time-unit)                       | 追溯时间单位         |

> 忽略精度参考【交易日级别以下K线数据查询#请求参数】 

#### 响应数据
| 属性              | 类型       | 说明               |
|-------------------|------------|--------------------|
| symbol            | String     | 交易对             |
| open              | BigDecimal | 开盘价             |
| high              | BigDecimal | 最高价             |
| low               | BigDecimal | 最低价             |
| close             | BigDecimal | 收盘价             |
| volume            | BigDecimal | 成交量             |
| openDate          | Date       | 开始日期           |
| closeDate         | Date       | 结束日期           |
| previousClose     | BigDecimal | 上一个交易日收盘价 |
| previousCloseDate | Date       | 上一个交易日日期   |
| change            | BigDecimal | 涨跌量             |
| changePercent     | BigDecimal | 涨跌幅             |

#### 请求响应示例
```
HTTP GET：https://api.55.com/quote/historical.trailing?symbol=ETHUSDT&endDate=1552176000000&interval=DAY&dateTimeUnit=DAY&periods=2
```
```json5
{
    "status": "SUCCESS",
    "message": null,
    "data": [
        {
            "symbol": "ETHUSDT",
            "open": 137.27,
            "high": 137.52,
            "low": 134.17,
            "close": 135.42,
            "volume": 534204.5049,
            "openDate": 1552176000000,
            "closeDate": 1552262400000,
            "previousClose": 137.24,
            "previousCloseDate": 1552176000000,
            "change": -1.82,
            "changePercent": -1.33
        },
        {
            "symbol": "ETHUSDT",
            "open": 133.56,
            "high": 138.59,
            "low": 133.02,
            "close": 137.24,
            "volume": 628692.092,
            "openDate": 1552089600000,
            "closeDate": 1552176000000,
            "previousClose": 133.56,
            "previousCloseDate": 1552089600000,
            "change": 3.68,
            "changePercent": 2.76
        },
        {
            "symbol": "ETHUSDT",
            "open": 136.87,
            "high": 139.23,
            "low": 130.77,
            "close": 133.56,
            "volume": 445643.7546,
            "openDate": 1552003200000,
            "closeDate": 1552089600000,
            "previousClose": 136.87,
            "previousCloseDate": 1552003200000,
            "change": -3.31,
            "changePercent": -2.42
        }
    ]
}
```
- - - -

### 交易日级别及以上K线数据（日K、周K、月K）推送

```
GET: /quote/historical.stream
```
#### 请求信息
| 属性     | 类型                                                         | 说明    |
|----------|--------------------------------------------------------------|---------|
| symbol   | String                                                       | 交易对  |
| interval | enum [historical-quote-interval](#historical-quote-interval) | K线类型 |

#### 响应信息
| 属性              | 类型                                                         | 说明               |
|-------------------|--------------------------------------------------------------|--------------------|
| interval          | enum [historical-quote-interval](#historical-quote-interval) | K线类型            |
| symbol            | String                                                       | 交易对             |
| open              | BigDecimal                                                   | 开盘价             |
| high              | BigDecimal                                                   | 最高价             |
| low               | BigDecimal                                                   | 最低价             |
| close             | BigDecimal                                                   | 收盘价             |
| volume            | BigDecimal                                                   | 成交量             |
| openDate          | Date                                                         | 开始日期           |
| closeDate         | Date                                                         | 结束日期           |
| previousClose     | BigDecimal                                                   | 上一个交易日收盘价 |
| previousCloseDate | Date                                                         | 上一个交易日日期   |
| change            | BigDecimal                                                   | 涨跌量             |
| changePercent     | BigDecimal                                                   | 涨跌幅             |

#### 请求响应示例
```
HTTP GET：https://api.55.com/quote/historical.stream?symbol=BTCUSDT&interval=DAY
```

````
event:_RESULT
data:{
data:  "interval" : "DAY",
data:  "symbol" : "BTCUSDT",
data:  "open" : 3871.98,
data:  "high" : 3891.88,
data:  "low" : 3828.53,
data:  "close" : 3835.06,
data:  "volume" : 138.0711,
data:  "openDate" : 1552348800000,
data:  "closeDate" : 1552435200000,
data:  "previousClose" : 3876.3,
data:  "previousCloseDate" : 1552348798269,
data:  "change" : -41.24,
data:  "changePercent" : -1.06
data:}

:
````
- - - -

## 枚举

### summarized-quote-interval

> 日级别以下K线类型

| 枚举      | 说明    |
|-----------|---------|
| MINUTE_1  | 1分钟K  |
| MINUTE_5  | 5分钟K  |
| MINUTE_15 | 15分钟K |
| MINUTE_30 | 30分钟K |
| HOUR_1    | 1小时K  |
| HOUR_2    | 2小时K  |
| HOUR_4    | 4小时K  |
| HOUR_6    | 6小时K  |
| HOUR_8    | 8小时K  |
| HOUR_12   | 12小时K |

### historical-quote-interval

> 日级别及以上K线类型

| 枚举  | 说明 |
|-------|------|
| DAY   | 日   |
| WEEK  | 周   |
| MONTH | 月   |

### market-depth-position

> 盘口数据

| 属性名   | 类型       | 说明   |
|----------|------------|--------|
| price    | BigDecimal | 委托价 |
| quantity | BigDecimal | 委托量 |

### date-time-unit

> 时间单位

| 枚举      | 说明   |
|-----------|--------|
| MINUTE_1  | 1分钟  |
| MINUTE_5  | 5分钟  |
| MINUTE_15 | 15分钟 |
| MINUTE_30 | 30分钟 |
| HOUR_1    | 1小时  |
| HOUR_2    | 2小时  |
| HOUR_4    | 4小时  |
| HOUR_6    | 6小时  |
| HOUR_8    | 8小时  |
| HOUR_12   | 12小时 |
| DAY       | 日     |
| WEEK      | 周     |
| MONTH     | 月     |


- - - -
