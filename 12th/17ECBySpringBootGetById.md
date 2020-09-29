# 再不耕田受不了啦！！從這篇開始造飛機，讓農夫們都能體驗天上飛的感覺吧！！(5)spring boot & Elastic on Cloud :get data by ID

---  

restful有趣的地方就是同樣的url  
因爲各種的http method而執行各種不同的東西  
對於我這種postMan每次都亂打亂拷的人都會不小心打錯呢

然後摳了個老半天，奇怪？爲何多了堆垃圾資料，原來是方法沒改呀~

好咧，今天也是直接開始吧~

## Restful必用，用ID取得單一文檔=>controller
```java
//Restful 時常使用的方法，相關url後直接帶id取得該資料
@GetMapping("/{id}")
public UserInfo findById(@PathVariable String id) throws Exception {
    return userInfoService.findById(id);
}
```
## servicer層我們一樣簡單寫,架構規範很重要，並不能因爲看起來沒用而省略
##### ~~會被罵爆~~~
```java
public UserInfo findById(String id) throws IOException {
        return userInfoDAO.findById(id);
    }
```
## 老樣子，主角登場DAO的說文解字~
```java
public UserInfo findById(String id) throws IOException {
    //由此可知爲何官方推薦RestHighLevelClient
    //因爲使用起來相當直覺，幾乎像是一個主服務呼叫另一個主服務

    //要求去INDEX使用get方法取回id爲...
    GetRequest getRequest = new GetRequest(INDEX, id);
    //對ES client請求取得回傳
    GetResponse getResponse = client.get(getRequest, RequestOptions.DEFAULT);
    Map<String, Object> resultMap = getResponse.getSource();
    //實體轉換
    return convertMapToUserInfo(resultMap);
}

//Entity轉換方法
private UserInfo convertMapToUserInfo(Map<String, Object> map) {
    return objectMapper.convertValue(map, UserInfo.class);
}
```

非常標準的依據get/{id}取得實體

來稍微準備下明天依據屬性的文章，等等還要去跟pm過需求
感覺快被pm殺死了呢~

---
## 對於文章中的任何用詞與專業字或都可以在下面留言提問 
###### ~~梗圖看不懂也行~~
# 蔽農會帶着謙卑爲懷的心情竭盡所能回覆與說明的
