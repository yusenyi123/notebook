# python微信账单cookie存入mysql

```
activate py38

pip install PyMySQL


mitmdump  -p 8083 -s fetch_cookies.py  '~u https://wx.tenpay.com/'
```

```
请求成功一次 {'userroll_encryption':'HLT76qqlMFM5kUJTG3VkGi+zA9+zKECPPcXMCWgx7VmimL3n11L4g7RuR6KCVsteQ1QUKvbvtNjJ0l/TyzDg1xet6jMF6jY90mk9iYyJgOw6ZYU+wnbJp2HiokPW82wU5r/OLL8hab9cBjOHYHJk/A==', 'userroll_pass_ticket': 'hAKG5QplkS/1ozm0jBz7T8uu393pRv2BhJLdSJGPbH8icYIFeRuXa8g7G7Xxpk5Q'}




Addon error: Traceback (most recent call last):
  File "fetch_cookies.py", line 43, in response
    print(data.userroll_encryption)
AttributeError: 'dict' object has no attribute 'userroll_encryption'
```



```
import pymysql
from pymysql.converters import escape_string
def update_db(update_sql):
    """更新"""
    # 建立数据库连接
    db = pymysql.connect(
        host="127.0.0.1",
        port=3306,
        user="root",
        passwd="root",
        db="fruitshop"
    )
    # 通过 cursor() 创建游标对象
    cur = db.cursor()
    try:
        # 使用 execute() 执行sql
        cur.execute(update_sql)
        # 提交事务
        db.commit()
    except Exception as e:
        print("操作出现错误：{}".format(e))
        # 回滚所有更改
        db.rollback()
    finally:
        # 关闭游标
        cur.close()
        # 关闭数据库连接
        db.close()



# 这个地方必须这么写 函数名：response
def response(flow):
    # 通过抓包软包软件获取请求的接口, 获取到cookies后，存入redis
    if 'userroll/userrolllist' in flow.request.url:
        # 数据的解析
        data = {
            'userroll_encryption': flow.response.cookies.get('userroll_encryption')[0],
            'userroll_pass_ticket': flow.response.cookies.get('userroll_pass_ticket')[0]
        }
        # r.set('wechat_cookies', json.dumps(data))
        print('请求成功一次', data)
        print(flow.response.cookies.get('userroll_encryption')[0])
        print(flow.response.cookies.get('userroll_pass_ticket')[0])

        userroll_encryption=escape_string(flow.response.cookies.get('userroll_encryption')[0])
        userroll_pass_ticket=escape_string(flow.response.cookies.get('userroll_pass_ticket')[0])



        update_sql = 'UPDATE netcookie SET ticket =" '+userroll_pass_ticket+'",encryption="'+userroll_encryption+'"   WHERE id = 1'
        update_db(update_sql)




```





## pom依赖

```
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>fastjson</artifactId>
            <version>1.2.6</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>

        <dependency>
            <groupId>com.squareup.okhttp3</groupId>
            <artifactId>okhttp</artifactId>
            <version>3.6.0</version>
        </dependency>
    </dependencies>
```









```
    @RequestMapping("/pay")
    public  void  test() throws IOException {
        OkHttpClient client = new OkHttpClient().newBuilder()
                .build();




        String userroll_pass_ticket="Yz2UG3VnffLBDSEt0LQckrKISUxsosC4RNKamtq14kl92ytS1lsdGxRMzSDA+hVk;";
        String userroll_encryption="BJVEI0OpBRFJrAmyHKbI+4A0DNKg5ApoWfrOyZsa5NvRaumVSYnoNjZAhkdv7UXjRJX0IdG8qxschR0QypVyQEsSS5tX+P6QreRqDdIiBqIqR+FPk6E1AG8W9cVkY74IinZCattBQ2t1XTWDS8wBnw==";
     String cookie="wxp_log_uid=BgAAyPYNfiCOxHXAiGarPhofii7TKVQeiLxVt4M%3D; userroll_version=2; userroll_pass_ticket="+userroll_pass_ticket+" userroll_encryption="+userroll_encryption;
        Request request = new Request.Builder()
                .url("https://wx.tenpay.com/userroll/userrolllist?classify_type=0&count=20&sort_type=1")
                .method("GET", null)
                .addHeader("Connection", "keep-alive")
                .addHeader("User-Agent", "Mozilla/5.0 (Linux; Android 10; ASUS_I001DA Build/QKQ1.190825.002; wv) AppleWebKit/537.36 (KHTML, like Gecko) Version/4.0 Chrome/78.0.3904.62 XWEB/2691 MMWEBSDK/201101 Mobile Safari/537.36 MMWEBID/5561 MicroMessenger/7.0.21.1783(0x27001543) Process/tools WeChat/arm64 Weixin GPVersion/1 NetType/WIFI Language/zh_CN ABI/arm64")
                .addHeader("Accept", "*/*")
                .addHeader("X-Requested-With", "com.tencent.mm")
                .addHeader("Sec-Fetch-Site", "same-origin")
                .addHeader("Sec-Fetch-Mode", "cors")
                .addHeader("Accept-Encoding", "gzip, deflate")
                .addHeader("Accept-Language", "zh-CN,zh;q=0.9,en-US;q=0.8,en;q=0.7")
                .addHeader("Cookie", cookie)
                .build();
        Response response = client.newCall(request).execute();
        System.out.println(response.body().string());

    }
```







