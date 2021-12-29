# Flask学习

## Flask环境

#### **虚拟环境**

**`virtualenv`**是一个虚拟的`Python`环境构建器。它可以帮助用户并行创建多个`Python`环境。 因此，它可以避免不同版本的库之间的兼容性问题。

以下命令用于安装**`virtualenv`：**

```python
pip install virtualenv
```

此命令需要管理员权限。您可以在Linux / Mac OS上的 **pip** 之前添加 **`sudo`** 。

如果您使用的是Windows，请以管理员身份登录。在`Ubuntu`上， **`virtualenv`**可以使用它的包管理器安装。

```
sudo apt-get install virtualenv
```

安装后，将在文件夹中创建新的虚拟环境。

```python
mkdir newproj
cd newproj
virtualenv venv
```

要在 **Linux / OS X** 上激活相应的环境，请使用以下命令：

```
venv/bin/activate
```

要在 **Windows** 上激活相应的环境，可以使用以下命令：

```python
venv\scripts\activate
```

我们现在准备在这个环境中安装`Flask`：

```python
pip install Flask
```

上述命令可以直接运行，不需要系统范围安装的虚拟环境。

---

## **Flask应用**

为了测试 **`Flask`** 安装，请在编辑器中将以下代码输入 **`Hello.py`：**

```python
from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello_world():
   return '<h1>Hello World<h1>'

if __name__ == '__main__':
   app.run()
```

必须在项目中导入`Flask`模块。 

Flask类的一个对象是我们的**`WSGI`**应用程序。

Flask构造函数使用**当前模块（__name __）**的名称作为参数。

Flask类的**route()**函数是一个装饰器，它告诉应用程序哪个`URL`应该调用相关的函数。

```
app.route(rule, options)
```

- **rule** 参数表示与该函数的URL绑定。
- **options** 是要转发给基础Rule对象的参数列表。

在上面的示例中，'/ ' URL与**hello_world()**函数绑定。

因此，当在浏览器中打开web服务器的主页时，将呈现该函数的输出。

最后，Flask类的**run()**方法在本地开发服务器上运行应用程序。

```
app.run(host, port, debug, options)
```

所有参数都是可选的

| 序号 | 参数与描述                                                   |
| ---- | ------------------------------------------------------------ |
| 1    | **host** 要监听的主机名。 默认为127.0.0.1（`localhost`）。设置为“0.0.0.0”以使服务器在外部可用 |
| 2    | **port** 默认值为5000                                        |
| 3    | **debug** 默认为false。 如果设置为true，则提供调试信息       |
| 4    | **options** 要转发到底层的`Werkzeug`服务器。                 |

上面给出的**Python**脚本是从Python shell执行的。

```
python Hello.py
```

Python shell中的消息通知您：

```
* Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
```

在浏览器中打开上述URL**`（localhost：5000）`**。将显示**“Hello World”**消息。

**这里注意，如果使用python自带的idle运行的时候可能会报以下错误：** 

```python
Traceback (most recent call last): 

File “C:/learn/python/xuexi/web/demoflask/app.py”, line 27, in 

app.run(); 

File “C:\Users\zhang\AppData\Local\Programs\Python\Python36\lib\site-packages\flask\app.py”, line 938, in run 

cli.show_server_banner(self.env, self.debug, self.name, False) 

File “C:\Users\zhang\AppData\Local\Programs\Python\Python36\lib\site-packages\flask\cli.py”, line 629, in show_server_banner 

click.echo(message) 

File “C:\Users\zhang\AppData\Local\Programs\Python\Python36\lib\site-packages\click\utils.py”, line 217, in echo 

file = _default_text_stdout() 

File “C:\Users\zhang\AppData\Local\Programs\Python\Python36\lib\site-packages\click_compat.py”, line 621, in func 

rv = wrapper_func() 

File “C:\Users\zhang\AppData\Local\Programs\Python\Python36\lib\site-packages\click_compat.py”, line 385, in get_text_stdout 

rv = _get_windows_console_stream(sys.stdout, encoding, errors) 

File “C:\Users\zhang\AppData\Local\Programs\Python\Python36\lib\site-packages\click_winconsole.py”, line 261, in _get_windows_console_stream 

func = _stream_factories.get(f.fileno()) 

io.UnsupportedOperation: fileno
```

只要不用idle执行就不会出错了，不影响后续使用。改用`cmd`下Python执行或者`pycharm`等运行都能成功。

#### **调试模式**

通过调用**run()**方法启动**Flask**应用程序。但是，当应用程序正在开发中时，应该为代码中的每个更改手动重新启动它。为避免这种不便，请启用**调试支持**。

如果代码更改，服务器将自行重新加载。它还将提供一个有用的调试器来跟踪应用程序中的错误。

在运行或将调试参数传递给**run()**方法之前，通过将**application**对象的**debug**属性设置为**True**来启用**Debug模式**。

```python
app.debug = True
app.run()
app.run(debug = True)
```

-----

## Flask 路由

现代Web框架使用路由技术来帮助用户记住应用程序URL。

可以直接访问所需的页面，而无需从主页导航。

Flask中的**route()**装饰器用于将URL绑定到函数。例如：

```python
@app.route('/hello')def hello_world():
   return 'hello world'
```

在这里，URL **'/ hello'** 规则绑定到**hello_world()**函数。 

因此，如果用户访问**http://localhost:5000/hello** ，

**hello_world()**函数的输出将在浏览器中呈现。

application对象的**add_url_rule()**函数也可用于将URL与函数绑定，如上例所示，使用**route()**。

装饰器的目的也由以下表示：

```python
def hello_world():
   return 'hello world'
app.add_url_rule('/', 'hello', hello_world)
```

----

## Flask 变量规则

通过向规则参数添加变量部分，可以动态构建URL。

此变量部分标记为**\<variable-name>**  。

它作为关键字参数传递给与规则相关联的函数。

在以下示例中，**route()**装饰器的规则参数包含附加到URL **'/hello'** 的**。** 

因此，如果在浏览器中输入**http://localhost:5000/hello/w3cschool**作为**URL**，则**'w3cschool'**将作为参数提供给 **hello()**函数。

```
from flask import Flask
app = Flask(__name__)

@app.route('/hello/<name>')
def hello_name(name):
   return 'Hello %s!' % name

if __name__ == '__main__':
   app.run(debug = True)
```

将上述脚本保存为**hello.py**并从Python shell运行它。

接下来，打开浏览器并输入URL**`http:// localhost:5000/hello/xueshaoxin`。**

以下输出将显示在浏览器中:

```
Hello xueshaoxin!
```

除了默认字符串变量部分之外，还可以使用以下转换器构建规则：

| 序号 | 转换器和描述                      |
| :--- | :-------------------------------- |
| 1    | **int**接受整数                   |
| 2    | **float**对于浮点值               |
| 3    | **path **接受用作目录分隔符的斜杠 |

在下面的代码中，使用了所有这些构造函数：

```python
from flask import Flask
app = Flask(__name__)

@app.route('/blog/<int:postID>')
def show_blog(postID):
   return 'Blog Number %d' % postID

@app.route('/rev/<float:revNo>')
def revision(revNo):
   return 'Revision Number %f' % revNo

if __name__ == '__main__':
   app.run()
```

从Python Shell运行上面的代码。访问浏览器中的URL **http://localhost:5000/blog/11**。

给定的数字用作**show_blog()**函数的参数。浏览器显示以下输出：

```
Blog Number 11
```

在浏览器中输入此URL - **http://localhost:5000/rev/1.1**

**revision()**函数将浮点数作为参数。以下结果显示在浏览器窗口中：

```
Revision Number 1.100000
```

Flask的URL规则基于**`Werkzeug`**的路由模块。

这确保形成的URL是唯一的，并且基于Apache规定的先例。

