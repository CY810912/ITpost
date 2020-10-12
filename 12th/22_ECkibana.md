## 田也耕了，作物也長起來了，是該來看看長怎樣要怎麼賣了~ Elastic on cloud management tool"s"(2)-kibana create index pattern

---
最近開始有人問我發文的圖都哪來的?
##### ~~可惡...比起這問題，比較希望問這梗圖是啥意思呀(´;ω;`)~~  

基本上所有的梗圖都是想到時自己手工去做的  
[梗圖與他們的產地](http://memes.tw/)  

另外一些圖文作家也都會去詢問對方是否同意~
![](https://CY810912.github.io/th12img/ecTool/itDogAgree.png)  

### [itDog原作者](https://www.facebook.com/itdogcom)

其他程式麻....大部分都是google出來的
看了太多了，有時候就會忘記在哪看...  
但記得的都會附上鏈結，順便幫辛苦錄製課程的老師打廣告  
如果有冒犯還請多見諒，會再補上原地址的~
![](https://CY810912.github.io/th12img/ecTool/author.png)  


---

好咧好咧，那麼來開始今天的閒話家常吧  
#### ~~(剛剛那才是正事，EC沒這麼專業不敢說是討論)~~  

還記得之前有提過一個mapping  
那個真的J8難懂([噩夢傳送門](https://ithelp.ithome.com.tw/articles/10242418))  

但為何前面程式的部分都沒有用到呢?  
關於這件事我們來打個生動的比方好了  
你...雖然常用MySql，但多久沒寫過create table XXX這句指令了呢?  
產表已經很煩了，產型態事比產表更煩人的事情  
這東西當然可以有gui幫做(雖然還是煩)  

## 這篇先簡單做點初學者該做的事create index pattern
kibana的入門篇，很多東西都要依照這個去往下做的

## 先來說說index pattern幹嘛的

>index pattern 顧名思義
index依據某個規則，通通綁在一起
之前說過index類似一張表
那麼index pattern就類似於把這些名字經過通配符對應起來的index
綁成一個庫，方便在搜尋的時後kibana去依據此index pattern做多index搜尋



那麼這麼好用的東西該怎麼創建呢？我們類夠~

## 光是這個入口就有夠難找的,在有時候引入sample data的情況下很難找到這裏
![](https://CY810912.github.io/th12img/ecTool/createIndexPettern.png)  

## 找到本篇主角所在頁面直接點下去~然後create index pattern
![](https://CY810912.github.io/th12img/ecTool/createIndexPettern2.png)  

## 這邊會幫你把所以已建立的index列出來，方便我們去建立希望的index pattern
這裏我們直接用之前在地上傳上來index做示範，接下來什麼都不用管，下一步到不能點就完事了
![](https://CY810912.github.io/th12img/ecTool/createIndexPettern3.png)  


##最後我們會停在這裏
![](https://CY810912.github.io/th12img/ecTool/createIndexPettern4.png)  

在這裏我們可以看到這個index所有的屬性被列出
點選旁邊的鉛筆，可以做些對於此屬性的格式化

但要做"mapping"."type"的修改仍需要下mapping Api指令
所以之前的篇章才並沒有對格式化做很多的說明  
~~（按一按就可以搞定的事，沒空學的話當然先擋着用麻)~~


>看到這裏恭喜，一半的kibana功能已經可以用啦  
光是引入sample Data後不明白爲何引入示範資料kibana有用  
我打半天的資料到處找不到，就花了超多時間的  
打技術文，不只要踩坑，還要說明坑怎麽填，真佛

下篇開始，來到處點點能講啥就講啥吧_(:3 」∠ )_  
~~（沒有拉，已經規劃好了，隨便說說而已）~~

---
## 對於文章中的任何用詞與專業字或都可以在下面留言提問 
###### ~~梗圖看不懂也行~~
# 蔽農會帶着謙卑爲懷的心情竭盡所能回覆與說明的