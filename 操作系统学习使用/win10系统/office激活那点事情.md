# office激活码那点事情

office的激活和windows系统的激活是分开，我们把系统激活之后，如果想要使用office需要单独激活



```
查看授权代码，根据office安装路径调整
cscript.exe "C:\Program Files (x86)\Microsoft Office\Office16\ospp.vbs" /dstatus
```



## 购买实测

### 1.office2019专业增强版的秘钥，价格50元，商家家说可以永久激活，可以反复重装，可以网页绑定



#### 安装完成后查看授权信息

![](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200905094134.png)



#### 在虚拟机里面使用淘宝买的密钥进行激活（我已经激活过一次在真机，第一次激活需要选择电话激活，后续我重装后只需要点击联网激活）

![](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200905094514.png)



#### 切换vhd真机启动后再进行联网激活，发现可以激活

![](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200905095524.png)

查看授权信息，发现多了一条授权 ICENSE NAME: Office 19, 0ffice19roplus2019MSDNR_ Retail edition

![](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200905095532.png)







#### 所谓的网页绑定

![image-20200905103407176](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200905103407.png)

![image-20200905103215691](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200905103215.png)

### 2. 另外一家商店购买，office2019专业增强版的秘钥，价格10元，商家说可以重装

商家没有让我去官网进行绑定下载，给了我一个百度云和ed2k链接，其实这个ed2k链接就是msdn tell you网站里面office2019镜像的下载链接

为了验证上面所说的50元的网页绑定，我用这个激活码去官方那边进行了测试，发现这个10元的激活码和上面50元的激活码一样可以网页绑定（50元就是铁坑我，我已经和他理论他退款给我了，改成了10块钱）

==所以这个10元和50元是一个类型的秘钥==





输入验证码然后进行下载绑定

![image-20200905105310859](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200905105310.png)







==可以发现所谓的网页绑定上已经有两个了==

![image-20200905105400580](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200905105400.png)







为了验证这种激活是不是绑定电脑的，在虚拟机里面完成这个密钥的第一次激活

#### 选择网络激活,不能进行网络激活

![image-20200905110009484](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200905110009.png)



选择电话激活

![image-20200905110248060](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200905110248.png)



在这个网站https://webact.185.hk/输入安装id就可以获得确认id

![image-20200905110259328](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200905110259.png)



完成激活

![image-20200905110527039](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200905110527.png)

### 3.另外一个商店购买，office家庭版和学生版本，价格50，商家说可以绑定微软账号

在网页激活绑定后可以在账号的服务和订阅处发现

![image-20200905141233300](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200905141233.png)



安装完office2019家庭版后，打开word然后登陆我的微软账号，发现自动激活了office

![image-20200905141423391](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200905141423.png)

查看授权激活信息

/7![](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200905141504.png)

这个密钥是最靠谱的，属于oem密钥但是可以绑定微软账号，可以反复重装激活并且更换电脑也能激活，只要登录微软账号office就会联网激活

## 结论

![image-20200905121325631](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200905121333.png)

授权信息中 ICENSE NAME: Office 19, 0ffice19roplus2019MSDNR_ Retail edition

可以看见是msdn零售证书

这是零售许可证，适用于为MSDN订阅付费的人。MSDN帐户的所有者是唯一有权使用该软件的帐户，并且不允许在生产中使用（不允许商用）。如果该帐户的所有者要取消其订阅，则该密钥不会过期，并且当前激活的产品将保持激活状态。==但是，如果Microsoft检测到密钥被滥用（例如非法转售给他人），则他们可以阻止将来使用密钥来激活任何东西。==

如果您在公司中使用这些密钥，则应停止使用，因为它们不合法。您还应该在此处报告转销商：

[https://www.microsoft.com/zh-cn/howtotell/cfr/report.aspx](https://www.microsoft.com/en-us/howtotell/cfr/report.aspx)

Microsoft许可证上实际上没有任何标记。如果您看到TechSoup以外的其他网站的价格低于既定经销商的价格，则它们可能不是合法的。





