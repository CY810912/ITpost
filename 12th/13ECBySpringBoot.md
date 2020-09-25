# 再不耕田受不了啦！！從這篇開始造飛機，讓農夫們都能體驗天上飛的感覺吧！！(2)spring boot connect Elastic on Cloud  開始找農具與整地啦

欸，是的開篇就要調整上篇的pom檔
身爲碼農就是要不斷推翻自己呢  
```xml
<!--首先先把上篇的spring boot升級到2.3.4可以少引用幾個包-->
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.3.4.RELEASE</version>
    <relativePath/> <!-- lookup parent from repository -->
</parent>

<!--記得更新上編碼，不然送出會杯具-->
<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
    <java.version>1.8</java.version>
</properties>

<!--最後檢查下依賴是否全都有，也要注意版本號喔-->
<dependencies>
    <!--必引，沒引用這個就什麼都不用玩了-->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <!--必引，沒引用這個也什麼都不用玩了，我們的主角-->
    <dependency>
        <groupId>org.elasticsearch</groupId>
        <artifactId>elasticsearch</artifactId>
        <version>LATEST</version>
    </dependency>
    <!--懶人必備，可以省去大量的get/set-->
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
    </dependency>
    <!--json 的常用包很多，這邊隨便拿這個擋一下-->
    <dependency>
        <groupId>com.google.code.gson</groupId>
        <artifactId>gson</artifactId>
        <version>2.8.5</version>
    </dependency>
    <!--這次的高可用客戶端，配置必須，因爲這個卡超久-->
    <dependency>
        <groupId>org.elasticsearch.client</groupId>
        <artifactId>elasticsearch-rest-high-level-client</artifactId>
        <version>LATEST</version>
    </dependency>
    <!--sping boot 升級後會自動包裝，如果不拿掉，tomcat會起不來，巨坑-->
    <!-- <dependency>
        <groupId>com.fasterxml.jackson.core</groupId>
        <artifactId>jackson-databind</artifactId>
        <version>2.9.6</version>
    </dependency> -->

    <!--測試用，留着無妨，不然還要寫掠過測試，麻煩哪-->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <scope>test</scope>
    </dependency>
</dependencies>
```
# 接下來是ElasticsearchConfig 與 yml
```yml

#本機版
elasticsearch:
  host: localhost
  port: 9200
  username:
  password:

#oncloude 線上版 至關重要，現在的程式碼直接切這個是跑不起來的
#elasticsearch:
#  host: 2233586ff6792224f91ad8d0d99ef387c44.asia-east1.gcp.elastic-cloud.com
#  port: 9243
#  username: elastic
#  password: 223vOtc8JDpeqxhiGQrfk2NZQJI

```
# 一級大標：拷貼黨銅學，下面 ↓這裏作記號，晚點這裏有坑 
```java
@Configuration
public class ElasticsearchConfig {
    @Value("${elasticsearch.host}")
    private String host;
    @Value("${elasticsearch.port}")
    private int port;
    @Value("${elasticsearch.username}")
    private String userName;
    @Value("${elasticsearch.password}")
    private String password;

    //這東西記得給，不然用著用著就壞了
    @Bean(destroyMethod = "close")
    public RestHighLevelClient restClient() {
        //基本認證類
        final CredentialsProvider credentialsProvider = new BasicCredentialsProvider();
        credentialsProvider.setCredentials(AuthScope.ANY,
                new UsernamePasswordCredentials(userName, password));

        //Elastic連線建立類
        RestClientBuilder builder = RestClient.builder(new HttpHost(host, port,null))
                .setHttpClientConfigCallback(httpClientBuilder -> httpClientBuilder.setDefaultCredentialsProvider(credentialsProvider));
        RestHighLevelClient client = new RestHighLevelClient(builder);
        return client;

    }
}
```
[鐵人賽連接參考資料](https://www.elastic.co/guide/en/elasticsearch/client/java-rest/current/java-rest-low-usage-initialization.html)

我的老天，連到官網看原文東西要怎麽用    
我一直以爲向敝農這樣的~~低端~~碼農是都不會遇到
## 鐵人賽真是淨化各種妖魔鬼怪    

就跟古早以前連接MySql還沒被封裝前，大家都是網路上拷來拷去    
然後再各憑實力蹦出新花樣....現在要我在寫可能又得去找找拷貼哪裏了...  

# 這種時候不找出我們的肌肉柴柴，渾身不舒服  

![](https://CY810912.github.io/th12img/springboot/frameWork.jpg)   

最後再來定義個廢廢的動態資料模板結束今天吧~

```java
@Data
public class Profile {
    private String id;
    private String name;
    private String six;
    private String phone;
}

```

不知道爲何原本以爲寫程式  
就是把已知東西拷貼拷貼拷貼應該就搞定了

但最近進度卻嚴重落後呢
要趕回去加班啦啦啦

---
## 對於文章中的任何用詞與專業字或都可以在下面留言提問 
###### ~~梗圖看不懂也行~~
# 蔽農會帶着謙卑爲懷的心情竭盡所能回覆與說明的