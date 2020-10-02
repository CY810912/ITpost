# 再耕下去就失格啦~spring boot & Elastic on Cloud (last:8) : Delete Data  

---

總覺得雖然我都連on cloud  
但都是用本機開發，這樣下去鐵定失格  
覺得保溫瓶逐漸透明化中，這篇就將spring boot結束掉吧~  
原本想寫個排程去[臺北資料庫撈東西放上去的](https://data.gov.tw/)  

沒事，輕輕鬆鬆的結束掉這篇吧

要開始準備on Cloud的文章了呢  
連假狀態，用手機用平版發文的，真是要命~

---

## 既然我們都能夠增改然後各種查了，是該來刪除資料了  
![](https://cy810912.github.io/th12img/springboot/delete1.png) 

## 指令請勿亂找亂下，肯定出大事的，請愛惜生命


## 最後一次看到這句話:接口入口controller
```java
@DeleteMapping("/{id}")
public String deleteProfileDocument(@PathVariable String id) throws Exception {
    return userInfoService.deleteUserInfo(id);
}
```
## 跟getById一樣簡單的service業務邏輯層
```java
public String deleteUserInfo(String id) throws IOException {
    return userInfoDAO.deleteDoc(id);
}
```
## 程式最後一篇，結束的簡潔有力，這樣就會有種：喔~我全會的錯覺
```java
public String deleteDoc(String id) throws IOException {
    DeleteRequest deleteRequest = new DeleteRequest(INDEX, id);
    return client.delete(deleteRequest, RequestOptions.DEFAULT).getResult().name();
}
```

刪除....實在沒啥好展示的呀  
就這樣，看官們也能拿這東西去到處招搖撞騙  
有在java裏面連接過elasticsearch的  

只要面試官沒看過，應該,大概,可能沒問題的

那就這樣囉~
明天開始我們沿着這臺紙飛機堆出來的資料回雲端看看kibana能爲我們做些什麼~  

##  [專案位置在這裡](https://github.com/CY810912/elastic-on-cloud)~
---
## 對於文章中的任何用詞與專業字或都可以在下面留言提問 
###### ~~梗圖看不懂也行~~
# 蔽農會帶着謙卑爲懷的心情竭盡所能回覆與說明的

