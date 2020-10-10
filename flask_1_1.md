### 给 flask url增加变量规则

为了给 URL 增加变量的部分，你需要把一些特定的字段标记成 `<variable_name>`。这些特定的字段将作为参数传入到你的函数中。当然也可以指定一个可选的转换器通过规则 `<converter:variable_name>`。 这里有一些不错的例子:

```python
from flask import Flask

app = Flask(__name__)

@app.route('/user/<username>')
def show_user_profile(username):
    # show the user profile for that user
    return 'User %s' % username

@app.route('/post/<int:post_id>')
def show_post(post_id):
    # show the post with the given id, the id is an integer
    return 'Post %d' % post_id

if __name__ == '__main__':
    app.run(port=4396)
```

在启动之后：

```bash
* Serving Flask app "route_variable" (lazy loading)
 * Environment: production
   WARNING: This is a development server. Do not use it in a production deployment.
   Use a production WSGI server instead.
 * Debug mode: off
 * Running on http://127.0.0.1:4396/ (Press CTRL+C to quit)
```

在浏览器输入`http://127.0.0.1:4396/user/afan`:

![avatar](https://res.cloudinary.com/afan1996/image/upload/v1602310659/WX20201010-141456_2x_dg2xin.png)

