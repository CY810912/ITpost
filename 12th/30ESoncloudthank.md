# 碼農最後的哄擡價格，高級操作：說出一口聚合分析（上？！）

---  
![](https://CY810912.github.io/th12img/ecTool/lastdog.png)  
[dog.com來源](https://www.facebook.com/itdogcom/photos/a.1006163632885366/1746657518835970/)  

這就是現在目前這個業界奇怪的地方  
面試時手撕tomcat，要你噴出一堆莫名奇妙的api細節  
說出不只沒說過，甚至沒碼過的東西  

然後進來後做的只有：這個按鈕調小點 (´;ω;`)  

[參考文檔](https://www.elastic.co/guide/en/elasticsearch/reference/7.9/search-aggregations.html)

# * 聚合分類的四種類型 
    1. Bucket(桶聚合):按照語法將文檔分配到不同的"桶"，實現資料分類，最主要的聚合分類
    2. Metric(指標聚合):對搜尋出來的資料進行某個欄位或某些統計，類似sql的count,sum,min...
    3. Matrix(矩陣聚合):這東西官方直接說可能會被廢棄，就不看啦
    4. Pipeline(管道聚合):類似lambda將一系列的操作或聚合進行一條線路的變更與分析...開發常用但打完bucket我可能直接覺得之後心情好再補了...

那麼今天打多少是多少，我們直接開始囉~~  
# 1. Bucket Aggregations [參考文檔](https://www.elastic.co/guide/en/elasticsearch/reference/7.9/search-aggregations-bucket.html) 
是的，參考文檔點開後旁邊有超多的分析法，沒一種都學沒什麼意義大概瞭解每種分析功能跟注意事項即可  
（英文文檔真是讀死我了)
特別挑了十個面試可能會被問得出來寫寫

# 桶集合基本使用方式
```js
GET /it_help/_search
{
  "size": 0,
  "aggs": {//宣告使用聚合分析
    "name": {//分析出後的欄位名稱
      "terms": {//分析方式
        "field": "name" //<=分析參數，terms類似於mysql group by因此選則需要某個欄位進行羣組
      }
    }
  }
}
```


1.	Auto-interval Date Histogram  
    > 類似Date histogram，但他可以直接指定Bucket將符合的數值返回
        參數:buckets=>返回小於等於該設定數結果
            format=>返回的日期格式
            minimum_interval=>最小間格時間單位
            missing=>如果文檔沒有該則使用該值
            time_zone=> 時區
            inteval=>（返回參數）最小依據buckets算出的間隔值
2.	Date histogram 
    > 依據日期或日期範圍，按間格將資料分類
        參數：min_doc_count=>最少文檔技術，少於此數不返回
            extended_bounds=>{min:0,max:100}返回範圍內的資料筆數
            missing=>如果文檔沒有該則使用該值
            format=>返回的日期格式
            time_zone=>時區
            offset=>偏移值
            calendar_interval=>日曆間隔
            fixed_interval=>指定固定間格時間單位 ex:2w
3.	Date Range   
    > 針對date類型字段做範圍聚合 
        參數:to=>起點
            from=>終點
4.	Filter   
    > filter結果"再"進行聚合
```json
POST /it_help/_search?size=0
{
    "aggs" : {
        "name" : {
            "filter" : { "term": { "type": "java" } },
            "aggs" : {
                "avg_age" : { "avg" : { "field" : "age" } }
            }
        }
    }
}
``` 
5.	Filters  
    >同上，多重過濾聚合，直接給範例
```json

GET it_help/_search
{
  "size": 0,
  "aggs": {
    "name": {
      "filters": {
        "filters": {
          "java": {
            "match": {
              "body": "java"
            }
          },
          "sex": {
            "match": {
              "body": "woman"
            }
          }
        }
      }
    }
  }
}
```
6.	Histogram:
    >依據數值或數值類型範圍按固定間隔將資料分類  
        參數:interval=>間隔值
            min_doc_count=>最少文檔技術，少於此數不返回
            extended_bounds=>{min:0,max:100}返回範圍內的資料筆數
            missing=>如果文檔沒有該則使用該值
            keyed=>變更返回格式，將此key返回
7.	Range 
    > 依據語法自定義一組範圍,每個範圍代表一個Bucket 直接看範例，這樣會有三個桶
        參數:to=>起點
            from=>終點
```json
GET /product/_search
{
  "aggs": {
    "price_ranges": {
      "range": {
        "field": "price",
        "ranges": [
          {
            "to": 100
          },
          {
            "from": 100,
            "to": 200
          },
          {
            "from": 200
          }
        ]
      }
    }
  }
}
```
8.	Rare Terms :
    >依據某個字段，將資料進行羣組分類(依據結果數升序排列),如同sql=>group by 
        參數：max_doc_count=>最多統計上限，最大值100
             precision=>精度值設定預設0.01
             include=>集合必須包含的術語
             exclude=>集合必須排除的術語
             missing=>如果文檔沒有該則使用該值
10.	Terms : 依據某個字段，將資料進行羣組分類(依據結果數降序排列),如同sql=>group by  

---

我的老天，寫到這已經精疲力竭了  
覺得寫30天仍舊不夠....明天再來寫寫Metric聚合分析好了...  
不然感覺根本沒完成
果然工程師這職業不好幹呀，光是打出來的東西就常常跟自己過不去了

