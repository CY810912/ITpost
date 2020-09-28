# 再不耕田受不了啦！！從這篇開始造飛機，讓農夫們都能體驗天上飛的感覺吧！！(3)spring boot & Elastic on Cloud :let's get && Restful

---

## [Restful API還是不熟的銅學請點我，好文，收好好嗎？](https://progressbar.tw/posts/53)  


好的，開篇就是個引用，朋友問我說Restful是啥？  
我回我前面不是有貼連接了？這懶人居然連找都懶的找...  

這邊簡單說說吧
## Restful 的超懶一句話
1. GET:不干涉資料庫，僅做查詢
2. POST:干涉資料庫檔案，通常傳入"BO" (Business Object)  
3. PUT:有些公司與POST混着用一般擁有id拿去新增或修改一筆資料
4. PATCH:通常拿來更新資料，蠻少公司用的，更少同時出現PUT/PATCH
5. DELETE:刪除，如果看不懂，可能要先去補英文，再來認真學才是真的  

恩恩，既然大家都說增刪改查（CRUD）
那麼就一個個來吧 

今天來把上次建的INDEX（it_help）所有資料拉出來  

## 同樣地階層式架構起手式：Controller
```java
@GetMapping
public List<UserInfo> findAll() throws Exception {
    return userInfoService.findAll();
}
```

## 再來Service:這次是希望讓es的調整與邏輯封裝起來，因此才寫在DAO，其實一般來說是不會這樣寫的
```java
public List<UserInfo> findAll() throws IOException {
    return userInfoDAO.findAll();
}
```

## 最後複雜的DAO<通常這邊會用泛型去寫入index與實體類，但爲了淺顯易懂就直接傳值了>
```java
public List<UserInfo> findAll() throws IOException {
    //建立搜尋請求
    SearchRequest searchRequest = buildSearchRequest(INDEX, TYPE);
    SearchSourceBuilder searchSourceBuilder = new SearchSourceBuilder();
    //es封裝查詢語句
    searchSourceBuilder.query(QueryBuilders.matchAllQuery());
    searchRequest.source(searchSourceBuilder);
    return getSearchResult(client.search(searchRequest, RequestOptions.DEFAULT));
}

/**
 * 將查詢返回勞資要轉變爲實體後回傳
 */
 private List<UserInfo> getSearchResult(SearchResponse response) {
        SearchHit[] searchHit = response.getHits().getHits();
        List<UserInfo> userInfoList = new ArrayList<>();
        for (SearchHit hit : searchHit) {
            userInfoList
                    .add(objectMapper
                            .convertValue(hit
                                    .getSourceAsMap(), UserInfo.class));
        }
        return userInfoList;
    }
```

那麼上次再寫完文章後偷偷塞了幾筆資料來執行看看吧，看起來一摸一樣呢~ ~~（豈不廢話??）~~
![](https://cy810912.github.io/th12img/springboot/getFindAll.png)  


好咧，明天來寫寫怎麽依照name辣個欄位來搜尋記錄吧~  
要準備開始正職務農了

---
## 對於文章中的任何用詞與專業字或都可以在下面留言提問 
###### ~~梗圖看不懂也行~~
# 蔽農會帶着謙卑爲懷的心情竭盡所能回覆與說明的