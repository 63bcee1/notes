Flask开发笔记

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