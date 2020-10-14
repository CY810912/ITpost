## EC的農地辣麼大，作物辣麼多，來認真找作物了（2）ES的逐一說文解字-Range & 常用的旁支末節

---

來到了倒數第二天  
真是快被榨乾了呢（還真是沒料 _(┐「﹃ﾟ｡)_） 
但說好寫三十篇技術文就是要灌滿三十天！！！ 

## 那麼今天也是忙碌且突然的開始囉！！  

[第一段參考文獻](https://www.elastic.co/blog/apache-lucene-numeric-filters)
首先我們知道Lucene能夠將字串直接拿來解析做查詢
爲了方便再把之前的東西貼過來
```
gt: > 大於（greater than）  
lt: < 小於（less than）  
gte: >= 大於或等於（greater than or equal to）  
lte: <= 小於或等於（less than or equal to  ）
```

# Range
那樣的事情我們叫做
1. 字串決定查詢類型
官方定義有兩種
   1. TermRangeQuery
   2. NumericRangeQuery

[詳文請見小農的前篇](https://ithelp.ithome.com.tw/articles/10249540)

2. 日期字串的範圍查詢 [參考文檔](https://www.elastic.co/guide/en/elasticsearch/client/curator/5.8/filtertype_period.html)
```js
//關鍵詞有以下
y-Years、M-Months、w-Weeks、d-Days、h-Hours、H-Hours、m-Minutes、s-Seconds
```
對範圍搜所有三個可以共用的概念  
    1. Date Math   
    2. Date Math to round  
    3. 日期格式（依據預設日期格式與指定時區進行檢索)  

假設今天想要查詢目前月整個月份，資料
gte就會查詢大於等於的日期（經過四捨五入）
lte就會查詢小於等於的日期（經過四捨五入）
假定 now 2020-10-13 (寫的文章時間)
```json
//Date Math 
{
    "query": {
        "range" : {
            "createAt" : {
                //這樣就會查詢出十月後的資料
                "gte" : "now/M",
            }
        }
    }
}
//這樣的話就等同於
//mysql:where createAt >= "2020-10-01 00:00:00" 
```
```json
//Date Math to round 
{
    "query": {
        "range" : {
            "createAt" : {
                "gte" : "now-1d/d",
                "lt" :  "now/d"
            }
        }
    }
}
//這樣的話就等同於
//mysql:where createAt >= "2020-10-13 00:00:00" and createAt  < "2020-10-14 00:00:00"
```
```json
//日期格式
{
    "query": {
        "range" : {
            "createAt" : {
                "gte": "13/10/2020",
                "lte": "2021",
                "format": "dd/MM/yyyy||yyyy",
                "time_zone": "+08:00"
            }
        }
    }
}
//mysql:where createAt >= "2020-10-13T00:00:00Z" and createAt < "2021-01-01T00:00:00.000Z"
```

欸~基本最難得日期範圍檢索大概就這樣了
多用幾次就習慣了，雖然我也是從專案起用到專案快結束了才習慣呢~
---

# ES 的旁支末節

## 1. Bool:這東西整天看到，但到底是啥？感覺有點像Boolean?  

欸~如果看官有這種感覺，那很有天份呢~

其實Bool query就是最少一個的布林子句所夠成  
也就是說每個子句結果最後吐出都必須爲true or false  
然而這個常出現的母集合其實只有四種子句
也就是我們上篇所提到的
1. filter
2. must
3. must_not
4. should

## 2. bulk:要塞的東西太多筆了，要不一次擠進去吧-ES的insert doc 批量操作
### 這樣的話就可以一個請求寫入多筆資料了
```
POST _bulk
{"index":{"_index":"it_help","_type":"_doc","_id":"1"}}
{ "id" : 2,"content":"我愛臺灣,國慶快樂" }
{"index":{"_index":"it_help","_type":"_doc","_id":"2"}}
{ "id" : 2,"content":"我愛臺灣,國慶快樂" }
{"index":{"_index":"it_help","_type":"_doc","_id":"3"}}
{ "id" : 3,"content":"愛臺灣,國慶快樂" }
{"index":{"_index":"it_help","_type":"_doc","_id":"4"}}
{ "id" : 4,"content":"國慶快樂,全國愛臺灣" }
```

## 3. ES的get? url Search  [參考文檔](https://www.elastic.co/guide/en/elasticsearch/reference/7.9/search-search.html#search-api-query-params-q)
### 範例：GET /{index}/_search?q=username:jacky
其中呢username=Field,jacky就是我們要找的值啦

[參考文檔](https://zhuanlan.zhihu.com/p/112824485)
下面列出比較常用的參數呢爲榮
* df：預設字段，不指定時會對所有字段進行查詢
* sort：根據字段名排序
* from：返回的索引匹配結果的開始值，默認為 0
* size：搜索結果返回的條數，默認為 10
//分頁就靠上面這兩個，每次查詢多少，查到第幾頁
* timeout：超時的時間設置
* fields：只返回索引中指定的列，多個列中間用逗號分開
* analyzer：當分析查詢字符串的時候使用的分詞器
* analyze_wildcard：通配符或者前綴查詢是否被分析，預設為 false
* explain：在每個返回結果中，將包含評分機制的解釋
* _source：是否包含元數據，同時支持 _source_includes 和 _source_excludes
* lenient：若設置為 true，字段類型轉換失敗的時候將被忽略，默認為 false
* default_operator：默認多個條件的關系，AND 或者 OR，默認為 OR複雜布林查詢用的

---

第29篇終於結束了
來回去加班囉~

---
## 對於文章中的任何用詞與專業字或都可以在下面
###### ~~梗圖看不懂也行~~
# 蔽農會帶着謙卑爲懷的心情竭盡所能回覆與說明的