考虑以下脚本中定义的规则：

```python
from flask import Flask
app = Flask(__name__)

@app.route('/flask')
def hello_flask():
   return 'Hello Flask'

@app.route('/python/')
def hello_python():
   return 'Hello Python'

if __name__ == '__main__':
   app.run()
```

这两个规则看起来类似，但在第二个规则中，使用斜杠**（/）**。因此，它成为一个规范的URL。因此，使用 **/python** 或 **/python/**返回相同的输出。

但是，如果是第一个规则，**/flask/ URL**会产生“404 Not Found”页面。

-----

## Flask URL构建

**url_for()**函数对于动态构建特定函数的URL非常有用。

**url_for()**函数接受函数的名称作为第一个参数，以及一个或多个关键字参数，每个参数对应于URL的变量部分。

以下脚本演示了如何使用**url_for()**函数：

```python
from flask import Flask, redirect, url_for
app = Flask(__name__)
@app.route('/admin')
def hello_admin():
   return 'Hello Admin'
@app.route('/guest/<guest>')
def hello_guest(guest):
   return 'Hello %s as Guest' % guest
@app.route('/user/<name>')
def hello_user(name):
   if name =='admin':
      return redirect(url_for('hello_admin'))
   else:
      return redirect(url_for('hello_guest', guest = name))
if __name__ == '__main__':
   app.run(debug = True)
```

上述脚本有一个函数**hello_user****(name)**，它接受来自URL的参数的值。

**hello_user****()**函数检查接收的参数是否与`'admin'`匹配。

如果匹配，则使用**url_for()**将应用程序重定向到**hello_admin()**函数，否则重定向到将接收的参数作为guest参数传递给它的**hello_guest()**函数。

保存上面的代码并从Python shell运行。

