---
title: WSGI
date: 2019-01-05 22:06:55
updated: 2019-01-05 22:06:55
tags:
categories:
- python
- python规范与协议
---


1. [WSGI — WSGI.org](http://wsgi.readthedocs.org/en/latest/)

2. [WSGI 简介](http://blog.csdn.net/on_1y/article/details/18803563)

/usr/lib/python2.7/wsgiref/handlers.py 中的run方法
~~~
def run(self, application):
    """Invoke the application"""
    # Note to self: don't move the close()!  Asynchronous servers shouldn't
    # call close() from finish_response(), so if you close() anywhere but
    # the double-error branch here, you'll break asynchronous servers by
    # prematurely closing.  Async servers must return from 'run()' without
    # closing if there might still be output to iterate over.
    try:
        self.setup_environ()
        self.result = application(self.environ, self.start_response)
        self.finish_response()
    except:
        try:
            self.handle_error()
        except:
            # If we get an error handling an error, just give up already!
            self.close()
            raise   # ...and let the actual server figure it out.
~~~


/usr/lib/python2.7/wsgiref/handlers.py 中的start_response方法
该方法中设置了status、headers信息(即wsgi服务器获取到了http header和status信息)，然后写入response中
~~~
def start_response(self, status, headers,exc_info=None):
    """'start_response()' callable as specified by PEP 333"""

    if exc_info:
        try:
            if self.headers_sent:
                # Re-raise original exception if headers sent
                raise exc_info[0], exc_info[1], exc_info[2]
        finally:
            exc_info = None        # avoid dangling circular ref
    elif self.headers is not None:
        raise AssertionError("Headers already set!")

    assert type(status) is StringType,"Status must be a string"
    assert len(status)>=4,"Status must be at least 4 characters"
    assert int(status[:3]),"Status message must begin w/3-digit code"
    assert status[3]==" ", "Status message must have a space after code"
    if __debug__:
        for name,val in headers:
            assert type(name) is StringType,"Header names must be strings"
            assert type(val) is StringType,"Header values must be strings"
            assert not is_hop_by_hop(name),"Hop-by-hop headers not allowed"
    self.status = status
    self.headers = self.headers_class(headers)
    return self.writ
~~~

/usr/lib/python2.7/wsgiref/handlers.py 中的finish_response方法
该方法获取到wsgi app返回的result，然后写入response中
~~~
def finish_response(self):
    """Send any iterable data, then close self and the iterable

    Subclasses intended for use in asynchronous servers will
    want to redefine this method, such that it sets up callbacks
    in the event loop to iterate over the data, and to call
    'self.close()' once the response is finished.
    """
    try:
        if not self.result_is_file() or not self.sendfile():
            for data in self.result:
                self.write(data)
            self.finish_content()
    finally:
        self.close()
~~~

bottle中的__call__方法
~~~
def wsgi(self, environ, start_response):
    """ The bottle WSGI-interface. """
    try:
        out = self._cast(self._handle(environ))
        # rfc2616 section 4.3
        if response._status_code in (100, 101, 204, 304)\
        or environ['REQUEST_METHOD'] == 'HEAD':
            if hasattr(out, 'close'): out.close()
            out = [] 
        start_response(response._status_line, response.headerlist)
        return out
    except (KeyboardInterrupt, SystemExit, MemoryError):
        raise
    except Exception:
        if not self.catchall: raise
        err = '<h1>Critical error while processing request: %s</h1>' \
              % html_escape(environ.get('PATH_INFO', '/'))
        environ['wsgi.errors'].write(err)
        headers = [('Content-Type', 'text/html; charset=UTF-8')]
        start_response('500 INTERNAL SERVER ERROR', headers, sys.exc_info())
        return [tob(err)]
~~~

