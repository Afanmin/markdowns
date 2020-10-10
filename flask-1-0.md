---
title: flask学习笔记(一)——简单的flask接口部署
date: 2020-10-10 11:02:38
tags:
	- web
	- flask
catagory: flask
---

### flask简介

flask是一款使用python编写的轻量级web框架。它使用简单的核心，通过extension增加其他功能。由于它简单的开发及部署方式，是以python作为编程语言的程序员喜闻乐见的web开发框架。使用flask可以非常快速并且对原本代码几乎无需修改的情况下，将某个脚本所实现的功能作为接口部署在服务器上。



### 安装

由于是python编写，通过pip就可以很轻松的安装flask。官方推荐使用venv在虚拟环境下安装使用，在开发环境下这是一个非常好的开发范例。然而，作为练习使用装在conda环境下完全没有问题（非常方便）。

```bash
pip install Flask
```



### 一个简单的flask应用

一个flask应用往往拥有以下结构：

```python
#hello.py hello-world server
from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello_world():
    return 'Hello World!'

if __name__ == '__main__':
    app.run()
```

将它保存下来后，使用python解释器运行它就可以将它启动在5000端口。如果你需要将它启动在特定端口或者使它能被公网访问，可以在`app.run()`传入`host='0.0.0.0'`以及`port=$指定端口号$`。

```bash
$ python hello.py
 * Running on http://127.0.0.1:5000/
```

我们可以稍微解释一下这段代码：

1. 首先我们导入了类 [`Flask`](http://www.pythondoc.com/flask/api.html#flask.Flask) 。这个类的实例化将会是我们的 WSGI 应用。第一个参数是应用模块的名称。 如果你使用的是单一的模块（就如本例），第一个参数应该使用 __name__。因为取决于如果它以单独应用启动或作为模块导入， 名称将会不同 （ `'__main__'` 对应于实际导入的名称）。获取更多的信息，请阅读 [`Flask`](http://www.pythondoc.com/flask/api.html#flask.Flask) 的文档。
2. 接着，我们创建一个该类的实例。我们传递给它模块或包的名称。这样 Flask 才会知道去哪里寻找模板、静态文件等等。
3. 我们使用装饰器 [`route()`](http://www.pythondoc.com/flask/api.html#flask.Flask.route) 告诉 Flask 哪个 URL 才能触发我们的函数。
4. 定义一个函数，该函数名也是用来给特定函数生成 URLs，并且返回我们想要显示在用户浏览器上的信息。
5. 最后我们用函数 [`run()`](http://www.pythondoc.com/flask/api.html#flask.Flask.run) 启动本地服务器来运行我们的应用。`if __name__ == '__main__':` 确保服务器只会在该脚本被 Python 解释器直接执行的时候才会运行，而不是作为模块导入的时候。

这段是官方给到的解释。我感觉有点复杂，简而言之就是用route修饰的函数，就是通过路由之后执行并且返回结果的代码。换而言之，可以通过route修饰器，将一个脚本里的函数，变成web应用。

### POST方法和request变量

`route`是一个传入多个参数的修饰器，首先一个必须的参数是路由规则，`'/'`，`'/hello'`分别对应了访问时在ip及端口后所接字段的样式。其次，`method`也是一个重要的参数，默认为`['GET']`，即通过get的方式访问该接口。将这个参数设为`['POST']`就只接受post形式的访问了。

```python
@app.route('/', methods=['POST'])
def form_args():
    try:
        vector = json.loads(request.form['vector'])
        path = request.form['path']
        return 'Hello World, from'+' '+path
    except:
        abort(501)
```

在使用了route修饰函数之后，函数中会多一个request变量，它是对访问发起方发来的请求的一个封装。通过访问`request.method`可以知道访问的方式，对post的访问方式，可以通过`request.form`访问到发过来的表格。

