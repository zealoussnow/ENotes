### 一、Django的安装

### 二、项目和应用创建

创建项目

	django-admin.py startproject demo

创建应用

	django-admin.py startapp blog

修改配置文件：settings.py

	添加应用----》blog
	修改时区

修改urls.py
	url(r'blog/index/$', 'blog.views.index')

处理blog/views.py

增加视图处理方法：

```python
	from django.http import HttpReponse
	def index(req):
		return HttpResponse('<h1>Helo,World!</h1>')
```

运行应用

	python manage.py runserver

访问的url：127.0.0.1:8000/blog/index


### 三、url配置的基本使用

参数传递：

位置参数

关键字参数