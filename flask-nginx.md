---
title: 通过nginx反向代理暴露flask端口
date: 2020-10-10 17:07:37
tags:
	- nginx
---

### 需求描述

现在博客在用的背景图片是某个国外图片网站的端口，用来随机返回壁纸，来达到每次打开网页可以看到不同背景的效果。然后因为这个网站是用的国外的api，在国内不开vpn来访问加载速度实在可惜，所以我打算用flask自己写一个。这时候就需要把flask所在的端口暴露出来。所以我这里就使用nginx的反向代理的功能，对需要用到的端口做端口转发。



### 具体操作

编辑nginx配置文件，我这里的配置文件路径是`/etc/nginx/sites-avaliable/defult`，在sever的结构内加上:

```yml
location /flask {
		proxy_pass http://127.0.0.1:4396/;
	}
```

这里我flask运行在4396端口。

值得注意的一点是，如果这里在端口号后面不加/，那么如果我访问`https://www.minyifan.fun/flask/index.html`则实际访问的是`https://127.0.0.1:4396/flask/index.html`,在加上/之后访问的是`https://127.0.0.1:4396/index.html`

