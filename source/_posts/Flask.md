---
title: Flask
date: 2019-01-05 22:06:55
updated: 2019-01-05 22:06:55
tags:
categories:
- python
- python模块
---

# 学习链接
[Flask-SocketIO documentation](http://flask-socketio.readthedocs.org/en/latest/)
[Flask-SQLAlchemy — Flask-SQLAlchemy 0.16 documentation](http://docs.jinkan.org/docs/flask-sqlalchemy/index.html)
[欢迎使用 Flask — Flask 0.10.1 documentation](http://www.pythondoc.com/flask/)
[Flask-SQLAlchemy 学习 » InBi's Blog](http://www.itwhy.org/%E6%95%B0%E6%8D%AE%E5%BA%93/flask-sqlalchemy-%E5%AD%A6%E4%B9%A0.html)
[Flask-SocketIO documentation](http://flask-socketio.readthedocs.org/en/latest/)
[Flask 的 Context 机制](https://blog.tonyseek.com/post/the-context-mechanism-of-flask/)

# Flask before request
before_request，请求前的预处理
蓝图也可以添加before_request，效果发生在app.before_request后，同理after_request
## 代码样例
~~~
@project.before_request
def droject_design():
    print 'before blueprint'
~~~

# Flask-cache使用memcached作为缓存后端
~~~
from flask import Flask
from flask.ext.cache import Cache
from werkzeug.serving import run_simple

class Development(object):
    MEMCACHED_HOST = '127.0.0.1:11211'
    MEMCACHED_USER = 'piaotiejun'
    MEMCACHED_PWD = 'password'
    CACHE_TYPE = 'memcached'

app = Flask(__name__)
app.config.from_object(Development)
cache = Cache(app)

@app.route("/")
@cache.cached(timeout=50)
def index():
    return 'hello world!'

if __name__ == '__main___':
    run_simple('0.0.0.0', 8000, app, use_reloader=True, use_debugger=True)
~~~

# Flask-cache安装与简单示例
flask-cache可以缓存flask视图函数和非视图函数，用以提高程序的性能及提高响应速度。
## 安装
~~~
pip install Flask-Cache
~~~

## 代码样例
~~~
from flask import Flask
from flask.ext.cache import Cache
from werkzeug.serving import run_simple

app = Flask(__name__)
cache = Cache(config={'CACHE_TYPE': 'simple'})
cache.init_app(app)

@app.route("/")
@cache.cached(timeout=50) # 要注意不要将这两个装饰器顺序弄反
def index():
        return 'hello world!'

if __name__ == '__main___':
    run_simple('0.0.0.0', 8000, app, use_reloader=True, use_debugger=True)
~~~

# Flask-cache缓存带参数的flask视图函数
flask-cache默认缓存的key为request.path，如果想缓存带参数的flask视图就会发生错误
(不同的参数获取的结果都是第一个参数缓存的结果)，解决方法是写一个新类继承Cache,重写cached方法，
使cache的默认缓存key变为request.path字符串与参数的字符串和的MD5，
这样同一个视图函数在不同参数下缓存的key就会不同，示例代码如下:
执行这段代码访问 [http://0.0.0.0:10001/?name=变换这个参数 http://0.0.0.0:10001/?name=变换这个参数] 参看终端中输出的cnt是否增加
~~~
#coding=utf8

import json
import functools
import hashlib

from flask import Flask
from flask.ext.cache import Cache
from flask import request, current_app
from werkzeug.serving import run_simple

cnt = 0

class CacheWarp(Cache):
    """
    处理带参数的flask视图缓存
    """
    def cached(self, timeout=None, key_prefix='view/%s', unless=None):
        def decorator(f):
            @functools.wraps(f)
            def decorated_function(*args, **kwargs):
                #: Bypass the cache entirely.
                if callable(unless) and unless() is True:
                    return f(*args, **kwargs)

                try:
                    cache_key = decorated_function.make_cache_key(*args, **kwargs)
                    print(cache_key)
                    rv = self.cache.get(cache_key)
                except Exception:
                    if current_app.debug:
                        raise
                    return f(*args, **kwargs)

                if rv is None:
                    rv = f(*args, **kwargs)
                    try:
                        self.cache.set(cache_key, rv, 
                                   timeout=decorated_function.cache_timeout)
                    except Exception:
                        if current_app.debug:
                            raise
                        return f(*args, **kwargs)
                return rv

            def make_cache_key(*args, **kwargs):
                if callable(key_prefix):
                    cache_key = key_prefix()
                elif '%s' in key_prefix:
                    args = (json.dumps(request.args) if request.args else '') or (json.dumps(request.form) if request.form else '') or ''
                    cache_key = key_prefix % request.path + args
                    cache_key = hashlib.md5(cache_key).hexdigest()
                else:
                    cache_key = key_prefix

                return cache_key

            decorated_function.uncached = f
            decorated_function.cache_timeout = timeout
            decorated_function.make_cache_key = make_cache_key

            return decorated_function
        return decorator

app = Flask(__name__)
cache = CacheWarp(config={'CACHE_TYPE': 'simple'})
cache.init_app(app)

@app.route("/", methods=['GET'])
@cache.cached(timeout=50)
def index():
    global cnt
    cnt += 1
    print(cnt)
    name = request.args.get('name')
    return 'hello world!%s'%name

if __name__ == '__main__':
    run_simple('0.0.0.0', 10001, app, use_reloader=True, use_debugger=True)
~~~