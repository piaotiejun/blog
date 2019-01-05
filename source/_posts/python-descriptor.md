---
title: python descriptor
date: 2019-01-05 22:06:55
updated: 2019-01-05 22:06:55
tags:
categories:
- python
- python buildin
---


## 作用: 在类上拦截对实例属性的访问
~~~
>>> class MyInt(int): 
...     def square(self): 
...             return self*self 
...      
>>> n = MyInt(2) 
>>> n.name = 'two' 
>>> n.square() 
4 
>>> n.name 
'two' 

n.square和n.name分别在几个对象的__dict__中查找'square'或'name'？ 
答案是2和4。n.square需要查找MyInt和n，n.name需要查找MyInt, int, object和n，查找顺序就如我列出来这样。

2是因为现在类中找到了square 然后在实例中找square，因为在实例中没找到square所以用的是类中找到的square，函数是非数据描述器
4是因为在类中没找到name 类的父类中也没找到name 然后才在实例中找到了name，类的普通数据成员不是描述器
~~~


## 参考
1. [如何理解 Python 的 Descriptor？ - Python 3.x - 知乎](http://www.zhihu.com/question/25391709)

2. [python描述符（descriptor）解密 - 极客范 - GeekFan.net](http://www.geekfan.net/7862/)

3. [Python描述器引导(翻译) — 一起写Python文章，一起看Python文章](http://pyzh.readthedocs.org/en/latest/Descriptor-HOW-TO-Guide.html)

4. [Descriptor HowTo Guide — Python 2.7.11 documentation](https://docs.python.org/2/howto/descriptor.html)

5. [Python duck typing | Zhiwei Li](http://zhiwei.li/text/2015/08/16/python-duck-typing/)

<div class="wtitle" id="wen">
<h2>深入解析Python中的descriptor描述器的作用及用法</h2>
</div>
<div class="con">
在Python中描述器也被称为描述符,描述器能够实现对对象属性的访问控制,下面我们就来深入解析Python中的descriptor描述器的作用及用法
</div>

<div class="con" id="content">
<p>一般来说，一个描述器是一个有“绑定行为”的对象属性(object attribute)，它的访问控制被描述器协议方法重写。这些方法是 __get__(), __set__(), 和 __delete__() 。有这些方法的对象叫做描述器。</p>
<p>默认对属性的访问控制是从对象的字典里面(__dict__)中获取(get), 设置(set)和删除(delete)它。举例来说， a.x 的查找顺序是, a.__dict__['x'] , 然后 type(a).__dict__['x'] , 然后找 type(a) 的父类(不包括元类(metaclass)).如果查找到的值是一个描述器, Python就会调用描述器的方法来重写默认的控制行为。这个重写发生在这个查找环节的哪里取决于定义了哪个描述器方法。注意, 只有在新式类中时描述器才会起作用。(新式类是继承自 type 或者 object 的类)</p>
<p>描述器是强大的，应用广泛的。描述器正是属性, 实例方法, 静态方法, 类方法和 super 的背后的实现机制。描述器在Python自身中广泛使用，以实现Python 2.2中引入的新式类。描述器简化了底层的C代码，并为Python的日常编程提供了一套灵活的新工具。</p>
<p><strong>描述器协议</strong></p>
<div class="jb51code">
<pre class="brush:py;">
descr.__get__(self, obj, type=None) --&gt; value
descr.__get__(self, obj, value) --&gt; None
descr.__delete__(self, obj) --&gt; None

</pre>
</div>
<p>一个对象如果是一个描述器，被当做对象属性(很重要)时重写默认的查找行为。</p>
<p>如果一个对象同时定义了__get__和__set__，它叫data descriptor。仅定义了__get__的描述器叫non-data descriptor。</p>
<p>data descriptor和non-data descriptor区别在于: 相对于实例的字典的优先级，如果实例字典有与描述器具同名的属性，如果描述器是data descriptor，优先使用data descriptor。如果是non-data descriptor，优先使用字典中的属性。</p>
<div class="jb51code">
<pre class="brush:py;">
class B(object):

  def __init__(self):
    self.name = 'mink'

  def __get__(self, obj, objtype=None):
    return self.name

class A(object):
  name = B()

a = A()
print a.__dict__  # print {}
print a.name    # print mink
a.name = 'kk'    
print a.__dict__  # print {'name': 'kk'}
print a.name    # print kk

</pre>
</div>
<p>这里B是一个non-data descriptor所以当a.name = 'kk'的时候，a.__dict__里会有name属性, 接下来给它设置__set__</p>
<div class="jb51code">
<pre class="brush:py;">
def __set__(self, obj, value):
  self.name = value

 ... do something

a = A()
print a.__dict__  # print {}
print a.name    # print mink
a.name = 'kk'    
print a.__dict__  # print {}
print a.name    # print kk

</pre>
</div>
<p>因为data descriptor访问属性优先级比实例的字典高，所以a.__dict__是空的。</p>
<p><strong>描述器的调用<br />
</strong>描述器可以直接这么调用： d.__get__(obj)</p>
<p>然而更常见的情况是描述器在属性访问时被自动调用。举例来说， obj.d 会在 obj 的字典中找 d ,如果 d 定义了 __get__ 方法，那么 d.__get__(obj) 会依据下面的优先规则被调用。</p>
<p>调用的细节取决于 obj 是一个类还是一个实例。另外，描述器只对于新式对象和新式类才起作用。继承于 object 的类叫做新式类。</p>
<p>对于对象来讲，方法 object.__getattribute__() 把 b.x 变成 type(b).__dict__['x'].__get__(b, type(b)) 。具体实现是依据这样的优先顺序：资料描述器优先于实例变量，实例变量优先于非资料描述器，__getattr__()方法(如果对象中包含的话)具有最低的优先级。完整的C语言实现可以在 Objects/object.c 中 PyObject_GenericGetAttr() 查看。</p>
<p>方法 object.__getattribute__()是每个新式类都有的方法(旧式类没有),   详细解释在[https://docs.python.org/2/reference/datamodel.html?highlight=__getattribute__#object.__getattribute__  python官网]</p>
<p>对于类来讲，方法 type.__getattribute__() 把 B.x 变成 B.__dict__['x'].__get__(None, B) 。用Python来描述就是:</p>
<div class="jb51code">
<pre class="brush:py;">
def __getattribute__(self, key):
  "Emulate type_getattro() in Objects/typeobject.c"
  v = object.__getattribute__(self, key)
  if hasattr(v, '__get__'):
    return v.__get__(None, self)
  return v
</pre>
</div>
<p>其中重要的几点：</p>
<ul>
  <li>描述器的调用是因为 __getattribute__()</li>
  <li>重写 __getattribute__() 方法会阻止正常的描述器调用</li>
  <li>__getattribute__() 只对新式类的实例可用</li>
  <li>object.__getattribute__() 和 type.__getattribute__() 对 __get__() 的调用不一样</li>
  <li>资料描述器总是比实例字典优先。</li>
  <li>非资料描述器可能被实例字典重写。(非资料描述器不如实例字典优先)</li>
  <li>super() 返回的对象同样有一个定制的 __getattribute__() 方法用来调用描述器。调用 super(B, obj).m() 时会先在 obj.__class__.__mro__ 中查找与B紧邻的基类A，然后返回 A.__dict__['m'].__get__(obj, A) 。如果不是描述器，原样返回 m 。如果实例字典中找不到 m ，会回溯继续调用 object.__getattribute__() 查找。(译者注：即在 __mro__ 中的下一个基类中查找)</li>
</ul>
<p><span style="color: #ff0000">注意:</span>在Python 2.2中，如果 m 是一个描述器, super(B, obj).m() 只会调用方法 __get__() 。在Python 2.3中，非资料描述器(除非是个旧式类)也会被调用。 super_getattro() 的实现细节在： Objects/typeobject.c ，[del] 一个等价的Python实现在 Guido’s Tutorial [/del] (译者注：原文此句已删除，保留供大家参考)。</p>
<p>以上展示了描述器的机理是在 object, type, 和 super 的 __getattribute__() 方法中实现的。由 object 派生出的类自动的继承这个机理，或者它们有个有类似机理的元类。同样，可以重写类的 __getattribute__() 方法来关闭这个类的描述器行为。</p>
<p><strong>描述器例子<br />
</strong>下面的代码中定义了一个资料描述器，每次 get 和 set 都会打印一条消息。重写 __getattribute__() 是另一个可以使所有属性拥有这个行为的方法。但是，描述器在监视特定属性的时候是很有用的。</p>
<div class="jb51code">
<pre class="brush:py;">
class RevealAccess(object):
  """A data descriptor that sets and returns values
    normally and prints a message logging their access.
  """

  def __init__(self, initval=None, name='var'):
    self.val = initval
    self.name = name

  def __get__(self, obj, objtype):
    print 'Retrieving', self.name
    return self.val

  def __set__(self, obj, val):
    print 'Updating' , self.name
    self.val = val

&gt;&gt;&gt; class MyClass(object):
  x = RevealAccess(10, 'var "x"')
  y = 5

&gt;&gt;&gt; m = MyClass()
&gt;&gt;&gt; m.x
Retrieving var "x"
10
&gt;&gt;&gt; m.x = 20
Updating var "x"
&gt;&gt;&gt; m.x
Retrieving var "x"
20
&gt;&gt;&gt; m.y
5

</pre>
</div>
<p>这个协议非常简单，并且提供了令人激动的可能。一些用途实在是太普遍以致于它们被打包成独立的函数。像属性(property), 方法(bound和unbound method), 静态方法和类方法都是基于描述器协议的。</p>
</div>

<p><span>转载自  http://m.jb51.net/show/87455</span></p>

