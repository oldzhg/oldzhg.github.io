---
title: Django学习笔记
tags: Tech
categories: 工具技巧
date: 2020-03-23 14:33:41
---

django使用https
=============

使用 runserver 是不能使用 https 的

解决办法:使用 runsslserver

步骤:

1.安装:

```
pip install django-extensions 
pip install django-werkzeug-debugger-runserver 
pip install pyOpenSSL 
```

2.启动方式

```
python manage.py runsslserver 0.0.0.0:8100 \
--certificate /etc/pki/libvirt-spice/server-cert.pem \
--key /etc/pki/libvirt-spice/server-key.pem 
或者
python3 manage.py runsslserver 0.0.0.0:8000 --certificate /path/to/certificate.crt --key /path/to/key.key
```

3.修改setting.py中的INSTALLED_APPS

```
INSTALLED_APPS = (...
"sslserver",
...
) 
```



又拍云默认屏蔽了请求源站后的跟随参数，如果配置了CDN值得注意



Django两种请求方式GET和POST

```
request.GET['username']
request.POST['username']
```

值得注意的是**POST可以接受空值，而GET接受空值时会报错**，可以加条件判断跳过

也可以采用下面这种方式

Try this and observe username printed: `http://127.0.0.1:8000/StartPage?username=test`.

