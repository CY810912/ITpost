## 這篇來介紹關於Elastic on cloud的農具好了，關於Elastic專有名詞說文解字：基本概念篇(下)含ES資料類型

---

昨天我們說了找到了立足點，那麼該怎麽學精學熟學廣學得有效率呢？  
當然是直接買線上課程，udemy,hahow,極客時間  
其次是買實體書，再來是網路上各種免費資源  
網路上免費資源並不是不好  
像鐵人賽也有相當多很厲害很實用的實作文
但一堂付費課程的出現遠遠比鐵人賽準備的時間多上更多更多  
課程的連貫性，潤稿等等都是免費資源沒法比擬的  

時間就是金錢，買課程只是少數可以用金錢換取時間的方法之一 
人家都說錢花了再賺就有了，可是問題是能賺錢的時間只會越來越少呢~  

---
# 那麼今天的正篇也開始囉噢噢噢~~  

今天來說說ELK,Mapping以及數據結構 ,還有餘力的話來寫寫_score機制  
（這個寫起來巨複雜的簡單寫寫就好)  
 

欸？怎麽好像有些關鍵詞前面已經出現過了  
沒事，那些我會開傳送門的，再打一次太累了  
字典類麻，總是要方便查的（以後我自己還要看呀）  

# 1. ELK
這東西真的是elasic每個選手每系列文章都出現到會懶掉的東西  
覺得看來看去這篇算是簡潔扼要，比起自己的系列文，貼貼別人的也很重要呢  
### [ELK是啥，看看其他大師怎麼說，沒啥圖但簡明扼要](https://ithelp.ithome.com.tw/articles/10237885)

# 2. 倒排索引  
直接傳送門送上[倒排索引這件事](https://ithelp.ithome.com.tw/articles/10241406)

# 3. 資料架構  
直接傳送門送上[ES對應我們所熟溪的mySql什麼是什麼](https://ithelp.ithome.com.tw/articles/10238966)

# 4. Mapping  
講起來可以很長很玄很神奇的東西  
這邊就只簡單說明定義就好了  
es當中有分爲動態映射，與靜態映射
Mapping是拿來定義一個DOC（一筆資料）每個欄位的存儲形態以及如何建立索引過程的"標記"  
會說是標記他幾乎等同於我們寫mysql最痛苦的數據結構....

再開始今天真正的超煩主題前，這裏兩扇傳送門都是喬叔的  
## 寫的不難，只是鉅細靡遺到讓人腦瓜子有點疼的喬叔寫的，耐著性子看收穫非常多  
### [Dynamic Mapping 動態映射](https://ithelp.ithome.com.tw/articles/10238283)  
### [Index Template 索引模板，也稱之爲靜態映射](https://ithelp.ithome.com.tw/articles/10239736)  

# 5. 資料類型 本日僞主題結束，真主題開始  

## 1. String類(字串類)
    1. keyword:不會分詞
    2. text:會分詞，不需定義他的分詞器，ES就會自動使用standard

standard是啥？傳送門開好囉~關於分詞器  
[請看這篇，詳細介紹](https://www.itread01.com/content/1542021982.html)  

## 2. number類(數值數據類型)  
    1. 整數型  
       * long 
       * integer
       * short
       * byte
    2. 浮點型
       * double
       * float
       * half_float
       * scaled_float  
## 3. date(時間類型)（[參考文章在這裏~](https://zhuanlan.zhihu.com/p/34240906)）
    * 這邊幾乎跟mysql一樣只是es可以選擇格式  
        1. yyyy-MM-dd HH:mm:ss
        2. yyyy-MM-dd
        3. epoch_millis
```json
//我是設定方式~
PUT my_index
{
  "mappings": {
    "_doc": {
      "properties": {
        "date": {
          "type":   "date",
          "format": "yyyy-MM-dd HH:mm:ss||yyyy-MM-dd||epoch_millis"
        }
      }
    }
  }
}
```

## 4. 布林值boolean
#### ~~(...如果這個不會，可能現在來看es有點太早...)~~  

## 5. binary(二進制類)（[參考文章~](https://blog.csdn.net/weixin_39723544/article/details/104869533)）  
    * 二進制數據類型，使用較少，不可搜尋，但官網有，所以還是去學去打了

## 6. 區間類型([參考文章~只是官網傳送門](https://www.elastic.co/guide/en/elasticsearch/reference/7.9/range.html))
    * integer_range
    * long_range
    * float_range
    * double_range
    * date_range 

>負責任點，來個說文解字是必須的  
gt: > 大於（greater than）  
lt: < 小於（less than）  
gte: >= 大於或等於（greater than or equal to）  
lte: <= 小於或等於（less than or equal to  ）

## 7. 複雜類型
    1. 陣列類Array
       * es 陣列類可以直接吃下任何類型資料可直接帶空或多值，只是陣列中所有值都必須是相同的數據類型
    2. 物件類Object
       * 這東西其實就是我們上篇ES幫我們產出的資料狀態  
       * object的列表類不允許彼此獨立查詢資料
       * 所以那個or（and）其實失效的，也就是[這裏描述的](https://ithelp.ithome.com.tw/articles/10249540/)
    3. 嵌套類Nested
       * 要確保我們所設定的object 列表類不全部被拉出，要使用此類
       * 這其實應該算是es的高級操作，這裏簡單體驗下那篇的搜尋應該變更爲如此才能是真正的(and) 
```json
GET it_help/doc/_search
{
    "query": {
        "nested": {
            "path": "it_help",
            "query": {
                "bool": {
                    "must": [
                        { "term": { "name": "ap" } },
                        { "term": { "sex": "man" } }
                    ]
                }
            }
        }
    }
}
```

## 8. 特定類型
    1. GEO地理類型
       * Geo-point:給予座標定位
       * Geo-shape:形状給與一個點並規劃圈選範圍
    2. ip
    3. completion 自動補齊類
    4. token_count 
    5. percolate 過濾類
    6. percolator 父子索引類
    7. alias 別名類型

其中最常用的應該就是ip跟GEO-point
這可以拿來檢視哪裏來的日誌最多，如果某個ip過多，可以啓動防衛機制


# [數值類型參考文章](https://www.elastic.co/guide/en/elasticsearch/reference/7.9/mapping-types.html)
---

我的老天...今天居然爆字爆成這樣  
真是想拆成兩篇呀

但剩下三篇跟一篇感想文
真是很不想超出篇幅呀  
這篇算心態上的大突破吧~

---
## 對於文章中的任何用詞與專業字或都可以在下面留言提問 
###### ~~梗圖看不懂也行~~
# 蔽農會帶着謙卑爲懷的心情竭盡所能回覆與說明的