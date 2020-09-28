# 再不耕田受不了啦！！從這篇開始造飛機，讓農夫們都能體驗天上飛的感覺吧！！(3.5)spring boot & Elastic on Cloud : Post new data FIX

---

繼昨天（對我來說是剛剛）我們連接EC爆了500錯  
這種backend閃都不能閃的問題  

## 從這張圖開始，再回到我們的農地看看發生了啥

![](https://cy810912.github.io/th12img/springboot/projectPostErr2.png)   

## 明顯發現農地裡有蟲，這隻蟲也是這次連接最大問題

![](https://cy810912.github.io/th12img/springboot/projectPostErr.png) 

花了我快兩個多小時直接追到source code裏面看哪裏拼接了http      

結果只是[這坑](https://ithelp.ithome.com.tw/articles/10244141)   
那麼該怎麼改呢？寫程式有趣的地方就再這，往往花了N小時的東西    
只需要N秒鐘其實就可修好    

```java
@Bean(destroyMethod = "close")
    public RestHighLevelClient restClient() {
        final CredentialsProvider credentialsProvider = new BasicCredentialsProvider();
        credentialsProvider.setCredentials(AuthScope.ANY,
                new UsernamePasswordCredentials(userName, password));


        // 就是這裏，有可能是兩個參數也可能是http也可能是Null 改爲"https"
        RestClientBuilder builder = RestClient.builder(new HttpHost(host, port,"https"))
                .setHttpClientConfigCallback(httpClientBuilder -> httpClientBuilder.setDefaultCredentialsProvider(credentialsProvider));
        RestHighLevelClient client = new RestHighLevelClient(builder);
        return client;

    }
```

## 是的，就是多打這天殺的四個字"https"就什麼都正常了

那麼成功畫面就不截圖了，來去雲上看看現在我們寫的東西吧
## 直接劇透:直接到kibana左上選單->Dev Tools->直接小三角形運行
```js
GET it_help/_search
```

![](https://cy810912.github.io/th12img/springboot/projectPostSucc.png)  

資料安分守己的躺在那呢  
爲了證明我沒虎爛，還把網址結了下來  

好的，既然是幕間當然不能長~
恭喜各位可以開始對EC各種騷操作了  
接下來文章就不演示本機示範，直接都連上on Cloud真槍實彈的來吧  


月底真是個加班加爆的節奏，公司最近在寫加密貨幣的東西  
用起來真滴深奧呢

---
## 對於文章中的任何用詞與專業字或都可以在下面留言提問 
###### ~~梗圖看不懂也行~~
# 蔽農會帶着謙卑爲懷的心情竭盡所能回覆與說明的

