# 我們都說自己是碼農，學怎麼學種菜跟收割也是門學問-EC-kibana不看API 讓他幫我生語句(2)來建立點簡單查詢吧

---

昨天閒聊到敝農學一樣新的東西會把東西抽象化跟就有已會的東西做比對  
啥意思？例如像這系列文章的開篇，可能不用先理解東西怎麼用，而是爲什麼要去學這東西？  

資料庫千奇百怪的  
要速度我選redis，要穩健我選mysql，要寫的快我選hbase，要讀得好我選es  

前端框架也是百花齊放  
今天公司要我引進前端框架有紅藍綠分別是~~小火龍,傑尼龜,妙蛙種子~~(angular,react,vue)
如果都是前端大神，我選講求函數面向的react讓大神各自發揮自己的強項   
如果是多數萌新流動高，我選約定大於設定，功能該寫哪就寫哪的VUE  
（angular呢？根本只是略懂呀，這學習曲線也太陡峭了吧，我好像一次再塞react,vue跟後端觀念）

所以呢，清楚掌握技術潮流與明白自己在幹嘛是很重要的~ 

--- 
## 那麼今天的正篇也一樣開始囉!!

昨天我們說到了簡單的基礎設定  
今天來從我們之前造飛機少的可憐的資料開始查吧_(┐「﹃ﾟ｡)_  

這邊我們直接建議使用KQL，最後再簡單展示下lucene長得如何
  

## 如果之前的index pattern與KQL都設定正確  
## 可以選擇想指向的index pattern與template屬性  
### 正常會出現下面兩個畫面
![](https://CY810912.github.io/th12img/ecTool/kibanaQuery0.png)  
![](https://CY810912.github.io/th12img/ecTool/kibanaQuery1.png)  

那麼來開始我們今天的查詢吧
下面這些是我們所有的資料，在未下指令的情況都會被列出  
![](https://CY810912.github.io/th12img/ecTool/kibanaQuery2.png)  

先試試水溫找出名爲jacky的碼農吧
![](https://CY810912.github.io/th12img/ecTool/kibanaQuery3.png)  

## ...實在不好意思分三張圖截....但每打一個在搜尋列都會自動提示後面能接什麼  
然後我們成功找出name有jacky的資料  

## 然後我們再拼接把前端的同事找出來吧
![](https://CY810912.github.io/th12img/ecTool/kibanaQuery4.png)  

## 如畫面，我們確實找出了依據目前爲止所需要的資料
## 但還是有點太簡單
## 我們再來拼接ap且爲men的同事吧

這裏爲何我們不找women呢？因爲要講解一件事情，ES通常都是預設糢糊搜尋的  
如果我只想只找man而不是把woman一起拉出來需要加""
![](https://CY810912.github.io/th12img/ecTool/kibanaQuery5.png)  

欸，如上圖結果都是我們想要的    

## 最後我們再將年齡18~25的同事濾出來結束這露露長的查詢吧
![](https://CY810912.github.io/th12img/ecTool/kibanaQuery6.png)  

這是我們所獲得最後的結果  
![](https://CY810912.github.io/th12img/ecTool/kibanaQuery7.png)  


## 這裏眼尖的看官應該發現只有下面的filter居然出現了Edit as Query DSL  

# 所以該不會換句話說，其實我們剛剛的語句都可已換成DEV tool看的懂的語句  
---
# 是的，藏的極深，真是找死我了  
# 看官們千萬別錯過這裏呀！！

## 點選上方的inspect=>request 就可以找出我們所選出的語句
![](https://CY810912.github.io/th12img/ecTool/kibanaQuery8.png)  

## 沒錯機器產出的kql 出現了，看官們自己體驗下，當然實際開發語句是可以縮減的  
只是再初學者，連and or都不知道的情況，這是相當便捷的查詢（反正不是你打） 
# 但是各位會發現 jacky的man被高亮了  
# 這是ES的查詢語句問題這裏必須使用nested(巢狀索引類型)

```json
//我是kql
 "query": {
    "bool": {
      "must": [],
      "filter": [
        {
          "bool": {
            "should": [
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
              },
              {
                "bool": {
                  "should": [
                    {
                      "bool": {
                        "should": [
                          {
                            "match": {
                              "name": "f2e"
                            }
                          }
                        ],
                        "minimum_should_match": 1
                      }
                    },
                    {
                      "bool": {
                        "filter": [
                          {
                            "bool": {
                              "should": [
                                {
                                  "match": {
                                    "name": "ap"
                                  }
                                }
                              ],
                              "minimum_should_match": 1
                            }
                          },
                          {
                            "bool": {
                              "should": [
                                {
                                  "match_phrase": {
                                    "sex": "man"
                                  }
                                }
                              ],
                              "minimum_should_match": 1
                            }
                          }
                        ]
                      }
                    }
                  ],
                  "minimum_should_match": 1
                }
              }
            ],
            "minimum_should_match": 1
          }
        },
        {
          "range": {
            "age": {
              "gte": 18,
              "lt": 25
            }
          }
        }
      ],
      "should": [],
      "must_not": []
    }
  },
```

最後我們切換成lucene看下  
//沒錯就是把我們的語句送出去而已，所以敲錯了字母也不會報錯就是直接500給你
```json
//我是lucene
"query": {
    "bool": {
      "must": [
        {
          "query_string": {
            "query": "name :jacky or name : f2e or  (name: ap and sex : \"man\" ) ",
            "analyze_wildcard": true,
            "time_zone": "Asia/Taipei"
          }
        }
      ],
      "filter": [
        {
          "range": {
            "age": {
              "gte": 18,
              "lt": 25
            }
          }
        }
      ],
      "should": [],
      "must_not": []
    }
  },
```

好的呢~  
今天看官們知道了科技始終來自於惰性  
這個月假超多，加班根本加不完  
時程趕不上呀~~我的老天  

---
## 對於文章中的任何用詞與專業字或都可以在下面留言提問 
###### ~~梗圖看不懂也行~~
# 蔽農會帶着謙卑爲懷的心情竭盡所能回覆與說明的