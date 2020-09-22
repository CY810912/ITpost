# 爲何有人說ES很難學？因爲他本身就反人類呀！這東西到底可以幹嘛(3-1)讓ES牙牙學語- Dynamic Mapping

---
3-1是個什麼東西  
覺得這個如同資料庫的建表，不學好根本就是直接玩壞ES  
甚至有些東西沒設定好，上線之後還需要停機調整  

停機調整是個什麼概念？  
假設說一間電商，一個小時可能一百張單（超小型電商的那種）  
可能因爲工程師捅包，log沒寫好，無法統計用戶消費習慣  
必須因爲這種小事停止接單~~(老闆還不抽你？)~~  

---

創建一張表或一個mapping我們通常稱之爲 **_數據建模_**  
一個念起來帥氣又專業的東西    

接下來敝農將以一個資淺後端的角度來說明"ES"數據建模的考量與注意點  
~~就是不用關係型資料庫概念，打錯了還請鞭大力點，矯正下敝農的三觀~~

## 1. 資料需求與性能需求的考量   
1. 資料需求:
   > 我們又稱之爲邏輯模型  
   主要依據實際
   1. 資料本身相關欄位需求
   2. 資料表與資料表之間關係連結需求
   3. 搜索相關的配置
   以上三點進行考量
2. 性能需求:
    > 我們又稱之爲物理模型
    主要依據
    1. Sharding(分片數量)
    2. 索引Mapping
       存儲的字段配置與關係處理
       （類似開關要存0/1,true/false,on/off）
>看過許多工程師把正規化整天掛在嘴邊   
結果設計出來的表 超。難。用  
如果寫菠菜（博彩）的話就類似於，把一起樂透開獎號碼拆成了七筆資料    
跟你說要用期號拉出來後在依照辣個欄位排序  
然後跟你說正規化做的很好，欄位原子性有做好....  
碼農都知道，你田地沒規劃好，種起來還要跟在地雷區種菜一樣

![](https://CY810912.github.io/th12img/sqlformat.png)


2. 釐清資料相依性與實際需求後依據專業知識挑選存儲類型
   基本依據四個步驟慢慢來
   1. 字段類型（到了文章中段終於要填坑了）
   ```
    字段類型  text v.s keyword
        text 用在像整首詩這樣，es會進行分詞，但不支持聚合分析與排序
        keyword 用在id,enum,性別,郵政編號等等關鍵字，不需分詞，但會拿來排序與數據分析等等
    字段類型 數據化結構
        資料類型能小則小，可以存byte就不要花俏存到long
        程式中開立的constant（enum）等常數項可以直接設置爲keyword
   ```
   2. 檢索設定
       如果不需要檢索資料"手動"設定稱false
       以便日後維護模板可以清楚明白
   3. 聚合與排序
       基本同上，但ES就是吃檢索排序與聚合吃很兇，一樣也是不用的就手動設定爲false
       避免過多專業知識，讓萌新維護起來整個蒙心
   4. 檢查其他資料的存儲設定
       有時候可能需要把store設定爲true保存原本字段內容（像圖片連結）
       還有節約硬體指標型數據是否需要考慮資料的壓縮比  

---
## 下面這段就是關於mapping的設定巨難，看官請依需求觀看，下篇再介紹mapping的類型

## 假設我們放入一本書的資料
```js
PUT books/_doc/1
{
  "title":"Mastering ElasticSearch 5.0",
  "description":"Master the searching, indexing, and aggregation features in ElasticSearch Improve users’ ",
  "author":"Bharvi Dixit",
  "public_date":"2017",
  "cover_url":"https://images-na.ssl-images-amazon.com/images/I/51OeaMFxcML.jpg"
}
```
##然後將字段類型做效能調整
```js
PUT books
{
      "mappings" : {
      "properties" : {
        "author" : {"type" : "keyword"},//可檢索作者
        "cover_url" : {"type" : "keyword","index": false},//不檢索圖片路徑
        "description" : {"type" : "text"},//描述需做分詞
        "public_date" : {"type" : "date"},//型別爲日期方便查詢
        "title" : {
          "type" : "text",//書名需分詞，且將屬性賦予keyword特性加速檢索
          "fields" : {
            "keyword" : {
              "type" : "keyword",
              "ignore_above" : 100
            }
          }
        }
      }
    }
}
```

那時候在學 ES的數據建模真是煞廢苦心，以下是參考資料來源  
需購買課程，有餘裕的銅學，可以拿出魔法小卡  

[極客時間：elasticsearch教程](https://time.geekbang.org/course/detail/100030501-102655)

這篇花了巨多時間又爆字，但又拆不了，要回去耕田拉  
不然會被逐出農地的  
---
## 對於文章中的任何用詞與專業字或都可以在下面留言提問 
###### ~~梗圖看不懂也行~~
# 蔽農會帶着謙卑爲懷的心情竭盡所能回覆與說明的