Use [`get()`](https://docs.python.org/2/library/stdtypes.html#dict.get) and avoid `MultiValueDictKeyError` errors:

```py
request.GET.get('username', '') 
```

后面可以设置成为空的默认参数



还有前后传参的时候不要把变量名写错了，不然会很麻烦。

看这位大佬就排除半天问题，最后仅仅是因为一个变量名的原因。

> 此处之所以出错，获取不到，是因为之前同事笔误，把参数名：function_group_id，误写为了fucntion_group_id，改回来，即可正常传递参数了。
>
> ​																																			---------[引用来源](https://www.crifan.com/django_request_query_params_cannot_receive_get_para/)



# [基于Django框架的网站部署](https://segmentfault.com/a/1190000008507042)

## 1.域名

    首先，当我们输入一个网址http://www.example.com/时，首先经过DNS解析到对应的IP地址，从而对该IP实现访问。所以，要让别人访问我们项目的第一步，就是需要拥有两样东西，域名和公网ip。
    域名的获得很简单，随便注册购买一个就好了。然后需要的是将域名解析到你的公网ip。而公网ip，一般在购买云服务器的时候能获得。
    经过这一步，我们实现了：请求-->DNS-->服务器ip，而我们的最终目的就是：请求-->DNS-->服务器ip-->黑盒子-->项目wsgi应用

## 2.使用gunicorn运行项目

具体gunicorn 的使用可以自行google一下

## 3.nginx接收外部请求，内部转发

在/etc/nginx/sites-available/文件夹下，新建一个文件blog，并添加如下简单设置

```
server {
    listen 80;
    server_name  你的域名 你的公网ip(可选);
    access_log  /var/log/nginx/blog.log;
    location /static {
        #静态文件如js，css的存放目录
        root /project/blog;
    }
    location / {
        include proxy_params;
        # 从外部接收请求后转发到本地的8000端口
        proxy_pass http://127.0.0.1:8000;
    }
}
```

从上面我们就可以明白，nginx 接收到请求后，转发给gunicorn正在监听的本地8000端口，gunicorn根据请求调用项目中相应的应用函数后返回结果。
自此我们就基本实现了请求-->DNS-->服务器ip-->nginx（80端口）-->127.0.0.1：8000-->项目wsgi应用
而关于nginx和gunicorn的具体配置还有许多，不妨多google一下延伸学习

## 4.总结

gunicorn让项目跑起来
nginx负责接收请求和转发请求到运行中项目监听请求的端口
部署到线上，主要需要域名，公网ip，二者均可以通过云服务器来解决，所以最好还是直接买个云服务器实践一下，just do it







Python3 同级目录直接import

不同级目录

from 目录 import 文件



ls | grep -v .mp4 | xargs rm -r



## 捕获url参数

进行url匹配时 把需要捕获的部分设置成正则表达式的组
Django框架 会自动把组内参数传递给对应视图函数

### 1.位置参数 (要求顺序)

使用正则位置参数 视图的形参名可以随意指定
`re_path(r"index(\d+)", views.index)`
(\d+)内的参数将被传递给index视图函数的形参

给位置参数必须数量一致，路由中有几个正则组，视图函数中就应该有几个形参，包含自带request参数

request 包含 用户通过浏览器请求的参数

### 2.关键字参数 （可以指定顺序）

`re_path(r"index(?P<name>\d+)", views.index)`
`(?P<name>\d+)`内的参数通过关键字参数传递给视图函数
视图函数必须以name命名形参，也就是命名必须一致，此时参数顺序不要求

**小结：**在url正则表达式通过’()‘来匹配传递的参数，如`’(\w+)'`代表匹配字符串，然后在视图函数的形参中加入对应数量的参数（必须在视图函数的形参中接收），即可在后台接收到传入的参数，他会根据对应的顺序依次赋值。当然我们也可以在匹配参数过程中指定对应的形参名称，如：

`url(r'^params_test_reg/str(?P<str>\w+)page(?P<page>\d+)/$',params_test_reg),`

**位置参数与关键字参数不能混用，但是用了一个位置参数时，要求剩下的必须都是位置参数，否则出现 `missing 1 required positional argument:`错误。



`request`是一个`HttpRquest`类型的对象

request参数是一个WSGIRequest对象
WSGIRequest 对象属于django.core.handlers.wsgi.WSGIRequest
request对象包含浏览器请求的信息 是将WISG协议中传递给框架的env信息的再次封装

#### WSGIRequest对象的属性

**request.path**: 返回字符串 表示请求的路径 不包含域名和参数部分
**request.method**: 返回字符串 表示HTTP请求的方式 如: "GET" "POST"
**request.encoding**: 返回字符串 表示请求的编码格式 如果为None表示使用浏览器默认设置utf-8 **注意:这个属性不是只读的如果修改 接下来对属性的访问使用新的encoding值**



可以在templates下新建404.html 和 500.html
将使用此模板替代默认404页面、500页面



#### QueryDict对象

WSGIRequest 对象的GET 和POST 属性用来取值
request.GET 或 request.POST 是一个 QueryDict 对象
QueryDict 对象属于 django.http.request.QueryDict

#### 创建QueryDict对象

q = QueryDict("a=1&a=2&b=3")

#### 对QueryDict对象取值

q["a"] 返回一个字符串
q.get("b") 返回一个字符串
q.getlist("a") 返回一个列表
如果取值不存在的键 使用[]取值会报错KeyError 使用get() 返回None 使用getlist 返回空列表

#### get()方法设置默认值

q.get("d", "default") 如果没有d这个Key 会返回字符串default
q.getlist("d", [1, "2"]) 如果没有d 这个Key 会返回列表[1, "2"]

#### QueryDict与普通字典的区别

QueryDict的Key可以对应多个值
q = queryDict("a=1&a=2&a=3")
用[]和get()取值有多个值的QueryDict对象时 只返回最后一个Key的值
q["a"] 返回 3
q.get("a") 返回 3
如果要对QueryDict取值Key对应的所有值 使用getlist()方法
q.getlist("a") 返回一个列表["1", "2", "3"]



## Cookie

Cookie 在访问网站时服务器生成的储存在浏览器的一段文本信息
Django中cookie中的值只以字符串形式储存
访问服务器时服务器设置一个cookie key为要保存的id值为要保存的信息
将cookie发送给浏览器浏览器保存在本地
访问服务器时浏览器发送cookie 服务器通过cookie的id取出信息用来判断状态

#### Cookie的特点

request.COOKIES是一个普通的Python字典
1.以键值对的方式存储 并且只储存字符串类型的值
2.通过浏览器访问网站时 浏览器会将所有跟这个网站相关的Cookie发送给网站
3.基于域名安全(不会将其他网站的cookie发给不相关的网站)
4.有过期时间 如果不指定时间 默认关闭浏览器后cookie就会过期设置cookie

通过HttpResponse类的对象或者它的子类的对象
使用对象.set_cookie()设置cookie
对象.set_cookie("key", value, max_age)



```bash
response = HttpResponse("设置cookie")
response.set_cookie("num", 1)
renturn response
```

实际是服务器在ResponseHeader里设置了Set-Cookie的值
浏览器发现Set-Cookie的值后会在本地存储对应的cookie

#### 读取cookie

通过HttpRequest类的对象或者它的子类的对象
使用对象.COOKIES[]读取cookie
对象.COOKIES["key"]

```kotlin
cookie = request.Cookie["num"]
return HttpResponse(cookie)
```

在读取cookie时 实际是浏览器在RequsetHeader里设置了Cookie的值
浏览器将本地保存的有关这个网站的cookie保存在Cookie全部发送给服务器

#### Cookie的寿命

浏览器在保存cookie时默认当浏览器关闭时使cookie过期
response.set_cookie(max_age= , expirse= )
max_age= 以秒为单位设置cookie过期时间
expirse= 以到期日期计算cookie过期时间

## session

Django的session中可以储存任意类型的数据
所有的session信息储存在服务器数据库中的session表中
表的主键叫做session_key唯一标示session在表中的位置
表的值叫做 session_data储存了设置的session键值对
过期时间叫做expire_date储存了session过期日期
访问服务器时服务器设置session为数据表创建记录
服务器发送一个key为sessionid，值为session_key的cookie给浏览器
访问时服务器通过key为sessionid的cookie获得session_key
后台再通过session_key在数据库中获得session_data

#### session特点

1.以键值对的方式存储 并且可以储存**任意类型**的值
2.依赖于cookie 唯一标识码sessionid就是cookie的key session_key就是cookie的值
3.有过期时间 如果不指定 默认两周时间

#### 设置session

通过HttpRequest对象的session属性设置和读取session信息
保存时类似于给字典添加键值对
对象.session["key"] = value



```bash
request.session["username"] = "smart"
```

#### 读取session

通过HttpRequset对象的session属性读取session信息
取值和没有值时的默认值
username = 对象.session["username"]
username = 对象.session.get("key", 默认值)



```csharp
username = request.session["username"]
username = request.session.get("username", "没有数据")
```

#### session的方法

session_data在数据表中的状态



```bash
4b662403cdb5656fdba155811da63a59046fe33a:{"username":"smart","password":"passwd"}
```

删除session值的部分 也就是session_data中括号中的部分
对象.session.clear()
删除session整条数据 也就是session_data对应的整条数据表记录
对象.session.flush()
删除session中指定的键值对
del 对象.session["key"]
判断session中是否有对应的key
对象.session.has_key("key")
设置会话的超时时间
如果不设置值则等同于设置为None
对象.session.set_expory(value)

- 如果value是一个整数，sessionid的cookie将在value秒后过 session也会在value秒后过期
- 如果value为0，sessionid的cookie浏览器关闭时过期 session会在两周后过期
- 如果value为None，sessionid的cookie两周后过期 session也会在两周后过期

## cookie和session的应用场景 

cookie: 记住用户名 安全性要求不高
session: 银行卡账户 密码 涉及到安全性比较高的数据

**注意：**要使用session还需要密匙种子SECRET_KEY，在app初始化添加进去既可以(app.config[SECRT_KEY] = '123WER1245erwrwe&*%W34@##*&@!)



### 日志记录

```python
import logging
logger = logging.getLogger('django')
logger = logging.getLogger("console")
logger.debug("hello")
logger.info('This is an error msg')
```

