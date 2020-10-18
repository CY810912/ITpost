# 延長賽:碼農最後的哄擡價格，高級操作：說出一口聚合分析（下）

---
沒想到有生之年也會遇到必須寫延長賽的日子  
太失策了  
但沒寫就有個東西卡在那的感覺  
真是不蘇胡，只好默默打開一年可能打開一個月的git庫更新了  
是的，這種如鯁在喉的感覺兩天就讓人太不舒服了  

趕快把這寫完才能心安理得地去寫結賽心得   

## 那麼今天來最後的Metric指標聚合吧!!   

[今天的參考起點](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-aggregations-metrics.html)

指標聚合就是將指定查詢的資料進行某種運算  
這個某種運算與Excel或一般SQL一樣，給與函式返回結果

那麼這次就先從最日常的一次開始再馬上一次結束吧 

# 1. max:查出聚合的最大值
# 2. min:查出聚合的最小值
# 3. sum:查出聚合的總和值
# 4. avg:查出聚合的平均值  

欸，這樣也太混，所以給個兩顆小栗子  


## 一般查詢聚合分析
```json
POST /it_help/_search?size=0
{
  "query": {
      "filter": {
        "match": { "name": "java" }//僅統計名中帶java
      }
  },
  "aggs": {
    "java_age_sum": { 
            "sum": {
                "field": "age",//統計欄位為年齡
                "miss": 18//若年齡值為空則以18來算
            } 
         }
  }
}
// 類似 sql :select sum(age) where name like %java%; 
```

## 然而聚合分析也支援腳本運算
```json
POST /it_help/_search?size=0
{
  "query": {
    "constant_score": {
      "filter": {
        "term": { "name": "java" }
      }
    }
  },
  "aggs": {
    "java_ageSum": {
      "sum": {
        "script": {
          "source": "doc.age.value"
        }
      }
    }
  }
}
```
# 5. Value Count :value計數，通常跟平均一起顯示，計算目標值計數，重複值也會算+1 

```json
POST /it_help/_search?size=0
{
  "aggs" : {
    "name_count" : { "value_count" : { "field" : "name" } }
  }
}
//類似 select count(*) from it_help where name <> null
```

##### 但這些東西往往都要一起用，要不摻在一起做~~撒~~ ....
# 6.stats:統計聚合
```json
//直接貼個官網的
POST /exams /_search ? size = 0 {
  "aggs": {
    "grades_stats": {
      "stats": {
        "field": "grade"
      }
    }
  }
}
//return
{ ...
  "aggregations": {
    "grades_stats": {
      "count": 2,
      "min": 50.0,
      "max": 100.0,
      "avg": 75.0,
      "sum": 150.0
    }
  }
}
```

# 7. Weighted Avg:加權平均數  
##  關鍵公式: ∑(value * weight) / ∑(weight)
國中時高階書生最愛比的分數，因為都太高了，硬要分高下  
像我們這種低端伴讀都比單科分數的XDD
用起來略抽象，直接上官網範例來解釋

```json
POST /exams/_doc?refresh //放入一筆資料
{
  "grade": [1, 2, 3],
  "weight": 2
}


POST /exams/_search
{
  "size": 0,
  "aggs": {
    "weighted_grade": {//宣告此聚合名稱
      "weighted_avg": {//指定平均數
        "value": {
          "field": "grade"
        },
        "weight": {//指定加權平均權重
          "field": "weight"
        }
      }
    }
  }
}

//返回
{
  ...
  "aggregations": {
    "weighted_grade": {
      "value": 2.0 //=((1*2) + (2*2) + (3*2)) / (2+2+2) == 2
    }
  }
}
```

# 8. cardinality:基數聚合:不計算重複值加總
```json 
//計算資料庫中有幾種性別
GET /it_help/_search {
  "size": 0,
  "aggs": {
    "distinct_sex": {
      "cardinality": {
        "field": "sex"
      }
    }
  }
}
//返回
"aggregations": {
  "distinct_sex": {
    "value": 2 //理所當然的吧，就是生理男跟生理女而已
  }
}

```

# 9. boxplot:箱型圖(盒鬚圖)聚合
基礎統計中我少數聽得懂的幾章 [點我去略懂](http://zh.wikipedia.org/wiki/%E7%AE%B1%E5%BD%A2%E5%9C%96)  
標示出資料庫中該欄位25% 50% 75% 及最大最小值

```json
GET it_help/_search
{
  "size": 0,
  "aggs": {
    "age_boxplot": {
      "boxplot": {
        "field": "age" 
      }
    }
  }
}

//返回
{
 "aggregations": {
    "load_time_boxplot": {
      "min": 18.0,
      "max": 36.0,
      "q1": 22.0,//下四分位數
      "q2": 26.0,//中位數
      "q3": 30.0//上四位數
    }
  }
}
```
#  10.percentile:百分位數指標
...這還真是不知道該怎麼解釋，就是盒鬚圖的詳細版 

[維基百科](https://zh.wikipedia.org/wiki/%E7%99%BE%E5%88%86%E4%BD%8D%E6%95%B0)拷貝一波，來長長知識
百分位數，統計學術語，如果將一組數據從小到大排序，並計算相應的累計百分位，則某一百分位所對應數據的值就稱為這一百分位的百分位數，以Pk表示第k百分位數。

```json
//用法與箱型圖極為相似，因為資料不夠就直接拷官網的拉
GET latency/_search
{
  "size": 0,
  "aggs": {
    "load_time_outlier": {
      "percentiles": {
        "field": "load_time" 
      }
    }
  }
}
//返回
{
 "aggregations": {
    "load_time_outlier": {
      "values": {
        "1.0": 5.0,
        "5.0": 25.0,
        "25.0": 165.0,
        "50.0": 445.0,
        "75.0": 725.0,
        "95.0": 945.0,
        "99.0": 985.0
      }
    }
  }
}
```

---

好咧，以上就是敝農比較常用到的指標聚合  
面試基本也只看到過這些  
(通常是很隨便的請列舉五個)  
但前五個算一個，所以再補五個，真是算夠意思了  
明天終於可以放心地打賽後心得了....

加班就算了，鐵人還延長，這啥勞碌命XDD

---
## 對於文章中的任何用詞與專業字或都可以在下面
###### ~~梗圖看不懂也行~~
# 蔽農會帶着謙卑爲懷的心情竭盡所能回覆與說明的