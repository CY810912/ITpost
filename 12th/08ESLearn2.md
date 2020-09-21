# 爲何有人說ES很難學？因爲他本身就反人類呀！這東西到底可以幹嘛(2)Elastic"Search"這件事

---

也有一群人說ES比起資料庫更像搜尋引擎
因爲ES他完全符合一個搜尋引擎該有的條件
只是他還附帶了存儲資料的功能

怎麽說他是搜尋引擎呢？
來隨便google一下吧
![](https://CY810912.github.io/th12img/search3.png)

## 1. 檢索:爬取內容  
## 2. 索引:進行分詞並建立分詞索引  
## 3. 排名:依據分詞索引內容建立倒排算出優先級  

---

### 基本只要以上三個就能說他是一個搜尋引擎  
~~只是很多人在第三步做壞了,google做的特好~~

那麼ES是怎麽實現的呢？
其實ElasticSerarch就是Lucene的封裝

啥？Lucene是什麼？封裝又是啥？
沒事，這不需要知道

![](https://CY810912.github.io/th12img/restfulapi.png)
## 這種事就跟你不需知道引擎原理就可以開車一樣  

ES也相同，並不需要搜尋引擎
只需要明白時下流行的 Restful api
並且也實現了分佈式架構水平擴展面對海量數據更加迎刃有餘

[不知道Restful API的銅學請點我，優質文章必收呀](https://progressbar.tw/posts/53)

# 接下來要理解ES有四個非常重要的名詞
超重要所以字超大

1. 索引index （表）
2. 類型Mapping （描述這張表的值屬性，**_數據建模的關鍵點_**）
3. 文檔Document （類似row data）

## 啥？索引？上篇不是講了嗎？又要混字數？
不對，混字數只是這兩行，這個索引跟倒排索引的索引不是一件事

來隨便看個簡單吧
```js
PUT users //index
{
    "mappings" : { //mapping
      "properties" : {
        "firstName" : {
          "type" : "text"
        },
        "lastName" : {
          "type" : "text"
        },
        "mobile" : {
          "type" : "keyword",
          "null_value": "NULL"
        }

      }
    }
}
//document
PUT users/_doc/1 
{
  "firstName":"cheng",
  "lastName": "jacky",
  "mobile": null
}
```

麻...大致上就是這樣等於建了個數據建模
與插入了一筆cheng jacky的資料了


## 老濕，不對呀，text 跟 keyword不是都是字串？有啥差別？
## 恩~這就是 Dynamic Mapping 的範圍了呢

---

看了看手錶與字數覺得今天可先下課啦


要回去耕田了，公司在寫虛擬幣，區塊鏈智能合約的東西也是相當有趣呢
希望明年的鐵人賽能有贊助，讓我有點動力去寫寫那東西

---

## 對於文章中的任何用詞與專業字或都可以在下面留言提問 
###### ~~梗圖看不懂也行~~
# 蔽農會帶着謙卑爲懷的心情竭盡所能回覆與說明的