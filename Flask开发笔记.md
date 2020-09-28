# Flask开发笔记

### 安装Flask

> ```
> pip install Flask
> ```

### 获取参数

常用的两种获取参数的方式，get和post

get多用于请求页面，做查询，刷新

post多于提交内容，做更新

post请求不会在url里显示参数名称和值

get请求会在url里显示参数名称和值

#### POST

前端代码：

```html
<form class="form-inline" method="post" action="/project">
<div class="form-group">
    <label>关键词</label>
    <input type="text" class="form-control" placeholder="输入关键词" name="keyword" maxlength="64">
</div>
<button type="submit" class="btn btn-default">搜索</button>
```

需要注意的是：

1，请求方式是post

2，请求url是相对的路径

3，input标签name的值

后端代码：

```python
if request.method == 'POST':
   keyword = request.form.get('keyword')
   print(keyword)
```

需要注意的是：

1，判断请求方式是不是post

2，取值的方式是 

```
request.form.get('keyword')
```

通过前端input标签的name的值来获取

#### GET

##### 第一种方式

前端代码：

``` html
<li class="list-group-item"><a href="/result?id={{project['id']}}" target="_blank">{{ project['name'] }}</a></li>
```

构造出来的请求如：/result?id=666

需要注意的是：

1，构造的url为相对路径

后端代码：

```python
if request.method=='GET':
   pid = request.args.get('id')
   print(pid)
```

需要注意的是：

1，判断请求方式是不是get

2，取值通过

```python
pid = request.args.get('id')
```

##### 第二种方式

前端代码：

```html
<li class="list-group-item"><a href="/result1/{{project['id']}}" target="_blank">{{ project['name'] }}</a></li>
```

构造出的请求如：/result/666

后端代码：

```python
@app.route('/result1/<pid>',methods=['GET'])
def result1(pid):
   if request.method=='GET':
      # pid = request.args.get('id')
      print(pid)
```

需要注意的是：

1，判断请求方式是不是get

2，获取参数，通过函数参数的方式

3，<>里写的参数名要与函数的参数名相同，才能取到值

### 生成数据模型

参考链接：https://blog.csdn.net/qq_42553568/article/details/90402059

1，安装三方库

```shell
pip install flask-sqlacodegen
```

2，编写命令

```shell
flask-sqlacodegen mysql://root:123456@localhost/spd --outfile models.py --flask
flask-sqlacodegen mysql://root:pwd@127.0.0.1/db_name --tables project --outfile models.py --flask
```

3，运行命令

### URL构建

**url_for()**用于构建特定函数的url

#### 传值

```python
def focus_delete():
	if request.method == 'GET':
		ukid = request.args.get('id')
		wxid = request.args.get('wxid')
		mysql = Mysql()
		mysql.delete_userkey(ukid)
		return redirect(url_for('focus', wxid=wxid, title="订阅"))
```

#### 静态资源

**url_for()**用于加载静态资源，css，js，img等

```html
<script src="{{url_for('static',filename='/js/bootstrap.js')}}">
<link rel="stylesheet" href="{{url_for('static',filename='/css/bootstrap.min.css')}}">
<img src="{{url_for('static',filename='/img/head.png')}}" alt="头像">
```

**直接在url_for中传入static，和文件路径**

### 页面HTML转义

```jinja2
{{project[4]|safe}}
```

project[4]的内容为html代码字符串，如果直接输出，页面直接会显示html代码，不是预期的效果，加上safe。就是说这个内容是安全的，可以直接转义为html。

### 整合Bootstrap4

首先是下载相应版本的文件

这个网址

> https://v4.bootcss.com/docs/getting-started/download/

直接下载

然后下载jquery和popper，这两个是bootstrap4的依赖库，都需要提前引用。

js这样引入

```html
<script src="{{url_for('static',filename='/js/popper.js')}}"></script>
<script src="{{url_for('static',filename='/js/jquery.js')}}"></script>
<script src="{{url_for('static',filename='/js/bootstrap.js')}}"></script>
```

css这样引入

```html
<link rel="stylesheet" href="{{url_for('static',filename='/css/bootstrap.min.css')}}">
```

如果在其他路径，也可以改url_for的参数，道理是一样的。

有一个坑是，popper.js需要下载umd版本的，不然会报错，引入不成功，下面是地址

```
<script src="https://cdn.bootcdn.net/ajax/libs/popper.js/2.5.0/umd/popper.js"></script>
```

可以直接引用，也可以下载到本地，然后相对路径引用，道理相同。

### 列表遍历

flask页面使用的是jinja2，所以需要熟悉jinja2的语法。

#### 常用语法

##### 继承模版

```jinja2
{% extends 'base.html' %}
```

##### 声明块

```jinja2
    {% block main %}
    
    {% endblock main %}
```

这个块可以被继承的子页面复用

使用的时候直接

```jinja2
{% extends 'base.html' %}
{% block main %}
<div class="container">
    <div class="row">
        <h1>1234567890</h1>
    </div>
</div>
{% endblock main %}
```

##### 引用值

后端传值到前端

```python
return render_template('myself.html', user=user, title=title)
```

前端使用

```
{{title}}
```

**双引号加变量名**

##### 遍历列表

前提是你传到页面的内容是一个列表

```jinja2
{% for uk in uks %}
<li class="list-group-item">
<span class="card-title">{{uk[0]}}</span>
</li>
{% endfor %}
```

##### 对列表项进行判断

```
{% if uk[0]|length>1 %}
```

这里对uk[0]做了长度大于1的判断，在使用列表项的值做判断计算的时候，**不需要使用{{}}**