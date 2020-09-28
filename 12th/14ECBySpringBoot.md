# 再不耕田受不了啦！！從這篇開始造飛機，讓農夫們都能體驗天上飛的感覺吧！！(3)spring boot & Elastic on Cloud : Post new data

---
以爲程式都寫完了，應該進度超快  
結果這邊不滿意，那邊不高興   
這裏套件版本升一下，那邊寫法改一下  
結果居然整個進度大落後！！！
窩的老天！！！

那麼就手起刀落開始今天的進度吧！！
# 來用spring boot推筆資料到 elastic on cloud!!  


沒錯，本次直接懶到一個極致，只給restful協定，其他就都不給了
更加體會何爲restful搜尋方式

## 本篇重點~Post

## 首先階層式架構起手式-Controller
```java
//restful 必備annotation BJ4
@RestController
public class UserInfoController {
    //引入Service 請愛用Resource 依據官方寫法寫Autowired會被同事討厭的
    @Resource
    private UserInfoService userInfoService;

    //Post request method
    @PostMapping
    public ResponseEntity createProfile(@RequestBody UserInfo userInfo) throws Exception {
        //隨便傳個創建吧
        return new ResponseEntity(userInfoService.createDoc(userInfo), HttpStatus.CREATED);
    }
}

```

## 再來階層式邏輯亂鬥區-Service
這裏我們簡單塞個UUID給他就好了，並且注入DAO
### (這裏就不分BO，VO,Entity)了，佔起來心都虛 
```java
@Service
public class UserInfoService {
    @Resource
    private UserInfoDAO userInfoDAO;

    public String createDoc(UserInfo userInfo) throws IOException {
        //除非人品異常爆發，不然不會重複的ID生成
        UUID uuid = UUID.randomUUID();
        //完整填充Bean後放入資料庫
        userInfo.setId(uuid.toString());
        return userInfoDAO.createDoc(userInfo).getResult().name();
    }

}
```
## 最後重頭戲來了把資料寫進去-DAO  
```java
//這裡記得用static Enum還要點來點去的，有點麻煩
import static com.ithelp.ironman.ecdemo.constant.EsConstant.INDEX;
import static com.ithelp.ironman.ecdemo.constant.EsConstant.TYPE;

@Repository
public class UserInfoDAO {
    @Resource
    private RestHighLevelClient client;
    @Resource
    private ObjectMapper objectMapper;

    public IndexResponse createDoc(UserInfo userInfo) throws IOException {
        IndexRequest indexRequest = new IndexRequest(INDEX,TYPE);
        indexRequest.id(userInfo.getId());
        indexRequest.source(convertProfileDocumentToMap(userInfo));
        return client.index(indexRequest, RequestOptions.DEFAULT);
    }

    private Map<String, Object> convertProfileDocumentToMap(UserInfo userInfo) {
        return objectMapper.convertValue(userInfo, Map.class);
    }
}
```

來檢查下 yml 是否正確，先從本機測試

```yml
elasticsearch:
  host: localhost
  port: 9200
  username:
  password:


#elasticsearch:
#  host: 3586ff6792224f91ad8d0d99ef387c44.asia-east1.gcp.elastic-cloud.com
#  port: 9243
#  username: elastic
#  password: vOtc8JDpeqxhiGQrfk2NZQJI

server:
  port: 8081
```

最後一樣的Do Re Mi Sol~
直接post打一打吧

## 優秀的200 正常沒噴錯的回來了
![](https://CY810912.github.io/th12img/springboot/projectPost1.jpg)   

## 如果只是本地開發，那麼今天就到這裏結束就可以啦

# 但事情總是沒有這麼簡單，本機都完成了，當然是迫不及待打打看on cloud，改個配置重啓下
## 以下是真實位置，自由取用沒關係，反正也不過剩下20天( º﹃º )
```yml
# elasticsearch:
#   host: localhost
#   port: 9200
#   username:
#   password:


elasticsearch:
 host: 3586ff6792224f91ad8d0d99ef387c44.asia-east1.gcp.elastic-cloud.com
 port: 9243
 username: elastic
 password: vOtc8JDpeqxhiGQrfk2NZQJI

server:
  port: 8081
```

![](https://CY810912.github.io/th12img/springboot/projectPostErr2.jpg)   

# 欸？500錯？怎麼可能？
##  沒事沒事，要改的東西太多了，改天吧

反正坑都踩過填過了，小事麻

來回去加班囉~

---
## 對於文章中的任何用詞與專業字或都可以在下面留言提問 
###### ~~梗圖看不懂也行~~
# 蔽農會帶着謙卑爲懷的心情竭盡所能回覆與說明的