打开浏览器并输入URL - **[http://localhost:5000/user/admin](http://localhost:5000/hello/admin)**

浏览器中的应用程序响应是：

```
Hello Admin
```

在浏览器中输入以下URL - **[http://localhost:5000/user/mvl](http://localhost:5000/hello/mvl)**

应用程序响应现在更改为：

```
Hello mvl as Guest
```

---

## Flask HTTP方法

`Http`协议是万维网中数据通信的基础。在该协议中定义了从指定URL检索数据的不同方法。

下表总结了不同的`http`方法：

| 序号 | 方法与描述                                                   |
| ---- | ------------------------------------------------------------ |
| 1    | **GET**以未加密的形式将数据发送到服务器。最常见的方法。      |
| 2    | **HEAD**和GET方法相同，但没有响应体。                        |
| 3    | **POST**用于将HTML表单数据发送到服务器。POST方法接收的数据不由服务器缓存。 |
| 4    | **PUT**用上传的内容替换目标资源的所有当前表示。              |
| 5    | **DELETE** 删除由URL给出的目标资源的所有当前表示。           |

默认情况下，Flask路由响应**GET**请求。但是，可以通过为**route()**装饰器提供方法参数来更改此首选项。

为了演示在URL路由中使用**POST**方法，首先让我们创建一个HTML表单，并使用**POST**方法将表单数据发送到URL。

将以下脚本另存为`login.html`

```html
<html>
   <body>
      <form action = "http://localhost:5000/login" method = "post">
         <p>Enter Name:</p>
         <p><input type = "text" name = "nm" /></p>
         <p><input type = "submit" value = "submit" /></p>
      </form>
   </body>
</html>
```

现在在`Python shell`中输入以下脚本：

```python
from flask import Flask, redirect, url_for, request
app = Flask(__name__)

@app.route('/success/<name>')
def success(name):
   return 'welcome %s' % name

@app.route('/login',methods = ['POST', 'GET'])
def login():
   if request.method == 'POST':
      user = request.form['nm']
      return redirect(url_for('success',name = user))
   else:
      user = request.args.get('nm')
      return redirect(url_for('success',name = user))

if __name__ == '__main__':
   app.run(debug = True)
```

开发服务器开始运行后，在浏览器中打开**`login.html`**，在文本字段中输入`name`，然后单击**提交**。

![Flask HTTP方法](https://atts.w3cschool.cn/attachments/tuploads/flask/post_method_example.jpg)

表单数据将POST到表单标签的`action`子句中的`URL`。

**http://localhost/login**映射到**login()**函数。由于服务器通过**POST**方法接收数据，因此通过以下步骤获得从表单数据获得的“nm”参数的值：

```
user = request.form['nm']
```

它作为变量部分传递给**'/success'** URL。浏览器在窗口中显示**welcome** 消息。

![Flask HTTP方法](https://atts.w3cschool.cn/attachments/tuploads/flask/welcome_message.jpg)

在**`login.html`**中将方法参数更改为**'GET'**，然后在浏览器中再次打开它。服务器上接收的数据是通过**GET**方法获得的。通过以下的步骤获得'nm'参数的值：

```
User = request.args.get('nm')
```

这里，**`args`**是包含表单参数对及其对应值对的列表的字典对象。与'nm'参数对应的值将像之前一样传递到'/ success' URL。

----

## Flask 模板

在前面的实例中,视图函数的主要作用是生成请求的响应,这是最简单请求。

**视图函数有两个作用:**

- 处理业务逻辑
- 返回响应内容

在大型应用中,把业务逻辑和表现内容放在一起,会增加代码的复杂度和维护成本.

- 模板其实是一个包含响应文本的文件,其中用占位符(变量)表示动态部分,告诉模板引擎其具体的值需要从使用的数据中获取
- 使用真实值替换变量,再返回最终得到的字符串,这个过程称为'渲染'
- Flask 是使用 `Jinja2 `这个模板引擎来渲染模板

**使用模板的好处**

- 视图函数只负责业务逻辑和数据处理(业务逻辑方面)
- 而模板则取到视图函数的数据结果进行展示(视图展示方面)
- 代码结构清晰,耦合度低

#### 模板基本使用

在项目下创建 templates 文件夹，用于存放所有模板文件，并在目录下创建一个模板文件 html 文件 hello.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
我的模板html内容
</body>
</html>
```

创建视图函数,将该模板内容进行渲染返回

```python
from flask import Flask, render_template
app = Flask(__name__)

@app.route('/')
def index():    
    return render_template('hello.html')
```

#### 模板变量

代码中传入字符串，列表，字典到模板中

```python
from flask import Flask, render_template
app = Flask(__name__)

@app.route('/')
def index():    
    # 往模板中传入的数据    
    my_str = 'Hello Word'    
    my_int = 10    
    my_array = [3, 4, 2, 1, 7, 9]    
    my_dict = {        
        'name': 'xiaoming',        
        'age': 18    
    }    
    return render_template('hello.html',                           
                           my_str=my_str,                           
                           my_int=my_int,                           
                           my_array=my_array,                           
                           my_dict=my_dict                           
                          )
```

模板中代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
</head>
<body>
  我的模板html内容
  <br />{{ my_str }}
  <br />{{ my_int }}
  <br />{{ my_array }}
  <br />{{ my_dict }}
</body>
</html>
```

运行效果

```
我的模板html内容
Hello Word
10
[3, 4, 2, 1, 7, 9]
{'name': 'xiaoming', 'age': 18}
```

#### 示例代码:

```python
from flask import Flask, render_template
app = Flask(__name__)

@app.route('/')
def index():    
    my_int = 18    
    my_str = 'curry'    
    my_list = [1, 5, 4, 3, 2]    
    my_dict = {        'name': 'durant',        'age': 28    }    
    # render_template方法:渲染模板    
    # 参数1: 模板名称  参数n: 传到模板里的数据    
    return render_template('hello.html',                           
                           my_int=my_int,                           
                           my_str=my_str,                           
                           my_list=my_list,                           
                           my_dict=my_dict
                          )
if __name__ == '__main__':    
    app.run(debug=True)
```

```html
<!DOCTYPE html>
<html lang="en">
<head>  
    <meta charset="UTF-8">  
    <title>Title</title></head><body>  
    <h2>我是模板</h2>  
    {{ my_int }}  
    <br>  
    {{ my_str }}  
    <br>  
    {{ my_list }}  
    <br>  
    {{ my_dict }}  
    <hr>  
    <h2>模板的list数据获取</h2>  
    <hr>  
    {{ my_list[0] }}  
    <br> 
    {{ my_list.1 }}  
    <hr>  
    <h2>字典数据获取</h2>  
    <hr>  
    {{ my_dict['name'] }}  
    <br>  
    {{ my_dict.age }}  
    <hr>  
    <h2>算术运算</h2>  
    <br>  
    {{ my_list.0 + 10 }}  
    <br>  
    {{ my_list[0] + my_list.1 }}
    </body>

</html>
```

---

## Flask 静态文件

Web应用程序通常需要静态文件，例如**javascript**文件或支持网页显示的**CSS**文件。

通常，配置Web服务器并为您提供这些服务，但在开发过程中，这些文件是从您的包或模块旁边的*static*文件夹中提供，它将在应用程序的**/static**中提供。

特殊端点'static'用于生成静态文件的URL。

在下面的示例中，在**index.html**中的HTML按钮的**OnClick**事件上调用**hello.js**中定义的**javascript**函数，该函数在Flask应用程序的**“/”**URL上呈现。

```python
from flask import Flask, render_template
app = Flask(__name__)

@app.route("/")
def index():
   return render_template("index.html")

if __name__ == '__main__':
   app.run(debug = True)
```

**index.html**的HTML脚本如下所示：

```html
<html>

   <head>
      <script type = "text/javascript" 
         src = "{{ url_for('static', filename = 'hello.js') }}" ></script>
   </head>
   
   <body>
      <input type = "button" onclick = "sayHello()" value = "Say Hello" />
   </body>
   
</html>
```

**在static文件夹中的hello.js**包含**sayHello()**函数。

```js
function sayHello() {
   alert("Hello World")
}
```

---

## Flask Request对象

来自客户端网页的数据作为全局请求对象发送到服务器。为了处理请求数据，应该从Flask模块导入。

Request对象的重要属性如下所列：

- **Form** - 它是一个字典对象，包含表单参数及其值的键和值对。
- **args** - 解析查询字符串的内容，它是问号（？）之后的URL的一部分。
- **Cookies** - 保存Cookie名称和值的字典对象。
- **files** - 与上传文件有关的数据。
- **method** - 当前请求方法。

---

## Flask 将表单数据发送到模板

我们已经看到，可以在 URL 规则中指定 http 方法。触发函数接收的 **Form** 数据可以以字典对象的形式收集它并将其转发到模板以在相应的网页上呈现它。

在以下示例中，**'/' URL** 会呈现具有表单的网页（student.html）。

填入的数据会发布到触发 **result()** 函数的 **'/result' URL**。

**result()** 函数收集字典对象中的 **request.form** 中存在的表单数据，并将其发送给 **result.html**。

该模板动态呈现**表单**数据的 HTML 表格。

下面给出的是应用程序的 Python 代码：

```python
from flask import Flask, render_template, request
app = Flask(__name__)
@app.route('/')
def student():
   return render_template('student.html')
@app.route('/result',methods = ['POST', 'GET'])
def result():
   if request.method == 'POST':
      result = request.form
      return render_template("result.html",result = result)
if __name__ == '__main__':
   app.run(debug = True)
```

下面给出的是 **student.html** 的 HTML 脚本。

```html
<form action="http://localhost:5000/result" method="POST">
     <p>Name <input type = "text" name = "Name" /></p>
     <p>Physics <input type = "text" name = "Physics" /></p>
     <p>Chemistry <input type = "text" name = "chemistry" /></p>
     <p>Maths <input type ="text" name = "Mathematics" /></p>
     <p><input type = "submit" value = "submit" /></p>
</form>
```

下面给出了模板**（ result.html ）**的代码：

```html
<!doctype html>
  <table border = 1>
     {% for key, value in result.items() %}
    <tr>
       <th> {{ key }} </th>
       <td> {{ value }}</td>
    </tr>
 {% endfor %}
</table>
```

运行 Python 脚本，并在浏览器中输入 URL **http://localhost:5000/**。

![1640241792890](C:\Users\abc\AppData\Roaming\Typora\typora-user-images\1640241792890.png)

当点击**提交**按钮时，表单数据以 HTML 表格的形式呈现在 **result.html** 上。

![1640241863756](C:\Users\abc\AppData\Roaming\Typora\typora-user-images\1640241863756.png)

----

## Flask Cookies

Cookie以文本文件的形式存储在客户端的计算机上。其目的是记住和跟踪与客户使用相关的数据，以获得更好的访问者体验和网站统计信息。

**Request对象**包含Cookie的属性。它是所有cookie变量及其对应值的字典对象，客户端已传输。除此之外，cookie还存储其网站的到期时间，路径和域名。

在Flask中，对cookie的处理步骤为：

**1.设置cookie：**

  设置cookie,默认有效期是临时cookie,浏览器关闭就失效

  可以通过 max_age 设置有效期， 单位是秒

```python
 resp = make_response("success")   # 设置响应体
 resp.set_cookie("w3cshool", "w3cshool", max_age=3600)
```


 **2.获取cookie**

  获取cookie，通过request.cookies的方式， 返回的是一个字典，可以获取字典里的相应的值

```python
cookie_1 = request.cookies.get("w3cshool")
```

**3.删除cookie**

  这里的删除只是让cookie过期，并不是直接删除cookie

  删除cookie，通过delete_cookie()的方式， 里面是cookie的名字

```python
resp = make_response("del success")  # 设置响应体
resp.delete_cookie("w3cshool")
```

以下为Flask Cookies的简单示例：

```python
from flask import Flask, make_response, request # 注意需导入 make_response

app = Flask(__name__)

@app.route("/set_cookies")
def set_cookie():
    resp = make_response("success")
    resp.set_cookie("w3cshool", "w3cshool",max_age=3600)    
    return resp

@app.route("/get_cookies")
def get_cookie():
    cookie_1 = request.cookies.get("w3cshool")  # 获取名字为Itcast_1对应cookie的值
    return cookie_1

@app.route("/delete_cookies")
def delete_cookie():
    resp = make_response("del success")
    resp.delete_cookie("w3cshool")
    return resp

if __name__ == '__main__':
    app.run(debug=True)
```

**设置cookies**

运行应用程序，在浏览器中输入 127.0.0.1:5000/set_cookies 来设置cookies，设置 cookies 的输出如下所示：

![1640242604351](C:\Users\abc\AppData\Roaming\Typora\typora-user-images\1640242604351.png)

**获取cookie**

根据视图函数中相对应的路径，输入 http://127.0.0.1:5000/get_cookies ，读回 cookies 的输出如下所示：

![1640242664066](C:\Users\abc\AppData\Roaming\Typora\typora-user-images\1640242664066.png)

**删除cookie**

根据视图函数中相对应的路径，输入 http://127.0.0.1:5000/delete_cookies ，删除 cookies 的输出。

> 注意删除，只是让 cookie 过期。 

---

## Flask 会话

与Cookie不同，**Session（会话）**数据存储在服务器上。会话是客户端登录到服务器并注销服务器的时间间隔。需要在该会话中保存的数据会存储在服务器上的临时目录中。

为每个客户端的会话分配**会话ID**。会话数据存储在cookie的顶部，服务器以加密方式对其进行签名。对于此加密，Flask应用程序需要一个定义的**SECRET_KEY**。

Session对象也是一个字典对象，包含会话变量和关联值的键值对。

例如，要设置一个**'username'**会话变量，请使用以下语句：

```python
Session['username'] = 'admin'
```

要释放会话变量，请使用**pop()**方法。

```python
session.pop('username', None)
```

以下代码是Flask中的会话工作的简单演示。URL **'/'**只是提示用户登录，因为未设置会话变量**'username'**。

```python
@app.route('/')
def index():
   if 'username' in session:
      username = session['username']
         return 'Logged in as ' + username + '<br>'  \
         "<b><a href = '/logout'>click here to log out</a></b>"
   return "You are not logged in <br><a href = '/login'></b>" &plus; \
      "click here to log in</b></a>"
```

当用户浏览到“/login”  login()视图函数时，因为它是通过GET方法调用的，所以将打开一个登录表单。

表单发送回**'/login'**，现在会话变量已设置。应用程序重定向到**'/'**。此时会话变量**'username'**被找到。

```python
@app.route('/login', methods = ['GET', 'POST'])
def login():
   if request.method == 'POST':
      session['username'] = request.form['username']
      return redirect(url_for('index'))
   return '''
	
   <form action = "" method = "post">
      <p><input type="text" name="username"/></p>
      <p<<input type="submit" value="Login"/></p>
   </form>
	
   '''
```

应用程序还包含一个**logout()**视图函数，它会弹出**'username'**会话变量。因此，**'/'** URL再次显示开始页面。

```python
@app.route('/logout')
def logout():
   # remove the username from the session if it is there
   session.pop('username', None)
   return redirect(url_for('index'))
```

运行应用程序并访问主页。（确保设置应用程序的**secret_key**）

```python
from flask import Flask, session, redirect, url_for, escape, request
app = Flask(__name__)
app.secret_key = 'any random string’
```

完整代码如下所示

```python
from flask import render_templatefrom flask import make_response
from flask import Flask, session, redirect, url_for, escape, request
app = Flask(__name__)
app.secret_key = 'fkdjsafjdkfdlkjfadskjfadskljdsfklj'
@app.route('/')
def index():
    if 'username' in session:
        username = session['username']
        return '登录用户名是:' + username + '<br>' + \
                 "<b><a href = '/logout'>点击这里注销</a></b>"
    return "您暂未登录， <br><a href = '/login'></b>" + \
         "点击这里登录</b></a>"
@app.route('/login', methods = ['GET', 'POST'])
def login():
    if request.method == 'POST':
        session['username'] = request.form['username']
        return redirect(url_for('index'))
    return '''
   <form action = "" method = "post">
      <p><input type="text" name="username"/></p>
      <p><input type="submit" value ="登录"/></p>
   </form>
   '''
@app.route('/logout')
def logout():
   # remove the username from the session if it is there
   session.pop('username', None)
   return redirect(url_for('index'))
if __name__ == '__main__':
    app.run(debug = True)
```

输出将显示如下。点击**“点击此处登录”**链接。

![img](data:image/png;base64,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)

链接将被定向到另一个屏幕。键入“admin”。

![img](data:image/png;base64,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iO2T2E6J7RVhb/7D3jxtK9dlWe3GKext+n9DIvDFl2f8j0o89keAAAECBAgQqJNAbJ/Edkpsr8TjumyAWw7Bc1Y1IOwJe5v+4osvzfhfshz44v4Xv/hF+tWvflWn73nLQoAAAQIECLRIILZDYnukvH2iV0+4mlW4qtt87MYp7G067EUxD1lkjDAAABRnSURBVAe+2FUi9o2PW5z9yo2BGlADakANqAE1MKsayNsgsT2Se/QEPUGvbgFslstz8a5rXXphluBNm1eEvXLgi/9Fi4Og4wvWjYEaUANqQA2oATUw6xqI7ZDYHolbDnqxrdK0bTDtEWLHqYHFHVcLe+NAGWbtD1Q59MUXa9zyF6377g8OBw5qQA2oATWgBqZbA3kbRMhbe7vNdm17fIQ9u3FO9H+6cuhz3+3x5MBBDagBNaAG1MD21IBA055AY12PXtf9sPfUU08lNwZqQA2oATWgBtSAGlADakANqIFm1EA/7LXoJE2aSoAAAQIECBAgQIAAgcYLLO3Y1z1mr/Et1UACBAgQIECAAAECBAi0SEDYa9HK1lQCBAgQIECAAAECBNojYDfO9qxrLSVAgAABAgQIECBAoEUCwl6LVramEiBAgAABAgQIECDQHoGFy97rmL32rG4tJUCAAAECBAgQIECgLQLLO68R9tqysrWTAAECBAgQIECAAIH2CFy861phrz2rW0sJECBAgAABAgQIEGiLwIZ69j784Q+n5eXldNFFF7kxmIsaiHqNuvVHgAABAgQIECBAoG0CY196ITaYhTwhd15rQOBr21eb9hIgQIAAAQIECCztHPOi6rlH78SJE9QIzI1A1GsE1KhffwQIECBAgAABAgTaJDD2bpy5R6dNONraDAG124z1qBUECBAgQIAAAQIbE+j27O29ft2xbDCvS2SAmgqo3ZquGItFgAABAgQIECAwVYHlnXE2zlmEvaM3p07n5nRko835yRfTVZ2r0hd/stERDU+gKyDsqQQCBAgQIECAAIE2CiztiOvsCXttXPetabOw15pVraEECBAgQIAAAQIlgYt3va/mYa+0sB4S2IyAsLcZNeMQIECAAAECBAjMu8BycVH1zfbsFbtYdlKn07vdNLiT5pGbVt67+abSbpy9XTq/+KWr+uPefDSlgeGP9mjLu3H2Ht9808p4naF5zvsKsfyTFxD2Jm9qigQIECBA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)

屏幕会显示消息“ **登录用户名是：admin** ”

![img](data:image/png;base64,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)

-----

## Flask 重定向和错误

Flask类有一个**redirect()**函数。调用时，它返回一个响应对象，并将用户重定向到具有指定状态代码的另一个目标位置。

**redirect()**函数的原型如下：

```python
Flask.redirect(location, statuscode, response)
```

在上述函数中：

- **location**参数是应该重定向响应的URL。
- **statuscode**发送到浏览器标头，默认为302。
- **response**参数用于实例化响应。

以下状态代码已标准化：

- HTTP_300_MULTIPLE_CHOICES
- HTTP_301_MOVED_PERMANENTLY
- HTTP_302_FOUND
- HTTP_303_SEE_OTHER
- HTTP_304_NOT_MODIFIED
- HTTP_305_USE_PROXY
- HTTP_306_RESERVED
- HTTP_307_TEMPORARY_REDIRECT

**默认状态**代码为**302**，表示**'found'**。

在以下示例中，**redirect()**函数用于在登录尝试失败时再次显示登录页面。

```python
from flask import Flask, redirect, url_for, render_template, request
# Initialize the Flask application
app = Flask(__name__)

@app.route('/')
def index():
   return render_template('log_in.html')

@app.route('/login',methods = ['POST', 'GET'])
def login():
   if request.method == 'POST' and
       request.form['username'] == 'admin' :
           return redirect(url_for('success'))
           return redirect(url_for('index'))

@app.route('/success')
def success():
   return 'logged in successfully'
	
if __name__ == '__main__':
   app.run(debug = True)
```

Flask类具有带有错误代码的**abort()**函数。

```python
Flask.abort(code)
```

**Code**参数采用以下值之一：

- **400** - 用于错误请求
- **401** - 用于未身份验证的
- **403** - Forbidden
- **404** - 未找到
- **406** - 表示不接受
- **415** - 用于不支持的媒体类型
- **429** - 请求过多

让我们对上述代码中的**login()**函数稍作更改。如果要显示**'Unauthurized'**页面，请将其替换为调用**abort(401)**，而不是重新显示登录页面。

```python
from flask import Flask, redirect, url_for, render_template, request, abort
app = Flask(__name__)

@app.route('/')
def index():
   return render_template('log_in.html')

@app.route('/login',methods = ['POST', 'GET'])
def login():
   if request.method == 'POST':
      if request.form['username'] == 'admin' :
         return redirect(url_for('success'))
      else:
         abort(401)
   else:
      return redirect(url_for('index'))

@app.route('/success')
def success():
   return 'logged in successfully'

if __name__ == '__main__':
   app.run(debug = True)
```

----

## Flask 消息闪现

Flask 提供了一个非常简单的方法来使用闪现系统向用户反馈信息。闪现系统使得在一个请求结束的时候记录一个信息，`然后在且仅仅在下一个请求中访问这个数据`，强调flask闪现是基于`flask`内置的`session`的，利用浏览器的`session`缓存闪现信息。所以必须设置`secret_key`。

在 Flask Web 应用程序中生成这样的信息性消息很容易。Flask 框架的闪现系统可以在一个视图中创建消息，并在名为 **next** 的视图函数中呈现它。

Flask 模块包含 **flash()** 方法。它将消息传递给下一个请求，该请求通常是一个模板。

```python
flash(message, category)
```

其中，

- **message** 参数是要闪现的实际消息。
- **category** 参数是可选的。它可以是“error”，“info”或“warning”。

为了从会话中删除消息，模板调用 **get_flashed_messages()**。

```python
get_flashed_messages(with_categories, category_filter)
```

两个参数都是可选的。如果接收到的消息具有类别，则第一个参数是元组。第二个参数仅用于显示特定消息。

以下闪现在模板中接收消息。

```
{% with messages = get_flashed_messages() %}
   {% if messages %}
      {% for message in messages %}
         {{ message }}
      {% endfor %}
   {% endif %}
{% endwith %}
```

让我们看一个简单的例子，演示Flask中的闪现机制。在以下代码中，**'/'** URL 显示登录页面的链接，没有消息闪现。

```
@app.route('/')
def index():
    return render_template('index.html')
```

该链接会将用户引导到**'/login'** URL，该 URL 显示登录表单。提交时，**login()** 视图函数验证用户名和密码，并相应闪现 **'success'** 消息或创建 **'error'** 变量。

```python
@app.route('/login', methods = ['GET', 'POST'])
def login():
    error = None
    if request.method == 'POST':
        if request.form['username'] != 'admin' or request.form['password'] != 'admin':
           error = 'Invalid username or password. Please try again!'
       else:
           flash('You were successfully logged in')
           return redirect(url_for('index'))
    return render_template('login.html', error = error)
```

如果出现**错误**，则会重新显示登录模板，并显示错误消息。

**login.html**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Login</title>
</head>
<body>
    <form method = "post" action = "http://localhost:5000/login">
        <table>
            <tr>
                <td>Username</td>
                <td><input type = 'username' name = 'username'></td>
            </tr>
            <tr>
                <td>Password</td>
                <td><input type = 'password' name = 'password'></td>
            </tr>
            <tr>
                <td><input type = "submit" value = "Submit"></td>
            </tr>
        </table>
    </form>
    {% if error %}
        <p><strong>Error</strong>: {{ error }}</p>
    {% endif %}
</body>
</html>
```

另一方面，如果登录成功，则会在索引模板上刷新成功消息。

**Index.html**

```php+HTML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Index</title>
</head>
<body>
    {% with messages = get_flashed_messages() %}
         {% if messages %}
               {% for message in messages %}
                    <p>{{ message }}</p>
               {% endfor %}
         {% endif %}
    {% endwith %}
<h3>Welcome!</h3>
<a href = "{{ url_for('login') }}">login</a>
</body>
</html>
```

下面给出了 Flask 消息闪现示例的完整代码：

**Flash.py**

```python
from flask import Flask, flash, redirect, render_template, request, url_for
app = Flask(name)
app.secret_key = 'random string'
@app.route('/')
def index():
    return render_template('index.html')
@app.route('/login', methods = ['GET', 'POST'])
def login():
    error = None
    if request.method == 'POST':
        if request.form['username'] != 'admin' or request.form['password'] != 'admin':
            error = 'Invalid username or password. Please try again!'
        else:
            flash('You were successfully logged in')
            return redirect(url_for('index'))
    return render_template('login.html', error = error)
```

执行上述代码后，您将看到如下所示的界面。

![Flask Message Flashing Example](https://atts.w3cschool.cn/attachments/tuploads/flask/flask_message_flashing_example.jpg)

当您点击链接，您将被定向到登录页面。

输入用户名和密码。

![Login Page](https://atts.w3cschool.cn/attachments/tuploads/flask/login_page.jpg)

点击**登录**。将显示一条消息“您已成功登录”。

![Successfully Logged in Page](https://atts.w3cschool.cn/attachments/tuploads/flask/successfully_logged_in_page.jpg)

----

## Flask 文件上传

在 Flask 中处理文件上传非常简单。它需要一个 HTML 表单，其 `enctype` 属性设置为“`multipart/form-data”`，将文件发布到 URL。

URL 处理程序从 `request.files[]` 对象中提取文件，并将其保存到所需的位置。



每个上传的文件首先会保存在服务器上的临时位置，然后将其实际保存到它的最终位置。

目标文件的名称可以是硬编码的，也可以从 `request.files[file] `对象的` filename `属性中获取。

但是，建议使用 `secure_filename()` 函数获取它的安全版本。



可以在 Flask 对象的配置设置中定义默认上传文件夹的路径和上传文件的最大大小。

> app.config['UPLOAD_FOLDER'] 定义上传文件夹的路径 

> app.config['MAX_CONTENT_LENGTH'] 指定要上传的文件的最大大小（以字节为单位）

以下代码具有 `'/upload' `URL 规则，该规则在 templates 文件夹中显示` 'upload.html'`，以及 `'/upload-file' `URL 规则，用于调用 `uploader() `函数处理上传过程。

`'upload.html' `有一个文件选择器按钮和一个提交按钮。

```html
<html>
<head>
  <title>File Upload</title>
</head>
<body>
    <form action="http://localhost:5000/uploader" method="POST" enctype="multipart/form-data">
        <input type="file" name="file"  />
        <input type="submit" value="提交" />
    </form>
</body>
</html>
```

您将看到如下所示的界面。

![img](data:image/png;base64,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)

选择文件后，单击提交。

表单的` post `方法调用` '/upload_file'` URL。

底层函数 `uploader()` 执行保存操作。 

以下是 Flask 应用程序的 Python 代码。

```python
from flask import Flask, render_template, request
from werkzeug.utils import secure_filenameimport os

app = Flask(__name__)
app.config['UPLOAD_FOLDER'] = 'upload/'

@app.route('/upload')
def upload_file():
    return render_template('upload.html')

@app.route('/uploader',methods=['GET','POST'])
def uploader():
    if request.method == 'POST':
        f = request.files['file']        
        print(request.files)        
        f.save(os.path.join(app.config['UPLOAD_FOLDER'], secure_filename(f.filename)))        	 return 'file uploaded successfully'    
    else:        
        return render_template('upload.html')if __name__ == '__main__':
   app.run(debug=True)
```

注意：app.config['UPLOAD_FOLDER'] = 'upload/'

upload 前面不能加“/”。 

上传成功会显示以下画面：

![img](data:image/png;base64,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)



上次文件被放到根目录的 upload 文件夹下：

![img](data:image/png;base64,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) 

----

## Flask 扩展

Flask通常被称为微框架，因为核心功能包括基于**Werkzeug**的WSGI和路由以及基于**Jinja2**的模板引擎。

此外，Flask框架还支持cookie和会话，以及**JSON**，静态文件等Web帮助程序。

显然，这不足以开发完整的Web应用程序。

而**Flask扩展**就具备这样的功能**。**Flask扩展为Flask框架提供了可扩展性。

**有大量的Flask扩展可用**。

Flask扩展是一个Python模块，它向Flask应用程序添加了特定类型的支持。

Flask Extension Registry（Flask扩展注册表）是一个可用的扩展目录。

可以通过**pip**实用程序下载所需的扩展名。

**Flask常用扩展包：**

- Flask-SQLalchemy：操作数据库；
- Flask-script：插入脚本；
- Flask-migrate：管理迁移数据库；
- Flask-Session：Session存储方式指定；
- Flask-WTF：表单；
- Flask-Mail：邮件；
- Flask-Bable：提供国际化和本地化支持，翻译；
- Flask-Login：认证用户状态；
- Flask-OpenID：认证；
- Flask-RESTful：开发REST API的工具；
- Flask-Bootstrap：集成前端Twitter Bootstrap框架；
- Flask-Moment：本地化日期和时间；
- Flask-Admin：简单而可扩展的管理接口的框架



每种类型的扩展通常提供有关其用法的大量文档。

由于扩展是一个Python模块，因此需要导入它才能使用它。

Flask 的扩展通常命名为“ Flask-Foo ”或者“ Foo-Flask ” 。可以在 PyPI 搜索 标记为 [Framework :: Flask](https://pypi.org/search/?c=Framework+%3A%3A+Flask) 扩展包。

**使用扩展**

请参阅每个扩展的文档以了解其安装、配置和使用说明。

一般来说，扩展从 [app.config](https://dormousehole.readthedocs.io/en/latest/api.html#flask.Flask.config) 获取其自身的配置并在初始化时传递给 应用实例。

例如，一个名为“ Flask-Foo ”的扩展使用如下:

```python
from flask_foo import Foo

foo = Foo()

app = Flask(__name__)
app.config.update(
    FOO_BAR='baz',
    FOO_SPAM='eggs',
)

foo.init_app(app)
```

----

## Flask 邮件

基于web的应用程序通常需要具有向用户/客户端发送邮件的功能。

**Flask-Mail**扩展使得与任何电子邮件服务器建立简单的接口变得非常容易。

首先，应该在pip实用程序的帮助下安装Flask-Mail扩展。

```
pip install Flask-Mail
```

然后需要通过设置以下应用程序参数的值来配置Flask-Mail。

| 序号 | 参数与描述                                                   |
| ---- | ------------------------------------------------------------ |
| 1    | **MAIL_SERVER**电子邮件服务器的名称/IP地址                   |
| 2    | **MAIL_PORT**使用的服务器的端口号                            |
| 3    | **MAIL_USE_TLS**启用/禁用传输安全层加密                      |
| 4    | **MAIL_USE_SSL**启用/禁用安全套接字层加密                    |
| 5    | **MAIL_DEBUG**调试支持。默认值是Flask应用程序的调试状态      |
| 6    | **MAIL_USERNAME**发件人的用户名                              |
| 7    | **MAIL_PASSWORD**发件人的密码                                |
| 8    | **MAIL_DEFAULT_SENDER**设置默认发件人                        |
| 9    | **MAIL_MAX_EMAILS**设置要发送的最大邮件数                    |
| 10   | **MAIL_SUPPRESS_SEND**如果app.testing设置为true，则发送被抑制 |
| 11   | **MAIL_ASCII_ATTACHMENTS**如果设置为true，则附加的文件名将转换为ASCII |

`flask-mail`模块包含以下重要类的定义。

#### Mail类

它管理电子邮件消息传递需求。类构造函数采用以下形式：

```
flask-mail.Mail(app = None)
```

构造函数将Flask应用程序对象作为参数。

#### Mail类的方法

| 序号 | 方法与描述                        |
| ---- | --------------------------------- |
| 1    | **send()**发送Message类对象的内容 |
| 2    | **connect()**打开与邮件主机的连接 |
| 3    | **send_message()**发送消息对象    |

#### Message类

它封装了一封电子邮件。Message类构造函数有几个参数:

```
flask-mail.Message(subject, recipients, body, html, sender, cc, bcc, 
   reply-to, date, charset, extra_headers, mail_options, rcpt_options)
```

#### Message类方法

**attach()** - 为邮件添加附件。此方法采用以下参数：

- **filename** - 要附加的文件的名称
- **content_type** - MIME类型的文件
- **data** - 原始文件数据
- **处置** - 内容处置（如果有的话）。

**add_recipient()** - 向邮件添加另一个收件人

在下面的示例中，Google gmail服务的SMTP服务器用作Flask-Mail配置的MAIL_SERVER。

**步骤1** - 在代码中从flask-mail模块导入Mail和Message类。

```python
from flask_mail import Mail, Message
```

**步骤2** - 然后按照以下设置配置Flask-Mail。

```python
app.config['MAIL_SERVER']='smtp.gmail.com'
app.config['MAIL_PORT'] = 465
app.config['MAIL_USERNAME'] = 'yourId@gmail.com'
app.config['MAIL_PASSWORD'] = '*****'
app.config['MAIL_USE_TLS'] = False
app.config['MAIL_USE_SSL'] = True
```

**步骤3** - 创建Mail类的实例。

```python
mail = Mail(app)
```

**步骤4** - 在由URL规则**（‘/’）**映射的Python函数中设置Message对象。

```python
@app.route("/")
def index():
   msg = Message('Hello', sender = 'yourId@gmail.com', recipients = ['id1@gmail.com'])
   msg.body = "This is the email body"
   mail.send(msg)
   return "Sent"
```

**步骤5** - 整个代码如下。

在Python Shell中运行以下脚本并访问**http://localhost:5000/。**

```python
from flask import Flask
from flask_mail import Mail, Message

app =Flask(__name__)
mail=Mail(app)

app.config['MAIL_SERVER']='smtp.gmail.com'
app.config['MAIL_PORT'] = 465
app.config['MAIL_USERNAME'] = 'yourId@gmail.com'
app.config['MAIL_PASSWORD'] = '*****'
app.config['MAIL_USE_TLS'] = False
app.config['MAIL_USE_SSL'] = True
mail = Mail(app)

@app.route("/")
def index():
   msg = Message('Hello', sender = 'yourId@gmail.com', recipients = ['id1@gmail.com'])
   msg.body = "Hello Flask message sent from Flask-Mail"
   mail.send(msg)
   return "Sent"

if __name__ == '__main__':
   app.run(debug = True)
```

请注意，Gmail服务中的内置不安全功能可能会阻止此次登录尝试。您可能必须降低安全级别。请登录您的Gmail帐户并访问此[链接](https://www.google.com/settings/security/lesssecureapps)以降低安全性。

![Decrease the Security](https://atts.w3cschool.cn/attachments/tuploads/flask/decrease_the_security.jpg)

----

## Flask WTF

Web应用程序的一个重要方面是为用户提供用户界面。HTML提供了一个****标签，用于设计界面。

可以适当地使用**Form（表单）** 元素，例如文本输入，单选按钮，选择等。

用户输入的数据以Http请求消息的形式通过GET或POST方法提交给服务器端脚本。

- 服务器端脚本必须从http请求数据重新创建表单元素。因此，实际上，表单元素必须定义两次 - 一次在`HTML`中，另一次在服务器端脚本中。
- 使用HTML表单的另一个缺点是很难（如果不是不可能的话）动态呈现表单元素。`HTML`本身无法验证用户的输入。

这就是**WTForms**的作用，一个灵活的表单，渲染和验证库，能够方便使用。

Flask-WTF扩展为这个**WTForms**库提供了一个简单的接口。

使用**Flask-WTF**，我们可以在Python脚本中定义表单字段，并使用`HTML`模板进行渲染。还可以将验证应用于`WTF`字段。

让我们看看这种动态生成的`HTML`是如何工作的。

首先，需要安装`Flask-WTF`扩展。

```
pip install flask-WTF
```

已安装的软件包包含一个`Form`类，该类必须用作用户定义表单的父级。

**WTforms**包中包含各种表单字段的定义。下面列出了一些**标准表单字段**。

| 序号 | 标准表单字段与描述                                          |
| :--- | :---------------------------------------------------------- |
| 1    | **TextField**表示<input type ='text'> HTML表单元素          |
| 2    | **BooleanField**表示<input type ='checkbox'> HTML表单元素   |
| 3    | **DecimalField**用于显示带小数的数字的文本字段              |
| 4    | **IntegerField**用于显示整数的文本字段                      |
| 5    | **RadioField**表示<input type = 'radio'> HTML表单元素       |
| 6    | **SelectField**表示选择表单元素                             |
| 7    | **TextAreaField**表示<textarea> HTML表单元素                |
| 8    | **PasswordField**表示<input type = 'password'> HTML表单元素 |
| 9    | **SubmitField**表示<input type = 'submit'>表单元素          |

例如，包含文本字段的表单可以设计如下：

```
from flask_wtf import Form
from wtforms import TextField

class ContactForm(Form):
   name = TextField("Name Of Student")
```

除了`'name'`字段，还会自动创建CSRF令牌的隐藏字段。这是为了防止**Cross Site Request Forgery（\**跨站请求伪造\**）**攻击。

渲染时，**这将导致等效的`HTML`脚本**，如下所示：

```
<input id = "csrf_token" name = "csrf_token" type = "hidden" />
<label for = "name">Name Of Student</label><br>
<input id = "name" name = "name" type = "text" value = "" />
```

在Flask应用程序中使用用户定义的表单类，并使用模板呈现表单。

```python
from flask import Flask, render_template
from forms import ContactForm
app = Flask(__name__)
app.secret_key = 'development key'

@app.route('/contact')
def contact():
   form = ContactForm()
   return render_template('contact.html', form = form)

if __name__ == '__main__':
   app.run(debug = True)
```

WTForms包也包含验证器类。它对表单字段应用验证很有用。以下列表显示了常用的验证器。

| 序号 | 验证器类与描述                                          |
| :--- | :------------------------------------------------------ |
| 1    | **DataRequired**检查输入字段是否为空                    |
| 2    | **Email **检查字段中的文本是否遵循电子邮件ID约定        |
| 3    | **IPAddress** 在输入字段中验证IP地址                    |
| 4    | **Length** 验证输入字段中的字符串的长度是否在给定范围内 |
| 5    | **NumberRange**验证给定范围内输入字段中的数字           |
| 6    | **URL**验证在输入字段中输入的URL                        |

现在，我们将对联系表单中的**name**字段应用**'DataRequired'**验证规则。

```python
name = TextField("Name Of Student",[validators.Required("Please enter your name.")])
```

如果验证失败，表单对象的**validate()**函数将验证表单数据并抛出验证错误。**Error**消息将发送到模板。在HTML模板中，动态呈现error消息。

```
{% for message in form.name.errors %}
   {{ message }}
{% endfor %}
```

以下示例演示了上面给出的概念。**`Contact`表单**的设计如下**（forms.py）**。

```python
from flask_wtf import FlaskForm
from wtforms import TextField, IntegerField, TextAreaField, SubmitField, RadioField,
   SelectField

from wtforms import validators, ValidationError

class ContactForm(Form):
   name = TextField("Name Of Student",[validators.Required("Please enter 
      your name.")])
   Gender = RadioField('Gender', choices = [('M','Male'),('F','Female')])
   Address = TextAreaField("Address")
   
   email = TextField("Email",[validators.Required("Please enter your email address."),
      validators.Email("Please enter your email address.")])
   
   Age = IntegerField("age")
   language = SelectField('Languages', choices = [('cpp', 'C&plus;&plus;'), 
      ('py', 'Python')])
   submit = SubmitField("Send")
```

验证器应用于**`Name`** 和**`Email`**字段。

下面给出了Flask应用程序脚本**（formexample.py）**。

```python
from flask import Flask, render_template, request, flash
from forms import ContactForm
app = Flask(__name__)
app.secret_key = 'development key'

@app.route('/contact', methods = ['GET', 'POST'])
def contact():
   form = ContactForm()
   
   if request.method == 'POST':
      if form.validate() == False:
         flash('All fields are required.')
         return render_template('contact.html',form = form)
      else:
         return render_template('success.html')
      elif request.method == 'GET':
         return render_template('contact.html',form = form)

if __name__ == '__main__':
   app.run(debug = True)
```

模板**（contact.html）**的脚本如下：

```html
<!doctype html>
<html>
   <body>
   
      <h2 style = "text-align: center;">Contact Form</h2>
		
      {% for message in form.name.errors %}
         <div>{{ message }}</div>
      {% endfor %}
     
      
      <form action = "http://localhost:5000/contact" method = post>
         <fieldset>
            <legend>Contact Form</legend>
            {{ form.hidden_tag() }}
            
            <div style ="font-size:20px;font-weight:bold;margin-left:150px;">
               {{ form.name.label }}<br>
               {{ form.name }}
               <br>
               
               {{ form.Gender.label }} {{ form.Gender }}
               {{ form.Address.label }}<br>
               {{ form.Address }}
               <br>
               
               {{ form.email.label }}<br>
               {{ form.email }}
               <br>
               
               {{ form.Age.label }}<br>
               {{ form.Age }}
               <br>
               
               {{ form.language.label }}<br>
               {{ form.language }}
               <br>
               {{ form.submit }}
            </div>
            
         </fieldset>
      </form>
      
   </body>
</html>
```

在`Python shell`中运行**formexample.py**，访问URL **http://localhost:5000/contact**。显示**`Contact`**表单将如下所示。

![Form Example](https://atts.w3cschool.cn/attachments/tuploads/flask/form_example.jpg)

如果有任何错误，页面将如下所示：

![Form Error Page](https://atts.w3cschool.cn/attachments/tuploads/flask/form_error_page.jpg)

如果没有错误，将显示**'success.html'**。

![Form Success Page](https://atts.w3cschool.cn/attachments/tuploads/flask/form_success_page.jpg)

----

## Flask SQLite

在 Flask 中，通过使用特殊的 g 对象可以使用 before_request() 和 teardown_request() 在请求开始前打开数据库连接，在请求结束后关闭连接。

以下是一个在 Flask 中使用` SQLite3 `的例子:

```python
import sqlite3
from flask import g

DATABASE = '/path/to/database.db'

def connect_db():
    return sqlite3.connect(DATABASE)

@app.before_request
def before_request():
    g.db = connect_db()

@app.teardown_request
def teardown_request(exception):
    if hasattr(g, 'db'):
        g.db.close()
```

> 请记住，销毁函数是一定会被执行的。即使请求前处理器执行失败或根本没有执行， 销毁函数也会被执行。因此，我们必须保证在关闭数据库连接之前数据库连接是存在 的。

#### 按需连接

上述方式的缺点是只有在` Flask` 执行了请求前处理器时才有效。如果你尝试在脚本或者 `Python `解释器中使用数据库，那么你必须这样来执行数据库连接代码:

```python
with app.test_request_context():
    app.preprocess_request()
    # now you can use the g.db object
```

这样虽然不能排除对请求环境的依赖，但是可以按需连接数据库:

```python
def get_connection():
    db = getattr(g, '_db', None)
    if db is None:
        db = g._db = connect_db()
    return db
```

这样做的缺点是必须使用 db = get_connection() 来代替直接使用 g.db 。

#### 简化查询

现在，在每个请求处理函数中可以通过访问 g.db 来得到当前打开的数据库连接。为了 简化 SQLite 的使用，这里有一个有用的辅助函数:

```python
def query_db(query, args=(), one=False):
    cur = g.db.execute(query, args)
    rv = [dict((cur.description[idx][0], value)
               for idx, value in enumerate(row)) for row in cur.fetchall()]
    return (rv[0] if rv else None) if one else rv
```

使用这个实用的小函数比直接使用原始指针和转接对象要舒服一点。

可以这样使用上述函数:

```python
for user in query_db('select * from users'):
    print user['username'], 'has the id', user['user_id']
```

只需要得到单一结果的用法:

```python
user = query_db('select * from users where username = ?',
                [the_username], one=True)
if user is None:
    print 'No such user'
else:
    print the_username, 'has the id', user['user_id']
```

如果要给` SQL `语句传递参数，请在语句中使用问号来代替参数，并把参数放在一个列表中 一起传递。不要用字符串格式化的方式直接把参数加入 `SQL` 语句中，这样会给应用带来 [SQL 注入](http://en.wikipedia.org/wiki/SQL_injection) 的风险。

#### 初始化模式

关系数据库是需要模式的，因此一个应用常常需要一个 schema.sql 文件来创建 数据库。因此我们需要使用一个函数来其于模式创建数据库。下面这个函数可以完成这个任务:

```python
from contextlib import closing

def init_db():
    with closing(connect_db()) as db:
        with app.open_resource('schema.sql') as f:
            db.cursor().executescript(f.read())
        db.commit()
```

可以使用上述函数在 Python 解释器中创建数据库：

```python
>>> from yourapplication import init_db
>>> init_db()
```

**以下是一个实例：**

创建一个`SQLite`数据库**'database.db'**并在其中创建学生表。

```python
import sqlite3

conn = sqlite3.connect('database.db')
print "Opened database successfully";

conn.execute('CREATE TABLE students (name TEXT, addr TEXT, city TEXT, pin TEXT)')
print "Table created successfully";
conn.close()
```

我们的Flask应用程序有三个**`View`**功能。

第一个**new_student()**函数已绑定到URL规则**（'/addnew'）**。它呈现包含学生信息表单的`HTML`文件。

```python
@app.route('/enternew')
def new_student():
   return render_template('student.html')
```

**'student.html'**的HTML脚本如下：

```python
<html>
   <body>
      
      <form action = "{{ url_for('addrec') }}" method = "POST">
         <h3>Student Information</h3>
         Name<br>
         <input type = "text" name = "nm" /></br>
         
         Address<br>
         <textarea name = "add" ></textarea><br>
         
         City<br>
         <input type = "text" name = "city" /><br>
         
         PINCODE<br>
         <input type = "text" name = "pin" /><br>
         <input type = "submit" value = "submit" /><br>
      </form>
      
   </body>
</html>
```

可以看出，表单数据被发布到绑定**addrec()**函数的**'/addrec'** URL。

这个**addrec()**函数通过**`POST`**方法检索表单的数据，并插入学生表中。与`insert`操作中的成功或错误相对应的消息将呈现为**'result.html'**。

```python
@app.route('/addrec',methods = ['POST', 'GET'])
def addrec():
   if request.method == 'POST':
      try:
         nm = request.form['nm']
         addr = request.form['add']
         city = request.form['city']
         pin = request.form['pin']
         
         with sql.connect("database.db") as con:
            cur = con.cursor()
            cur.execute("INSERT INTO students (name,addr,city,pin) 
               VALUES (?,?,?,?)",(nm,addr,city,pin) )
            
            con.commit()
            msg = "Record successfully added"
      except:
         con.rollback()
         msg = "error in insert operation"
      
      finally:
         return render_template("result.html",msg = msg)
         con.close()
```

**result.html**的`HTML`脚本包含一个转义语句**{{msg}}**，它显示**`Insert`**操作的结果。

```html
<!doctype html>
<html>
   <body>
   
      result of addition : {{ msg }}
      <h2><a href = "\">go back to home page</a></h2>
      
   </body>
</html>
```

该应用程序包含由**'/list'** URL表示的另一个**list()**函数。

它将**'rows'**填充为包含学生表中所有记录的**MultiDict**对象。此对象将传递给**list.html**模板。

```python
@app.route('/list')
def list():
   con = sql.connect("database.db")
   con.row_factory = sql.Row
   
   cur = con.cursor()
   cur.execute("select * from students")
   
   rows = cur.fetchall()
   return render_template("list.html",rows = rows)
```

此**list.html**是一个模板，它遍历行集并在**`HTML`**表中呈现数据。

```html
<!doctype html>
<html>
   <body>
   
      <table border = 1>
         <thead>
            <td>Name</td>
            <td>Address>/td<
            <td>city</td>
            <td>Pincode</td>
         </thead>
         
         {% for row in rows %}
            <tr>
               <td>{{row["name"]}}</td>
               <td>{{row["addr"]}}</td>
               <td> {{ row["city"]}}</td>
               <td>{{row['pin']}}</td>	
            </tr>
         {% endfor %}
      </table>
      
      <a href = "/">Go back to home page</a>
      
   </body>
</html>
```

最后，**'/' URL**规则呈现**'home.html'**，它作为应用程序的入口点。

```python
@app.route('/')
def home():
   return render_template('home.html')
```

home.html的页面比较简单，只需要负责相关的页面跳转即可：

```html
<!DOCTYPE html>
<html>

<body>
    <a href="/addrec">Add New Record</a>
    <br />
    <a href="/list">Show List</a>
</body>
</html>
```

以下是**`Flask-SQLite`**应用程序的完整代码。

```python
from flask import Flask, render_template, request
import sqlite3 as sql
app = Flask(__name__)

@app.route('/')
def home():
   return render_template('home.html')

@app.route('/enternew')
def new_student():
   return render_template('student.html')

@app.route('/addrec',methods = ['POST', 'GET'])
def addrec():
   if request.method == 'POST':
      try:
         nm = request.form['nm']
         addr = request.form['add']
         city = request.form['city']
         pin = request.form['pin']
         
         with sql.connect("database.db") as con:
            cur = con.cursor()
            
            cur.execute("INSERT INTO students (name,addr,city,pin) 
               VALUES (?,?,?,?)",(nm,addr,city,pin) )
            
            con.commit()
            msg = "Record successfully added"
      except:
         con.rollback()
         msg = "error in insert operation"
      
      finally:
         return render_template("result.html",msg = msg)
         con.close()

@app.route('/list')
def list():
   con = sql.connect("database.db")
   con.row_factory = sql.Row
   
   cur = con.cursor()
   cur.execute("select * from students")
   
   rows = cur.fetchall();
   return render_template("list.html",rows = rows)

if __name__ == '__main__':
   app.run(debug = True)
```

在开发服务器开始运行时，从`Python shell`运行此脚本。在浏览器中访问**http://localhost:5000/**，显示一个简单的菜单：

![Simple Menu](https://atts.w3cschool.cn/attachments/tuploads/flask/simple_menu.jpg)

点击**“添加新记录”**链接以打开**学生信息**表单。

![Adding New Record](https://atts.w3cschool.cn/attachments/tuploads/flask/adding_new_record.jpg)

填写表单字段并提交。底层函数在学生表中插入记录。

![Record Successfully Added](https://atts.w3cschool.cn/attachments/tuploads/flask/record_successfully_added.jpg)

返回首页，然后点击**'显示列表'**链接。将显示一个显示样品数据的表。

![Table Showing Sample Data](https://atts.w3cschool.cn/attachments/tuploads/flask/table_showing_sample_data.jpg)

-----

