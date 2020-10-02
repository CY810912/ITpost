# 再不耕田受不了啦！！從這篇開始造飛機，讓農夫們都能體驗天上飛的感覺吧！！(7)spring boot & Elastic on Cloud :update data from field 

---

今年鐵人賽真是特別硬呢  
連續兩個連假，一轉眼庫存直接掃空....  
但~沒事，我超邊緣，可以假日來公司加班順便打個鐵人賽~  
![](https://cy810912.github.io/th12img/springboot/aJoke.png)   

這次鐵人賽也是個多執行序的概念  
在我打這些增刪改查的程式碼同時  
也在研究研究on cloud線上環境究竟可以怎麽可以~~混~~寫完最後十天  

今天寫個修改，明天寫個刪除，就又要回線上處理那些飄渺的東西啦

---
## 上兩篇我們到了可以依據屬性或id把整筆doc拉出來，接下來有了資料就可以改啦

## controller入口，依據Restful用上了put
但其實使用post判斷有沒有傳入id來決定是增還是改也是很多啦....
```java
//將要修改的資料"整筆"傳進來，不傳原本有的欄位會null喔
@PutMapping
public ResponseEntity updateProfile(@RequestBody UserInfo userInfo) throws Exception {
    return new ResponseEntity(userInfoService.updateDoc(userInfo), HttpStatus.CREATED);
}
```
## service業務邏輯層回傳修改結果字串
```java 
public String updateDoc(UserInfo userInfo) throws Exception {
    return userInfoDAO.updateDoc(userInfo);
}
```
## 最後DAO 搜尋用searchRequest,修改當然用updateRequest囉
```java
public String updateDoc(UserInfo userInfo) throws IOException {
    //創建一個請求，代入要對哪個index與哪個id資料做修改
    UpdateRequest updateRequest = new UpdateRequest(INDEX,userInfo.getId());
    //將資料轉換撐map準備送出
    updateRequest.doc(convertUserInfoToMap(userInfo));
    //最後將方法送出，並將結果往返前端
    return client.update(updateRequest, RequestOptions.DEFAULT).getResult().name();
}
```

那麼來看看今天的結果吧~
![](https://cy810912.github.io/th12img/springboot/errorData1.png) 

### 我們發現這筆資料的性別是錯誤的，撇開有些人名字取得中性，但敝農確實是個男的

然後我們將正確資料送出~
![](https://cy810912.github.io/th12img/springboot/errorData2.png) 

最後再截postman的資料實在沒啥意思，我們直接回雲端看看
![](https://cy810912.github.io/th12img/springboot/errorData3.png) 

欸~是的，看到資料正常沒問題
字數也遠遠超過300字，所以今天又可以下班囉~  

果然寫程式真是最快樂的事呢~

##  [專案位置在這裡](https://github.com/CY810912/elastic-on-cloud)~
---
## 對於文章中的任何用詞與專業字或都可以在下面留言提問 
###### ~~梗圖看不懂也行~~
# 蔽農會帶着謙卑爲懷的心情竭盡所能回覆與說明的
