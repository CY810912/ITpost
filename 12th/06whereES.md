# elastic on cloud  對一個碼農來說果然還是太運維了，好想寫code呀，這東西到底可以幹嘛(1)elastic在項目中拿來幹嘛的

---
> 哎呀哎呀，這個標題真是有夠限制  
> 對一個碼農來說一個畫面上都是配置與log查詢  
> 少了Maker的事情，就覺得"噢噢喔~這實在太大了"(我是說資訊)

每天苦思許久，文章到底要寫啥  
（對，很多都是當天熱騰騰的）   
果然對於給一個工具箱給我，說明每件工具可以拿來幹嘛  
農夫比較喜歡拿了農具，知道農具是幹嘛的，先用出了個東西在說

---
好吧，那就來補全Elastic stack這 **_倉管_** 的相關知識吧

## 恩？爲何說elastic stack是倉管？  


這牽扯到一個很重要的觀念，這個工具在整項專案中扮演這什麼角色

## 先來看看農夫的流程吧

``` java
農夫開闢田地
=>
    開始種田
    (施肥，除蟲，被老婆罵爲何家裡沒錢，幾時才能賣出？)
=>
    準備收割了
    原本都是人工收割，雖然精準，也知道每個作物長怎樣，但實在太慢了
=>
    發現這樣不行
    多找幾個人來，一起收割，一個累倒了，還有其他人繼續活動這
=>
    因爲人手多了產出越來越快，名聲越來越好
    盤商說想要知道生產履歷，方便查詢這個作物幾時生產，生產流程如何
=>
    然後發現還要再多一個人來管理日誌.....
```

## 再來看看碼農的流程吧
``` java
碼農開啓專案
=>
    開始碼程式
    (寫邏輯，除蟲，被PM不斷直問項目如何？幾時產出？
=>
    程式開始動了，有資料進來了
    原本都是sql做增刪改查，每筆資料詳實記錄，但實在太慢了
=>
    發現這樣不行
    讀寫分離，主從庫一個個技術加上
=>
    資料結果與程序日誌各種資訊也開始要被記錄下來
    上頭說，你要給個合理的解釋爲何要添機器，客戶一直該資料太慢
=>
    然後發現還要再多一個人來管理日誌.....
```

---
流程如此相似 ~~(工程師都說自己是農夫還沒人相信)~~  

## 那麼那個管理日誌的人我們就暫且叫他elasticsearch(簡稱ES)吧

但ES如同字面上的名稱，我們常常把它當做一種資料庫  
但他其實更像是
# 搜尋引擎

來簡單畫個流程圖吧

![](https://CY810912.github.io/th12img/process.jpg)


...畫完流程圖覺得今天可以了，極限了  
這個簡單的流程圖畫得不簡單
原本還想打些什麼，還是下一篇吧


---

>果然還是要去學個一套流程圖軟體來用  
公司最近來了個管理轉程式的工程師，覺得充滿景仰    
到底是怎樣的經歷可以讓他從收租的變繳租的XDD

---

## 對於文章中的任何用詞與專業字或都可以在下面留言提問 
###### ~~梗圖看不懂也行~~
# 蔽農會帶着謙卑爲懷的心情竭盡所能回覆與說明的