```
    
    
    
    //获得账单，手机上监听到账的app使用
    @RequestMapping("/getBill")
    public  ModelAndView  test() throws IOException {
        OkHttpClient client = new OkHttpClient().newBuilder()
                .build();


        Netcookie netcookie = netdao.selectById(1);

        String userroll_pass_ticket="Yz2UG3VnffLBDSEt0LQckrKISUxsosC4RNKamtq14kl92ytS1lsdGxRMzSDA+hVk;";
        String userroll_encryption="BJVEI0OpBRFJrAmyHKbI+4A0DNKg5ApoWfrOyZsa5NvRaumVSYnoNjZAhkdv7UXjRJX0IdG8qxschR0QypVyQEsSS5tX+P6QreRqDdIiBqIqR+FPk6E1AG8W9cVkY74IinZCattBQ2t1XTWDS8wBnw==";



        userroll_pass_ticket=netcookie.getTicket();
        userroll_encryption=netcookie.getEncryption();
        System.out.println(userroll_encryption);
        System.out.println(userroll_pass_ticket);
        String cookie="wxp_log_uid=BgAAyPYNfiCOxHXAiGarPhofii7TKVQeiLxVt4M%3D; userroll_version=2; userroll_pass_ticket="+userroll_pass_ticket+"; userroll_encryption="+userroll_encryption;
        Request request = new Request.Builder()
                .url("https://wx.tenpay.com/userroll/userrolllist?classify_type=0&count=20&sort_type=1")
                .method("GET", null)
                .addHeader("Connection", "keep-alive")
                .addHeader("User-Agent", "Mozilla/5.0 (Linux; Android 10; ASUS_I001DA Build/QKQ1.190825.002; wv) AppleWebKit/537.36 (KHTML, like Gecko) Version/4.0 Chrome/78.0.3904.62 XWEB/2691 MMWEBSDK/201101 Mobile Safari/537.36 MMWEBID/5561 MicroMessenger/7.0.21.1783(0x27001543) Process/tools WeChat/arm64 Weixin GPVersion/1 NetType/WIFI Language/zh_CN ABI/arm64")
                .addHeader("Accept", "*/*")
                .addHeader("X-Requested-With", "com.tencent.mm")
                .addHeader("Sec-Fetch-Site", "same-origin")
                .addHeader("Sec-Fetch-Mode", "cors")
                .addHeader("Accept-Encoding", "gzip, deflate")
                .addHeader("Accept-Language", "zh-CN,zh;q=0.9,en-US;q=0.8,en;q=0.7")
                .addHeader("Cookie", cookie)
                .build();
        Response response = client.newCall(request).execute();
        
        
        //查询账单，将数据库中不存在的账单条目加入数据库中
        String  billmsg=response.body().string();
        System.out.println(billmsg);


        

        return  new   ModelAndView("test","msg",billmsg);
    }

```

## 使用情景描述

```
在mumu安卓模拟器上设置网络代理，mitmdump是python网络代理，会监听安卓模拟器上的包

mitmdump  -p 8083 -s fetch_cookies.py  '~u https://wx.tenpay.com/'


安卓模拟器每隔一段时间使用宏，宏每隔一段时间点击一次微信的账单，这时候会发送一个账单请求，mitmdump可以抓取到这个账单请求，提取请求里面的参数存放的mysql数据库中(抓取微信的账单请求的部分参数是微信这个软件里面根据一些算法生成的，而且有一定的有效时间)


我们使用监听到账app在手机端一直监听，当收到收款信息后，向我们的服务器发送一个请求/getBill路径，这里面会执行的代码是，从mysql数据库中取出请求的参数拼接成一个可用的获取账单的请求，然后进行发送，收到回复后将账单数据加入到mysql数据库中供用户进行兑换



```

### 如何使用

#### 1.开启mitmdump网络代理

```
activate py38
mitmweb -p 8083

mitmdump  -p 8083 -s fetch_cookies.py  '~u https://wx.tenpay.com/'
```



#### 2.安卓模拟器将网络代理设置成mitmdump的地址，登录微信，然后开启宏，每隔一段时间点击微信的账单

![image-20210424225755665](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210424225803.png)

#### 

#### 3.在安卓模拟器上开启收款监听app





