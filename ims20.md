各位IT邦的勇者們  
今天又來到了鐵人賽  
以強制開賽來說的第三個  
最難熬的周末假日

身為一介菜鳥碼農  
假日都是瘋狂拿來進修的。･ﾟ･(つд`ﾟ)･ﾟ･

但可惜所參加的是主題挑戰  
原本想說禮拜天就偷懶點，整個拿來閒聊就好了

但不行，怕一個不小心就被BAN掉了

--- 
### 好的，今天周末假日，不耽誤各位大師們太多時間看文章
### 所以文章略短，也超簡單

~~這理由真是冠冕堂皇的真漂亮~~

既然叫前端三本柱  
那麼今天來隨便聊一下沒有甚麼好講的html吧

來看一下vsCode一般正常情況下按下驚嘆號所產生的html文檔
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="description" content="網站的描述。。。">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <meta name="keywords" content="HTML, CSS, XML, XHTML, JavaScript">
    <meta name="author" content="Runoob">
    <link rel="icon" href="/icon.jpg" type="image/x-icon" />
    <link rel="stylesheet" href="css/cssSource.css">
    <script src="js/your-script.js"></script>

    <title>Document</title>
</head>
<body>
    
</body>
</html>
```
* < !DOCTYPE html>
     * 說明:宣告此篇文檔為"HTML5"檔，也方便瀏覽器辨識，
     * 經驗:此句很重要如果沒打這句，可能會在一些奇怪的地方出錯，例如Font Awesome 、ionic....等等等之類的框架icon跑不出來，或著變亂碼等錯誤，除了半天才發現只是僅僅少了宣告這句

* < html lang="en">
    * 說明:宣告這篇文件由lang所寫
    * 經驗:其實小農個人覺得這個有沒有宣告其實沒什麼關係，等等會說明，比較值得一提的是其實所有的html標籤都可以class這個屬性，所以可以給html這個標籤,例如:< html class="light">or< html class="dark">並且用js或jq搭配之前文章所提到的SCSS，來去操作這個頁面目前所設定的主題顏色

* < meta charset="UTF-8">
    * 說明:宣告此篇文檔由UTF-8所寫
    * 經驗:所以上面lang="en"並不特別強制編寫就在這，其實小農很是納悶，既然大家都在utf-8，為何這東西不能乾脆設定成預設呢?

* < meta name="viewport" content="width=device-width, initial-scale=1.0">
    * 說明:宣告這個頁面的寬度是根據設備寬度，初始設備放大1倍
    * 經驗:這句沒加響應式網頁(RWD)真的都無法正常運行，所以如果套用bootstrap網頁卻怪的一蹋糊塗，先來這看看這句是否正常，然後不希望使用者亂用雙手放大縮小畫面，可以將initial-scale=1.0修改成initial-scale=no，但有些瀏覽器可以無視這一條去放大縮小 

~~這樣的話可以鎖定客戶手賤，亂放大縮小然後又跟你說怎麼跑版了?~~

* < meta name="description" content="網站的描述。。。">
    * 說明:主要是方便以後要修改這網頁可以一目瞭然這個頁面再幹嘛，然後據說搜尋引擎可能會參照屬性去做搜尋
    * 經驗:老實說對這句其實沒甚麼使用過，主管也沒特別要求過要打這東西，因為通常檔名沒有命名好就被要求了，但因為自生成有，有時候我還是會用英文隨便打一下(根本也沒有同事看呀)  
    
* < meta http-equiv="X-UA-Compatible" content="ie=edge">
    * 說明:看到關鍵字IE就知道這句是為了兼容萬惡瀏覽器的兼容語句
    * 經驗:萬惡瀏覽器IE沒有這句還真的會掛，許多同事聽到這次專案如果要兼容IE有些還會焦慮症直接發作，各種CSS不兼容，JS的特立獨行，以及ES6語法根本不能跑，但大陸那邊一堆瀏覽器是從IE改的，根本不能不理他

~~神龍呀!!!讓IE消失吧，我怕了~~

* < meta name="keywords" content="google">
    * 說明:關鍵字下的好，流量沒煩惱
    * 經驗:那時還在學校，有專題競賽，無聊如我把健康教育宣導的網站關鍵字放:18X，然後放到github.io上，還真的蒐的到d(`･∀･)b

* < link rel="icon" href="/icon.jpg" type="image/x-icon" />
    * 說明:網頁頁籤上都有一個小小的圖示，來源就是這個
    * 經驗:比CSS還智能，它會自動放大縮小，當然也可以給他CLASS去調整，有時候我會把它當彩蛋來玩，因為根本沒人會注意他

* < meta name="author" content="Runoob">
    * 說明:說明這網站作者是誰
    * 經驗:我只是個碼農，我是誰根本不重要

---

好的，對於菜鳥最困擾的頭部mata的基礎  
其他類似Refresh 、Pragma 、Window-target、
等等等的我們明天再聊吧  
ヽ(∀ﾟ )人(ﾟ∀ﾟ)人( ﾟ∀)人(∀ﾟ )人(ﾟ∀ﾟ)人( ﾟ∀)ﾉ

這系列文章盡量不去寫JS，因為其他人寫的足夠多了  
如果真的寫到了我也會盡量奉上乾貨的｡ﾟ(ﾟ´ω`ﾟ)ﾟ｡