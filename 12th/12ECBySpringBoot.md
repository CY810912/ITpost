# 再不耕田受不了啦！！從這篇開始造飛機，讓農夫們都能體驗天上飛的感覺吧！！(1)spring boot connect to elastic on Cloud!!(new Porject)

---

如題跟朋友聊天炫耀有來寫這個主題  
爲了那個elastic 聯名款保溫瓶，他看了看你不是碼農？怎麽都沒扣？
![](https://CY810912.github.io/th12img/springboot/lineChat.png)


## 欸?聽到這個就覺得好像不把專長跟主題合在一起，廣大碼農們哪知道怎麽連到on cloud?  

## 於是乎stackoverflow.com敲下spring boot connect to elastic on cloud...  
## 然後去git找找，去google找找，能找的地方都去找，公司寫的集成太大，職業道德也隱隱作祟

![](https://CY810912.github.io/th12img/springboot/holyShit.png)

# 好吧，只能自己刻了，接下來未知篇數的獨創，let's start!!

##### ~~要命喔我的天....都沒時間寫文章了，加班爆表，然後還要自己刻輪子~~

從開專案開始吧
## 1. 基本上這篇還是主要是講ES所以spring boot的部分就是蜻蜓點水
![](https://CY810912.github.io/th12img/springboot/projectStart.png)

> 直接intellij spring boot，然而對這神好用的東西不熟的銅學  
任意門幫你們放這了  
[spring boot30天手把手必收文章](https://ithelp.ithome.com.tw/users/20107812/ironman/1538)

## 2. 用上現在市場主流的java8 lambda是好物
![](https://CY810912.github.io/th12img/springboot/projectStart2.png)

> 對lambda不熟的銅學，[這裏也有邊好文收着](https://zhuanlan.zhihu.com/p/145734155) 

## 3. 最後來引入常用 maven 這次只要引入lombok就好了 ~~(get/set真是太煩了)~~
![](https://CY810912.github.io/th12img/springboot/projectStart3.png)

## 4. 然後直接下一步到專案創建完成DO RE MI SO~
![](https://CY810912.github.io/th12img/springboot/projectStart4.png)

> 草率到不行的專案就此完成了呢

##  這個專案會同步到這裏，爲了不劇透，開了個新的重頭來~
反正最後一定動的起來XDD

最後附上 pom.xml檔，專案啓起來應該是要長這樣的留意版本號囉
``` xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.0.4.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>

    <groupId>com.ithelp.ironman</groupId>
    <artifactId>ec-demo</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>ec-demo</name>
    <description>connect to elastic by jacky</description>
    <packaging>jar</packaging>


    <properties>
        <java.version>1.8</java.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
        </dependency>
       
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

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

</project>

```

---
## 對於文章中的任何用詞與專業字或都可以在下面留言提問 
###### ~~梗圖看不懂也行~~
# 蔽農會帶着謙卑爲懷的心情竭盡所能回覆與說明的
