## SQL&NOSQ: Mysql vs redis vs mangoDB vs Hbase vs elasticsearch比較(上)SQL資料庫篇 


![](https://CY810912.github.io/th12img/allMysql.png)

除非公司土豪，而且能夠調優資料庫的大神一堆直接Oracle就打天下   
不然資料庫百百種幹嘛都要會都越學？Mysql直接開"卍解"就好啦  
關聯資料庫這麼強，從表到列都能下查詢語句，就是擴表有點煩而已  

資料有可能不見吃ram的redis，寫入超慢的elasticsearch  
沒有事務與關聯有些甚至不能分頁的Nosql，聽起來都很不可靠去學是吃飽著撐？

然而事情是沒這麼美好的，DB跟人類一樣，吃跟吐不可能同時進行，頂多有人很快  
吃需要嚼要吞，吐除非已經在喉嚨(db cache)不然也需要一點時間醞釀  

![](https://CY810912.github.io/th12img/dbcrash.jpg)


[點我看全文](https://www.facebook.com/itdogcom/posts/1283568385144888)

因此因應各種場景下的資料庫就這麼誕生了    
有吞的快的，吞下去到相關養分吸收一氣呵成的，吐的快的，吐的準得....  

# 接下來就是正文了，各種資源道聽途說的介紹DB  

關聯式相較於非關聯式種類與特性沒有差這麼多，因此將非關聯式放到下一篇特別說  
~~(請自行套用老高畫面)~~

## 關聯性資料庫入門首選 MySql(MariaDB)
那就不囉嗦直接列表式條列下關聯型資料庫優缺點

### 優點
1. 容易理解且觀念能夠相通  
    相較於noSql sql列表式甚至直接用影印機印出來非工程師也能明白，即便連接多張表也能清楚畫出連接點( Foreign key )
2. 操作與查詢方便  
    關聯型資料庫有著相似的CRUD(增刪改查)語法，撇開進階的語法選用與效能不說，幾乎學習完一個資料庫大部分資料庫都能略懂
3. 支持事務  
    事務性是啥?老王給老李100元:老王帳戶-100，老李帳戶+100，整個流程不可切割，要嘛成功要嘛失敗(原子性)，給的錢只能是數字(一致性)，不管如何結果都是兩個帳號+-100(隔離性)，只要交易完成資料庫必定有(持久性)
4. 服務穩定  
    數據庫基本不會死掉(那就是災難了)也有警報提示，即使快掛了把CPU都吃光，也不會直接造成服務器當機

看了以上，如果速度不夠加個分庫分表，讀寫分離不是就可以扛下大部分的IO了嗎?
還是有問題的，因此我們來看看缺點

### 缺點
1. 維護困難
    相較於上述第二點，雖然有著方便的查詢功能，但背後卻是大量技術維護，其中水平擴充，主從庫，與表擴展都相較no sql相對麻煩，且背後相關技術複雜
2. 高併發時IO壓力大
    因為支持事務的關係，即便只是對某列作變更，也可能引發整個表被讀入內存，導致IO過高
3. 全文搜索的雞肋
    不具備分詞能力，模糊查找導致索引無法生效，例如:"like %我是碼農%"就找不到"我是苦逼碼農"
4. 調優困難
    雖然入門容易，但調優技術卻相當困難，索引建立不好反而變得暴力查庫，進而使資料庫變慢

![](https://CY810912.github.io/th12img/carzyDB.jpg)

[點我看全文](https://www.facebook.com/itdogcom/posts/1385281961640196)

下一篇準備來寫寫非關聯式資料庫  
寫了遍之後要跟公司解釋為甚麼選這個方案應該方便許多....
~~(不然每次兩杯茶喝完了還是沒人搞懂為何一個項目為何要三四種資料庫)
~~