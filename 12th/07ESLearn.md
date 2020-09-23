# 爲何有人說ES很難學？因爲他本身就反人類呀！這東西到底可以幹嘛(1)ES倒排索引這件事

---

> 這小農夫開頭就要戰技術了嗎？
> 哪敢，敝農還在用這個技術耶

今天公司招募人才  
一個人才年薪要價兩百萬  
一個人才年薪只要五十萬  

然而傳產不說，以工程師來說  
薪水這東西很有趣，一般剛畢業有點經驗的新手  
跟在業界"好好做事"十年的老手差異性在哪？  

你給一個十年老手了不起四倍薪水，他絕對能寫出新手超出四倍甚至十倍  
不管是開發速度，硬體效能開銷比，可維護性，重構性，工程師真是性價比很高呢  

## 同理
ES 的硬體效能開銷非常大，最好是SSD+64G RAM 但爲何呢？  
人家redis寫入讀取超快，RAM了不起吃個2~4G就好  
使用場景跟做的事情完全不一樣呀  

今天來寫寫倒排索引這鬼畜東西  

一般我們可以非常輕易的背出靜夜思    
~~（床前明月光那首，這邊就不混字數寫整首了）~~  

## 但是打著打著又怕有"銅學"把點數全拿去程式了，沒點啥國文，詩在最下面


我們人腦習慣性會拿標題與具有意義的值當索引，如同MySql  
靜夜思 -> 整首詩  

然而今天如果要你背出"有月的詩"
那麼你的腦中就會作出詩體like %月%
這種不具索引且超沒效率的查找

~~經過昨天夭壽流程表今天又要寫表格，我的老天~~  

--- 

## 一般人類與MySql

key | value 
---------|----------
 靜夜思 | 整首詩...
 月下獨酌 | 整首詩...
 望廬山瀑布 | 整首詩...

---

## 反人類索引ES 第一階

key | value 
---------|----------
 前 | 靜夜思整首詩....
 月 | 靜夜思整首詩...
 前 | 望廬山瀑布整首詩...
 月 | 月下獨酌整首詩...

然而這樣不只索引爆炸，連value都大量重複
因此
## 反人類索引ES 第二階 將一般index title放入value解決內容爆炸
key | value 
---------|----------
 前 | 靜夜思
 月 | 靜夜思
 前 | 望廬山瀑布
 月 | 月下獨酌

但這樣索引依舊爆炸呀怎麼辦
## 反人類索引ES 第三階 將索引整合壓縮 
key | value 
---------|----------
 前 | 靜夜思,望廬山瀑布
 月 | 靜夜思,月下獨酌

因此我們在搜索詩詞有前的時候輕鬆帶出
靜夜思與望廬山瀑布這兩首詩

好的，倒排索引基本概念大致就是這樣
但這樣遠遠不足ES做的四分之一，讓我們明天繼續反人類吧！！

## 來來來，詩在這裏，背成順口溜，沒事炫一波也是很厲害的
``` java
//靜夜思
床前明月光，
疑是地上霜。
舉頭望明月，
低頭思故鄉。

//月下獨酌
花間一壺酒，
獨酌無相親。
舉杯邀明月，
對影成三人。

//望廬山瀑布
日照香爐生紫煙，
遙看瀑布掛前川。
飛流直下三千尺，
疑是銀河落九天。
```

---

## 對於文章中的任何用詞與專業字或都可以在下面留言提問 
###### ~~梗圖看不懂也行~~
# 蔽農會帶着謙卑爲懷的心情竭盡所能回覆與說明的