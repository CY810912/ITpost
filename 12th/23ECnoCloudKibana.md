# 我們都說自己是碼農，學怎麼學種菜跟收割也是門學問-EC-kibana不看API 讓他幫我生語句(1)先來搞懂KQL與lucene

---

如題，小弟乃一介在碼田裡耕耘的人  
有時耕耕咖啡豆(java)，有時搞搞觀光農場(F2E)   
作物產出太慢偶爾還要去看看引水設施(SQL)或植物出了啥問題(抓蟲)  
最麻煩的是有時還要管經銷商腦子到底裝啥(理解需求)

作為~~一把廉價的瑞士刀~~ 一個農夫  
要學的東西實在太多了，不建立起自己的一套系統太沒效率了  
比起拿起書直接啃，不如把這個東西給"抽象化"  
去明白這個東西的誕生是為了甚麼?  
他是一個過渡?還是一個突破?  

跟之前學的東西有甚麼不一樣，更適合拿來處理甚麼場景  
  
哎呀，接下來每天來交流這樣的東西一點點好了  
##### ~~不然字數怕會不夠~~
---  

## 正篇開始~  
首先我們明白elastic 就是個在資料庫與搜尋引擎間搖擺的東西   
每個碼農都知道資料的增刪改，佔整體開發的約20%  
各種花式查詢則佔整體的80%  
其中最多眉角要考慮的則是效能  
但效能這種東西往往都是要對東西一定熟悉度以上才能明白的  
那我們今天就來簡簡單單的學點查詢吧  
還記得我們前篇搞了個index pattern嗎?
[任意門在這裡](https://ithelp.ithome.com.tw/articles/10248081)  

沒錯沒錯，沒這個接下來就做不下去了 

那麼我們回到上次創建index pattern那頁有一個
saved Objects的地方
![](https://CY810912.github.io/th12img/ecTool/saveObject.png)  

這裡也是個很重要的地方  
大部分(至少我摸到現在)對kibana操作的存檔都會在這裡  
包含我們等等要產出的東西  

這邊會發現我們上次寫的it*這個index pattern已經在這裡了  

那這次再點選這裡開始我們的查詢吧!!
![](https://CY810912.github.io/th12img/ecTool/searchStart.png) 

首先我們已經知道其實elasticsearch就是lucene這把刀在掛上用restful實現查詢與加上其他分片與叢集功能的瑞士刀資料庫  
那麼因為lucene已經被封裝過了，後面實作了許許多多操作，因此可以使用restful與kibana定義出來的"KQL"進行查詢  
  
![](https://CY810912.github.io/th12img/ecTool/kql_lucene.png)  

## 我們可以在上圖設定"背景"要用哪種語言來做搜尋  

### 辣麼kql與lucene差別在哪裡呢?
其實只是表達方式不同而已
像是我是碼農與敝人是碼農的差別  

lucene比較導向於query_string
kql則是我們時常在各大網站上所通常搜出來與學到的語法

沒有哪個比較好  
囉嗦度與嚴謹度kql較高  
可以進行叫為精確的查詢

而語句直觀度lucene大勝  
(因為都是字串直接打上去)

先來甚麼都不說直接請出我們的例子君*2，看官們自己體驗下  
![](https://CY810912.github.io/th12img/aExcemple.png) 


```json
//我是lucene
"query": {
    "bool": {
        "must": [
        {
            "query_string": {
            "query": "name:jacky",
            "analyze_wildcard": true,
            "time_zone": "Asia/Taipei"
            }
        }
        ],
        "filter": [],
        "should": [],
        "must_not": []
    }
},
```
```json
//我是KQL
"query": {
    "bool": {
        "must": [],
        "filter": [
        {
            "bool": {
            "should": [
                {
                "match": {
                    "name": "jacky"
                }
                }
            ],
            "minimum_should_match": 1
            }
        }
        ],
        "should": [],
        "must_not": []
    }
},
```

好的咧，這篇光是研究背景知識就花了快三個多小時  
結果還不小心爆字了  

那麼今天先到這裡拉~
明天開始來試著用kibana內建搜尋搜出資料吧

---
## 對於文章中的任何用詞與專業字或都可以在下面留言提問 
###### ~~梗圖看不懂也行~~
# 蔽農會帶着謙卑爲懷的心情竭盡所能回覆與說明的
