## 這篇來介紹關於Elastic on cloud的農具好了，關於Elastic專有名詞說文解字：基本操作篇(上)索引(index)與資料(doc)的增刪改查(CURD)

---

最近跟許多部門的同事聊天，希望可以稍微也理解一下別的領域的專業~~跟探聽各種小道消息~~  
最近真是被on Cloud這東西搞得很頭大GCP，AWS各種雲服務  
身爲只會略懂操作linux的小農來說，那完全是片森林  
東西好像有點多，又有點千篇一律，但又各有巧妙  
坑也不像程式那樣顯而易見，往往都是真的運作了，請求多起來時才發現  
## 出大事惹~  

碼農決定是否學一個東西主要著重在lib本身穩定與社群大小  
以及是否好維護足以快速叠代需求  
運維則是關心於圖表，數據，能夠在不影響運作的情況下給與多少目前的資訊  
並在神不知鬼不覺的情況下完成所有的問題排錯，標準的沒出事就是好事  

與不同的部門交流可以學習到不同面向的東西，原本前端後來開發後端  
更能明白接口怎麽開對前端來說是好做的  

套句我主管說的，接口可以後端先定義，前端做了不舒服可以討論來改
重點是"討論"~  

## 會去關心理解需求與業務幫助公司賺錢的工程師才能提升思維與視野而且少做工呢~
![](https://CY810912.github.io/th12img/ecTool/dogcom.jpg)  

## [點我看原文~](https://www.facebook.com/itdogcom/photos/a.1006163632885366/1659522980882758)

# 那麼今天的正篇開始囉~真是就算拆了上下篇也是肯定爆字的標題呢  

# 今天請愛用ctrl(cmd)+f, talk is cheap show you code 
### 資料想像請沿用之前連接spring boot的簡寫到剩下name


#  1. 首先先來寫寫索引（index）的CRUD

#  1.1. 新增索引Insert 
一般很多公司會直接寫個指令集將所有.template直接寫入本機ES開始開發  
就可以少裝很多套件
```js
# 創建索引名為 it_help 的索引
//因爲是給人看json加個pettery
PUT _template/it_help?pretty
{
    //創建index_patterns
    "index_patterns": ["it*"],
    // 對於it_help這個索引的設置
    "settings": {
        "index": {
            "number_of_shards": 3, // 分片數量設置為3，預設為5
            "number_of_replicas": 3 // 副本數量設置為3，預設為1
        }
    },
    //創建index別名
    "aliases": {
       "{index}_alias":{}
    },
    // 對於it_help映射配置(又名數據建模或資料建模)
    "mappings": {
        "_doc": {
            "dynamic": false, // 動態映射配置
            // 字段屬性配置
            "properties": {
                // 表示字串id，類型為integer
                "id": {
                    "type": "integer"  
                },
                "name": {
                    "type": "text",
                    "analyzer": "jieba-analysis", // javam用繁體存儲時的分詞器
                },
                "createAt": {
                    "type": "date"
                }
            }
        }
    }
}
```

# 1.2. 刪除索引
這裏的刪除索引幾乎等於mysql 的 drop table請慎用，資料不見會跑路的
```js
DELETE /it_help
```
# 1.3 修改索引
```js
// 修改設定：修改副本數
PUT /it_help/_settings
{
    "index" : {
        "number_of_replicas" : 2
    }
}

// 修改設定：修改分片刷新時間,預設為1s（ES本身寫入很慢，分片刷新後才能在下指令查到）
PUT /it_help/_settings
{
    "index" : {
        "refresh_interval" : "2s"
    }
}

// 修改模板：新增字段 age
PUT /it_help/_mapping/_doc 
{
  "properties": {
    "age": {
      "type": "integer"
    }
  }
}
```

# 1.4. 查詢索引 
```js
//查詢
GET /it_help
//查詢所有索引（_cat可以查詢某個類下所有目錄)
GET /_cat/indices
```

#  2. 資料(_doc)的CRUD...D可能下篇再詳細寫

# 2.1. 新增資料    
資料新增有三種，指定id，ES幫你指定ID，指定ES怎麽幫你指定ID   
第三個有點抽象且高概率可能會報error小弟本身並沒有用過    
這東西從官網來看似乎都是處理資料同步問題時使用  
[參考資料~附上~](https://www.elastic.co/guide/en/elasticsearch/guide/2.x/create-doc.html)
```js
// 新增單條數據，並指定es的id 為 1
PUT /it_help/_doc/{id}?pretty
{
  "name": "java tom"
}
// 新增單條數據，使用ES自動生成id
POST /it_help/_doc?pretty
{
  "name": "java tomas"
}

// 使用 op_type 屬性，強制執行某種操作
PUT it_help/_doc/{id}?op_type=create
{
     "name": "f2e zoe"
}
```
# 2.2. 刪除資料
```js
//根據id，刪除單個數據
DELETE /it_help/_doc/{id}

//根據查詢刪除
POST it_help/_delete_by_query
{
  "query": { 
    "match": {
     "name": "f2e zoe"
    }
  }
}

```
# 2.3. 更新資料
依據Restful修改單條數據，修改語句和新增語句相同，根據ID，存在更新；不存在新增
```js
PUT /it_help/_doc/{id}?pretty
{
  "name": "app android"
}

// 根據查詢條件name=java jacky，修改name="更新後的name"
（版本沖突而不會導致_update_by_query 中止）
POST /it_help/_update_by_query
{
  "script": {
    "source": "ctx._source.name = params.name",
    "lang": "painless",
    "params":{
      "name":"java jacky2"
    }
  },
  "query": {
    "term": {
      "name": "java jacky"
    }
  }
}
```

# 2.4. 簡單資料查詢...下篇在寫詳細的,寫到這已經快失智( º﹃º )
```js
// 1.根據id，獲取單個數據
GET /it_help/_doc/1
// 2.獲取索引下的所有數據
GET /it_help/_doc/_search
// 3.簡單單一查詢
GET /it_help/_doc/_search
{
  "query": {
    "match": {
      "name": "java jacky2"
    }
  }
}
```

---

好咧，超難找的文章
一篇之內template跟index的增刪改查完成了  

被判都沒寫oncloud但也是功德無量了
回去加班拉，變天了，感冒+加班+鐵人賽_(┐「﹃ﾟ｡)_
頂扣扣


[此篇參考資料~](https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html)

---
## 對於文章中的任何用詞與專業字或都可以在下面留言提問 
###### ~~梗圖看不懂也行~~
# 蔽農會帶着謙卑爲懷的心情竭盡所能回覆與說明的