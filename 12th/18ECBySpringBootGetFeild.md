# 再不耕田受不了啦！！從這篇開始造飛機，讓農夫們都能體驗天上飛的感覺吧！！(5)spring boot & Elastic on Cloud :get data from field 

---  

上上篇我們把整個index給拉了出來  
應該是可以通過指定屬性名稱通通轉成Map然後再從Map做查詢...
然後再返回成List，多寫幾個高級bug，無用迴圈...
然後老闆嫌實在太慢後，睡個午覺改成正常點的code

KPI就上升了呢~
![](https://cy810912.github.io/th12img/springboot/crazyDog.jpg)  
[點我看原文~BY DOG COM](https://www.facebook.com/itdogcom/photos/a.1006163632885366/1718520864982969/)

會打這個黑暗招，我可能也加班加到不正常了呢
快年底了，案子不斷湧進來的，我是誰我在哪我在幹嘛?

##### ~~SBS開始囉~~
# 今天來看看依據field搜索EC上的資料吧~

## 一樣來看看controller吧
```java
//沒錯，直接懶到爆，用屬性當url，查詢值也當url,反正沒資料最多也就回傳空陣列
@GetMapping(value = "/search/{field}/{value}")
public List<UserInfo> searchByField(@PathVariable String field, @PathVariable String value) throws Exception {
    return userInfoService.findUserInfoByField(field,value);
}
```
## 一樣簡陋的service，大部分的ES操作封裝再DAO，就沒事了呢
```java
public List<UserInfo> findUserInfoByField(String field, String name) throws IOException {
    return userInfoDAO.findByName(field,name);
}
```

## 最後難度陡然飆升的DAO，來個一行行說文解字吧
```java
public List<UserInfo> findByName(String field, String value) throws IOException {
    //來建立個搜尋請求，並告訴他我們要找ES哪個 INDEX
    SearchRequest searchRequest = new SearchRequest(INDEX);
    //建立ES 的資源搜尋類，搜尋起手式
    SearchSourceBuilder searchSourceBuilder = new SearchSourceBuilder();
    //開始拼接ES的搜尋語句，語句爲field match(等於) value
    MatchQueryBuilder matchQueryBuilder = QueryBuilders
            .matchQuery(field, value);
    //命令搜尋請求資源爲剛建立的搜尋語句
    searchRequest.source(searchSourceBuilder.query(matchQueryBuilder));
    //let search~ 這邊有用上一篇使用的私有通用方法
    return getSearchResult(client.search(searchRequest, RequestOptions.DEFAULT));
}
```

是不是感覺比起直接學kibana還簡單呢~  
##  那肯定是錯覺  

好咧，程式修修調調的
都是爲了更加精簡化，少一行是一行

出來別的地方耕耕田也差不多，該回去地主（公司）的田繼續務農啦

---
## 對於文章中的任何用詞與專業字或都可以在下面留言提問 
###### ~~梗圖看不懂也行~~
# 蔽農會帶着謙卑爲懷的心情竭盡所能回覆與說明的

