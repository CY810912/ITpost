## 欸，不是，才第五天就跑題對嗎？不是麻，試用那點時間哪夠會ES?今晚想來點什麼至少要知道怎麼點麻
---

昨天好不容易把on cloud 開了個頭，結果今天又被打回去  
沒辦法14天除非你不斷生出email，或魔法小卡魔力充足，不然根本不夠學

![](https://CY810912.github.io/th12img/localstart.png)

那麼今天就直接一口氣把本機裝完然後回到天上 ~~（免得我的保溫瓶不見！！）~~
因爲可能被判離題同場加映elastic 不專業對應 MySql 

# 基本ES是需要安裝java的因此直接建議開發者要裝的話直接安裝7.X.X以上
**因爲裏面內置了java環境，省事**  

[ES簡中官方下載任意門](https://www.elastic.co/cn/downloads/)

這時候有強迫症的看官可能說：這位農夫怎麽不給個入口，直接就下載頁了呢？  
1. 想節省篇幅，同場加映很麻煩
2. 還是時不時想提一下我們在on cloud主題

啥意ㄙ....閉嘴！沒比較沒傷害
![](https://CY810912.github.io/th12img/download.png)

看官們可以看到畫面上有四個
# **_在Elastic cloud上啓用_**
沒錯呢，用on cloud可以一次在電腦裏少裝四個東西
解除安裝啥的也都不用管了

點選下載後會有步驟教學與影片可以學安裝
這邊就不冗言贅字了基本如果想要運行的跟on cloud一樣  
四個都要裝跑不掉（不然最至少裝kibana,跟Elasticsearch **~~(什麼廢話？！)~~**）

當然es也有提供command line 或brew但對於linux與mac資料夾結構會被打亂
所以在學習上不建議

![](https://CY810912.github.io/th12img/guistart.png)


這邊超簡單講解下下載這四個功能，之後會更加詳細說明  
1. Elasticsearch:是說....你不知道這個你怎麽會在這？
2. kibana:類似MySql之於workbench 或 navicat就是個資料庫的可視化工具  
   讓您少敲點command line助於您的身心
3. APM:進行程序性能監控套件，包含效能圖繪製（請求回應時間，未處理錯誤及異常等等)
4. elasitc enterprice Search:顧名思議，要錢的，可以觀測資料，進行app search等等

其中還有本機建議下載的
1. Logstash:ELK 的L能夠採集資料與轉換資料例如csv,json等等

---

# 最近看到一堆人有同場加映我也來加映

# 極 ~~不~~ 專業 用RDB理解Elasticsearch

RDB | Elasticsearch| 說明
---------|----------|----------
 Table | index(type) | Es並沒有好幾張表變成一個庫的概念，我們直接稱之爲index爲一張表 
 Row | Document | 我們說一張表中的一筆資料，在ES中就是一個document 
 Column | Field | 這筆資料中有很多屬性，例如用戶密碼，在搜尋結果類似json的key
 Schema | Mapping | 資料屬性，字串，數組或布林等等，7.0開始智能辨識，也可動態指定
 SQL | DSL | 我們所說的指令語法DSL不是關鍵字，而是形容ES是領域的特定語言  


##### ~~表格真是超難用的....真是佩服有人怎麽用的這麼漂亮，下次用截圖的好了~~

---  

好咧，碼農要回去耕田了，不然沒飯吃，會餓死  
明天讓我回天上繼續看要怎麼用kibana

---

## 對於文章中的任何用詞與專業字或都可以在下面留言提問 
###### ~~梗圖看不懂也行~~
# 蔽農會帶着謙卑爲懷的心情竭盡所能回覆與說明的