# spring相关题目



## springboot多个配置文件加载顺序配置

```
https://segmentfault.com/a/1190000023514649?utm_source=sf-similar-article

https://segmentfault.com/a/1190000017508365

spring.profiles.active=dev

先加载 application.properties中的配置
然后加载application-dev.properties中的配置, 如果重复的, 用dev去覆盖上面的; 如果dev中没有的, application.properties中的配置就生效!
```

