---
title: Django
date: 2019-01-05 22:06:55
updated: 2019-01-05 22:06:55
tags:
categories:
- python
- python模块
---

# 概述
~~~
由于Django 是在快节奏的编辑环境中开发的,设计使得常见 Web 开发任务快速且容易。

数据库模型:
编写django models；下一步，运行Django命令行工具来自动创建数据库表

动态的管理界面：它不只是一个框架 ，而是一所完整的房子:
一旦你的模型定义完毕之后，Django能自动创建一个专业的、可以用于生产环境的 管理界面 – 可以让认证的用户添加、修改和删除对象的一个站点
这里的设计理念是你的网站由一个员工、客户或者可能是你自己去编辑 —— 而你不想仅仅为了管理内容而去创建后台界面。
创建Django应用的一个典型工作流程是创建模型然后尽快地让admin sites启动和运行起来， 这样您的员工（或客户）能够开始录入数据。 
然后，才开发展现数据给公众的方式。

设计你的URLs:
创建一个叫做URLconf的Python模块。这其实是你应用的目录，它包含URL模式与Python回调函数间的一个简单映射。
 URLconfs 还用作从Python代码中解耦URLs。

编写你的视图:
一个视图会根据参数来检索数据、加载一个模板然后使用检索出来的数据渲染模板

设计你的模板:
简单地说，它定义网站的外观
Django有一个模板搜索路径，它让你最小化模板之间的冗余。
最后，Django使用“模板继承”的概念。
~~~

# 开始django
## 创建一个项目
~~~
django-admin startproject piaotiejun_blog

生成的文件结构如下:
piaotiejun_blog
├── piaotiejun_blog
│   ├── __init__.py
│   ├── __init__.pyc
│   ├── settings.py
│   ├── settings.pyc
│   ├── urls.py
│   ├── urls.pyc
│   ├── wsgi.py
│   └── wsgi.pyc
└── manage.py
~~~

## 数据库的建立(mysql)
~~~
先在mysql中创建一个数据库CREATE DATABASE blog;
编辑piaotiejun_blog/piaotiejun_blog/settings.py
DATABASES = { 
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'piaotiejun_blog',
        'USER': 'root',
        'PASSWORD': '1990221',
        'HOST': '127.0.0.1',
        'POST': '3306',
    }   
}
在piaotiejun_blog/目录下执行python manage.py migrate
migrate查看INSTALLED_APPS设置并根据piaotiejun_blog/piaotiejun_blog/settings.py文件中的数据库设置创建任何必要的数据库表
~~~

## 运行app
~~~
python manage.py runserver 0.0.0.0:8000
~~~

## 设置编码时区
~~~
piaotiejun_blog/piaotiejun_blog/settings.py中设置
LANGUAGE_CODE = 'zh-CN'
TIME_ZONE = 'Asia/Shanghai'
~~~

## 创建一个应用
~~~
python manage.py startapp blog

此时的文件目录树为:
piaotiejun_blog
├── blog
│   ├── admin.py
│   ├── __init__.py
│   ├── migrations
│   │   └── __init__.py
│   ├── models.py
│   ├── tests.py
│   └── views.py
├── manage.py
└── piaotiejun_blog
    ├── __init__.py
    ├── __init__.pyc
    ├── settings.py
    ├── settings.pyc
    ├── urls.py
    └── wsgi.py

