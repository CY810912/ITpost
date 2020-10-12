## EC的農地辣麼大，作物辣麼多，來認真找作物了（1）ES的逐一說文解字-搜尋

---

來到第28天了，卻覺得頭很痛
ES的收尋知識點有點大，要細講自己也講不清楚
要粗講，可能又講的不清楚  

我的老天那

---

# 最後幾篇，文思枯竭，那麼我們就直接開始囉~

[本篇參考文章](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-your-data.html)

ES小農經過小小的到處東找西查，最後還是覺得官網最詳細，只可惜大量英文....
（中文版只有更新到2.x...看過就覺得直接打上來，可能很多都廢棄了）  

官網基本將ES的收尋簡單分成了五個單元
今天小農就依照這五個單元簡單一次就入門吧~  

# 1. 搜尋基礎  
[參考文檔](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-filter-context.html)

    1.1 query context
        * 會對搜尋進行算分(_score)
        * 使用query關鍵包裹詞語
          1. must: 必須符合
          2. should: 選擇性符合
          3. must_not: 必須不符合 
    2.2 filter context
        * 不會對搜尋進行算分，但能夠利用暫存更快速取得資料
          1. 必須符合

範例語句:
```js
GET /{index}/_search
{
    "query":{
        "bool":{
            //1. 必須住在新北市
            "must":{
                "term":{
                    "city":"New Taipei City"
                }
            },
            //2. 年紀必須大於等於35
            "must_not":[
                {
                    "range":{
                        "age":{
                            "gte":"30"
                        }
                    }
                }
            ],
            //3. 名字中有Tsai,Jolin,之中一個即可，符合越多分數越高
            "should":[
                {
                    "term":{
                        "name":"Tsai",
                    }
                },
                {
                    "term":{
                        "name":"Jolin",
                    }
                }
            ],
            //4. 必須爲女性
             "filter":{
                "term":{
                    "sex":"women"
                }
            }
        }
    }
}
```

# 2. 分詞(Full text queries)
[參考文檔](https://www.elastic.co/guide/en/elasticsearch/reference/current/full-text-queries.html) 

    2.1. match 
        * 將搜索語句經過分詞後進行"OR"查詢
    2.2. match_phrase 
        * 將搜索語句經過分詞後，按照分詞順訊進行"and"搜尋，且可使用slop容許分詞間中間間格幾個詞
         （就是分詞全要有，且順序要對)
    2.3. match_phrase_prefix 
        * 對最後一個分詞進行通配符搜索，並且可使用max_expansions來設定糢糊比對數控制
          其中 max_expansions 最小爲1
    2.4. multi_match
        * match的多組版本，將每組內容都進行分詞後進行"OR"查詢
    2.5. common terms
        * 對不常見的指定分詞給與更高的算分
    2.6. query_string
        * 允許在單個查詢字串中使用and,or,not進行多字段搜索
    2.7. simple_query_string
        * 像前面單元的lucene，直接將語句進行搜尋

# 三個很像的來點小栗子吧
## 我們假設資料爲以下
{ "id" : 1,"content":"國慶快樂,我愛臺灣" }
{ "id" : 2,"content":"我愛臺灣,國慶快樂" }
{ "id" : 3,"content":"愛臺灣,國慶快樂" }
{ "id" : 4,"content":"國慶快樂,全國愛臺灣" }
## match
``` 
{
  "query":{
    "match":{
      "content.ik_smart_analyzer":"國慶快樂"
    }
  }
}
==
mysql where keyWork = 國慶 or keyWork = 快樂
```

## match_phrase
```
{
    "query": {
        "match_phrase": {
            "content.ik_smart_analyzer": {
            	"query": "快樂我愛",
            	"slop":1
            }
        }
    }
}
==
mysql where keyWork = 國慶 and 國慶_Position=0 and keyWork = 我愛 and 我愛_Position=1
```
## match_phrase_prefix
```
{
  "query": {
    "match_phrase_prefix": {
      "content.ik_smart_analyzer": {
        "query": "臺",
        "max_expansions": 1
      }
    }
  }
}
===
mysql where keyWord = 臺 or keyWord like “臺?”
```

# 3. 精確比對(Term-level queries)
將搜尋語句進行"分詞"的完全配對搜索
```json
{ "id" : 1,"content":"國慶快樂,我愛臺灣" }
{ "id" : 2,"content":"我愛臺灣,國慶快樂" }
{ "id" : 3,"content":"愛臺灣,國慶快樂" }
{ "id" : 4,"content":"國慶快樂,全國愛臺灣" }
```
[參考文檔](https://www.elastic.co/guide/en/elasticsearch/reference/current/term-level-queries.html)

    3.1. term 
        搜索詞不會被分詞，必須完全符合
```json
//查無資料因爲沒有分詞:國慶快樂,我愛臺灣
{
  "query": {
    "term" : { "content" : "國慶快樂,我愛臺灣" }
  }
}
//id:1~4皆會查出
{
  "query": {
    "term" : { "content" : "臺灣" }
  }
}
```
    3.2. terms 
        等於mysql "in" 將所有該屬性符合配對都找出
```json
//id:1~4皆會查出
{
  "query": {
    "terms" : { "content" : ["國慶","臺灣"]}
  }
}
```
    3.3. terms_set
        至少符合一個或多個分詞字段
    3.4. range 
        ## //第29篇 基礎搜尋的進階說明
    3.5. exists 
        查詢某個屬性不爲空的資料
```json
"query": {
    "exists" : { "field" : "content" }
}
```
    3.6. prefix 
        直接搜尋以某字段開頭的資料
```json
"query": {
    "prefix" : { "content": "國" }
}
//==sql where content like "國%”
```
    3.7. wildcard
```json
"query": {
    "wildcard" : { "content": "*國" }
}
//==sql where content like "%國”
"query": {
    "wildcard" : { "content": "國?" }
}
//where author like "國_”
``` 
    3.8. regexp 
    3.9. fuzzy 
        拼字錯誤或容錯查詢
```json
"query": {
    "fuzzy" : {
        "author": {
            "value": "我礙臺灣",
            "fuzziness": 1,//最大錯別字長度
            "prefix_length": 1,//開頭至少要對幾個字
            "max_expansions": 100//最多搜尋結果
        }
    }
}
```
    3.10. type 
        7.x被棄用，可能以後被刪除
    3.11. ids 
        依據多個id[]來檢索文檔
 
---

原本想將五類都打完
明天來詳講range的花式用法
bool到底是什麼  
然後一些常用api吧~

---
## 對於文章中的任何用詞與專業字或都可以在下面留言提問 
###### ~~梗圖看不懂也行~~
# 蔽農會帶着謙卑爲懷的心情竭盡所能回覆與說明的
