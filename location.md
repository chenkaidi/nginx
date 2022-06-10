### 原文

https://www.jb51.net/article/244331.htm

##### 目录

- [1.location和proxy_pass都带/，则真实地址不带location匹配目录](https://www.jb51.net/article/244331.htm#_label0)
- [2.location不带/，proxy_pass带/，则真实地址会带/](https://www.jb51.net/article/244331.htm#_label1)
- [3.location带/，proxy_pass不带/，则真实地址会带location匹配目录/api/](https://www.jb51.net/article/244331.htm#_label2)
- [4.location和proxy_pass都不带/，则真实地址会带location匹配目录/api/](https://www.jb51.net/article/244331.htm#_label3)
- [5.同1，但 proxy_pass带地址](https://www.jb51.net/article/244331.htm#_label4)
- [6.同2，但 proxy_pass带地址，则真实地址会多个/](https://www.jb51.net/article/244331.htm#_label5)
- [7.同3，但 proxy_pass带地址，则真实地址会直接连起来](https://www.jb51.net/article/244331.htm#_label6)
- [8.同4，但 proxy_pass带地址，则真实地址匹配地址会替换location匹配目录](https://www.jb51.net/article/244331.htm#_label7)
- [总结](https://www.jb51.net/article/244331.htm#_label8)

前置测试访问域名：www.test.com/api/upload



## 1.location和proxy_pass都带/，则真实地址不带location匹配目录

```
location ``/api/` `{``  ``proxy_pass http:``//127``.0.0.1:8080/;``}
```

访问地址：www.test.com/api/upload-->http://127.0.0.1:8080/upload



## 2.location不带/，proxy_pass带/，则真实地址会带/

```
location ``/api` `{``  ``proxy_pass http:``//127``.0.0.1:8080/;``}
```

访问地址： www.test.com/api/upload-->http://127.0.0.1:8080//upload



## 3.location带/，proxy_pass不带/，则真实地址会带location匹配目录/api/

```
location ``/api/` `{``  ``proxy_pass http:``//127``.0.0.1:8080;``}
```

访问地址： www.test.com/api/upload-->http://127.0.0.1:8080/api/upload



## 4.location和proxy_pass都不带/，则真实地址会带location匹配目录/api/

```
location ``/api` `{``  ``proxy_pass http:``//127``.0.0.1:8080;``}
```

访问地址： www.test.com/api/upload-->http://127.0.0.1:8080/api/upload



## 5.同1，但 proxy_pass带地址

```
location ``/api/` `{``  ``proxy_pass http:``//127``.0.0.1:8080``/server/``;``}
```

访问地址： www.test.com/api/upload-->http://127.0.0.1:8080/server/upload



## 6.同2，但 proxy_pass带地址，则真实地址会多个/

```
location ``/api` `{``  ``proxy_pass http:``//127``.0.0.1:8080``/server/``;``}
```

访问地址： www.test.com/api/upload-->http://127.0.0.1:8080/server//upload



## 7.同3，但 proxy_pass带地址，则真实地址会直接连起来

```
location ``/api/` `{``  ``proxy_pass http:``//127``.0.0.1:8080``/server``;``}
```

访问地址： www.test.com/api/upload-->http://127.0.0.1:8080/serverupload



## 8.同4，但 proxy_pass带地址，则真实地址匹配地址会替换location匹配目录

```
location ``/api` `{``  ``proxy_pass http:``//127``.0.0.1:8080``/server``;``}
```

访问地址： www.test.com/api/upload-->http://127.0.0.1:8080/server/upload



## 总结

1.proxy_pass代理地址端口后有目录(包括 / )，转发后地址：代理地址+访问URL目录部分去除location匹配目录 
2.proxy_pass代理地址端口后无任何，转发后地址：代理地址+访问URL目录部