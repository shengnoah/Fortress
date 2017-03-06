# MoonScript for GrayLog
GrayLog REST API Wrapper for Moonscript


此程序是Graylog REST对外提供的API的一个Moonscript的Wrapper封装，把REST接口提供的数据服务，变成通过函数调用的方式取得相应REST接口返回的数据。


下面是一个实际的基于Lapis框架程序中调用此SDK的例子：



### Stream数据取得

```lua
class App extends lapis.Application
  "/testcase": =>
    --准备对应REST的输入参数，如果相应该有的项目没有设定会输出NG原因。
    param_data= {
        fields:'username',
        limit:3,
        query:'*',
        from: '2017-01-05 00:00:00',
        to:'2017-01-06 00:00:00',
        filter:'streams'..':'..'673b1666ca624a6231a460fa'
    }
    --进行鉴权信息设定
    url  = GMoonSDK\auth 'supervisor', 'password', '127.0.0.1', '12600'
    
    --调用对应'TYPE'相对应的REST服务，返回结果。
    ret = GMoonSDK\dealStream 's_ua', param_data
    ret
```    
上文提到 ‘TYPE’， 其实就是对Endpoints的一种编号，基本上和GrayLog REST API是一对一关系。

```lua
   endpoints: {
        's_uat':{'/search/universal/absolute/terms':{'field', 'query', 'from', 'to', 'limit'} }
        's_ua':{'/search/universal/absolute':{'fields', 'query', 'from', 'to', 'limit'} }
        's_urt':{'/search/universal/relative/terms':{'field', 'query', 'range'} }
        's_ut':{'/search/universal/relative':{'fields', 'query', 'range'} }
    }
```
理论上说，可以个修改以上的数据结构，对应各种REST API的服封装(GET),只要知道对应URL与可接收的参数列表。

### Dashboard的Widget数据更新

      rglog = require "GRestySDK"
      data = '{
       "description": "user-port",
       "type": "QUICKVALUES",
       "config": {
         "timerange": {
           "type": "relative",
           "range": 28800 
         },
         "field": "dst_port",
         "stream_id": "56e7ab11fd624ca91defeb11",
         "query": "username: candylab",
         "show_data_table": true,
         "show_pie_chart": true
       },
       "cache_time": 10
     }
     '
      url  = rglog\auth 'admin', 'password', '0.0.0.0', '12345'
      rglog\updateWidget('57a7bc60be624b691feab6f','019bca13-50cf-481e-a123-a0d2e649b41a',data)

