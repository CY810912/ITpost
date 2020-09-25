# 爲何有人說ES很難學？因爲他本身就反人類呀！這東西到底可以幹嘛(3-2)ES的牙牙學語模板類型

---
這應該是短期間ES的知識篇最後一篇.....  
脫離on cloud太久了，怕一個不小心被判失格就GG了  

但這些不寫後面引入資料跟建制模板，拉出資料根本講不下去呀  
真是令人爲難呢 on cloud這個詞  

欸，不是，沒在抱怨主辦方Σヽ(ﾟД ﾟ; )ﾉ  
只是後端技術宅就是比較愛探討怎麽用比較好  

---
## 正篇開始囉，今天來從上篇觀念繼續往下鑽，原本應該可能會有點硬
但最近加班實在加到腦細胞都快死光了，所以我們來簡單點  
原本想來寫Dynamic vs strict   
Nested的設定  
以及一些mapping的meta訊息  

但實在太深了，我也只是會用，要我寫文章  
我大概自己也看不懂(≖ᴗ≖๑)  

## 今天來個說文解字吧  
挑幾個常用常見的出來說說

點擊關鍵字可連往官方連結
身爲一個英文聾啞人士，如果還是文盲可能文章寫起來就有點吃力了呢

1. [Index Template](https://www.elastic.co/guide/en/elasticsearch/reference/7.1/indices-templates.html) & [Dynamic Template]( https://www.elastic.co/guide/en/elasticsearch/reference/7.1/dynamic-mapping.html)
   1. index Templat
    > 顧名思議直接由ES去幫助設定這張index的Mapping和相關Setting  
    按照預設規則自動配對到新創建的索引上,特性有以下
    * 這個template僅在索引被創建時才會生效，所以模板更動，已建模板不會被更動
    * 可以設定多個模板依照 index_patterns去做marge
    * 可以指定order數值來決定誰marge誰(數值越低越先被操作，之後相同屬性order較大則覆蓋上去)
  
超抽象，來舉個栗子  
![](https://CY810912.github.io/th12img/aExcemple.png)
```js 
PUT /_template/template_1
{
    "index_patterns" : ["*"],//套用至所有模板
    "order" : 0,//基底模板
    "settings" : {
        "number_of_shards" : 1 //指定套用分片
    },
    "mappings" : {
        "_source" : { "enabled" : false }//是否壓縮字段
    }
}

PUT /_template/template_2
{
    "index_patterns" : ["te*"],//套用至te開頭的模板
    "order" : 1,//覆蓋基底模板
    "settings" : {
        "number_of_shards" : 1 //指定套用分片
    },
    "mappings" : {
        "_source" : { "enabled" : true }//開啓壓縮字段
    }
}
```

   2. Dynamic Template 
   > 算是比較JS的簡單暴力模板類型，啥意思？ES覺得他是啥就是啥  
   * 所有字串都設定成Keyword  
   * 所有is 開頭的屬性通通設定爲布林值
   * 所有long 都被設定爲long

``` js
PUT my_index
{
    "mappings": {
        "dynamic_templates": [ 模板宣告
            {
                "strings_as_boolean": { //如果是字段且符合is開頭就設定爲boolean
                    "match_mapping_type":   "string",
                    "match":"is*",
                    "mapping": {
                        "type": "boolean"
                    }
                }
            },
            {
                "strings_as_keywords": { // 如果單存爲字段就設爲keyword
                    "match_mapping_type":   "string",
                    "mapping": {
                        "type": "keyword"
                    }
                }
            }
        ]
    }
}

```
---
天哪，這篇也是一個爆字雖然大步份是程式碼
但想傳達應該有傳達到了

果然寫維運的東西，大部分都是設定，寫起來就有點硬

是該收收東西明天一樣早起耕田~

---
## 對於文章中的任何用詞與專業字或都可以在下面留言提問 
###### ~~梗圖看不懂也行~~
# 蔽農會帶着謙卑爲懷的心情竭盡所能回覆與說明的