当编写一个数据库驱动的Web应用时，第一步就是定义该应用的模型 —— 本质上，就是定义该模型所对应的数据库设计及其附带的元数据。
模型指出了数据的唯一、明确的真实来源。 它包含了正在存储的数据的基本字段和行为。 Django遵循DRY (Don't repeat yourself)原则。
这个原则的目标是在一个地方定义你的数据模型，并从它自动获得需要的信息。意思是在一个地方设计数据库，其余的数据库相关的内容都充这个地方自动获取。
迁移工具也符合以上哲学 —— 这不同于Ruby On Rails中的迁移；例如，迁移完全依照于你的模型文件且本质上只是一个历史记录，
Django通过这个历史记录更新你的数据库模式使它与你现在的模型文件保持一致。

将应用blog加入到django中:
Django 应用是可以“热插拔”的，即可以在多个项目中使用同一个应用，也可以分发这些应用， 因为它们不需要与某个特定的Django安装绑定
piaotiejun_blog/piaotiejun_blog/settings.py中
INSTALLED_APPS = (
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'blog',
)

编写model:
修改piaotiejun_blog/blog/model.py为以下代码

from django.db import models

class Category(models.Model):
    name = models.CharField(max_length=200)
    pub_date = models.DateTimeField('date published')
    article_cnt = models.IntegerField(default=0)

class Article(models.Model):
    auther = models.ForeignKey(Category)
    name = models.CharField(max_length=200)
    content = models.TextField(max_length=10240, default='')
    pub_date = models.DateTimeField('date published')
    read_cnt = models.IntegerField(default=0)
    star_cnt = models.IntegerField(default=0)

激活模型:
python manage.py makemigrations blog
通过运行makemigrations告诉Django，已经对模型做了一些更改（在这个例子中，你创建了一个新的模型）并且会将这些更改存储为迁移文件，此时数据库尚未更改

python manage.py sqlmigrate blog 0001 查看将在数据库中执行的sql

python manage.py check 它会检查你的项目中的模型是否存在问题 只是检查迁移记录和项目模型是否存在问题

python manage.py migrate 在数据库执行以上sql migrate命令会找出所有还没有被应用的迁移文件，
Django使用数据库中一个叫做django_migrations的特殊表来追踪哪些迁移文件已经被应用过，
并且在你的数据库上运行它们 —— 本质上来讲，就是使你的数据库模式和你改动后的模型进行同步。
~~~

## Django自动生成的管理界面
~~~
为你的员工或者客户生成用于添加、修改和删除内容的管理性站点是一件单调乏味、缺乏创造力的工作。
 为此，Django会根据你写的模型文件完全自动地生成管理界面。
Django是在新闻编辑室这样的环境中被开发出来的，这样的环境中“内容发布者”站点和“公共”站点有着非常明显的界限。 
网站管理者使用管理界面来添加新闻故事、新闻事件、体育比赛分数等，这些内容会被展示在公共站点上。
Django解决了为网站管理者创建统一的管理界面用以编辑内容这个问题。
管理界面不是让访问网站的人使用的，它服务于网站管理者。 它用于网站的管理员。

创建一个管理员用户：
python manage.py createsuperuser

登陆管理界面
http://127.0.0.1:8000/admin/ 进入管理界面

让blog应用在管理站点中可编辑:
管理站点的首页面上没有blog，修改piaotiejun_blog/blog/admin.py
from django.contrib import admin
from .models import Category, Article

admin.site.register(Category)
admin.site.register(Article)

修改页面中表单的排列顺序:
修改piaotiejun_blog/blog/admin.py
from django.contrib import admin
from .models import Category, Article

class CategoryAdmin(admin.ModelAdmin):
    fields = ['name', 'article_cnt', 'pub_date']

class ArticleAdmin(admin.ModelAdmin):
    fieldsets = [ 
            ('basic', {'fields': ['category', 'name']}), 
            ('content', {'fields': ['content']}),
            ('info', {'fields': ['pub_date', 'read_cnt', 'star_cnt']})
    ]   

admin.site.register(Category, CategoryAdmin)
admin.site.register(Article, ArticleAdmin)

自定义项目的模板：
编辑piaotiejun_blog/piaotiejun_blog/settings.py
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [os.path.join(BASE_DIR, 'templates')],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]
DIRS 是加载Django模板时检查的一个文件系统目录列表；它是一个搜索路径，把模板放在项目目录下会是一个值得提倡的、应该遵循的约定

在piaotiejun_blog/目录下创建templates/admin文件夹
在piaotiejun_blog/目录下执行cp /home/piaotiejun/workspace/python_workspace/django_test/venc/local/lib/python2.7/site-packages/django/contrib/admin/templates/admin/base_site.html templates/admin
修改base_site.html即可修改admin首页的内容

关于模板搜索算法的例子。该例子下 TEMPLATES 的配置是:
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [
            '/home/html/example.com',
            '/home/html/default',
        ],
    },
    {
        'BACKEND': 'django.template.backends.jinja2.Jinja2',
        'DIRS': [
            '/home/html/jinja2',
        ],
    },
]
如果你调用函数get_template('story_detail.html'), Django按以下顺序查找story_detail.html：
/home/html/example.com/story_detail.html ('django' engine)
/home/html/default/story_detail.html ('django' engine)
/home/html/jinja2/story_detail.html ('jinja2' engine)
~~~

## 编写对外的客户查看的页面视图
~~~
视图是Django应用中的一“类”网页，它通常使用一个特定的函数提供服务，并且具有一个特定的模板。

编辑piaotiejun_blog/blog/views.py,代码如下:
from django.http import HttpResponse

from .models import Article, Category

def index(request):
    all_category = Category.objects.all()
    output = '\n'.join(c.name for c in all_category)

    return HttpResponse(output)


设置路由:
1.创建piaotiejun_blog/blog/urls.py,代码如下:
from django.conf.urls import url

from . import views

urlpatterns = [
    url(r'^$', views.index, name='index'),
]
2.让主URLconf可以链接到blog.urls模块，编辑piaotiejun_blog/piaotiejun_blog/urls.py
from django.conf.urls import include, url
from django.contrib import admin

urlpatterns = [
    url(r'^blog/', include('blog.urls')),
    url(r'^admin/', include(admin.site.urls)),
]
~~~

