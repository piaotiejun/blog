---
title: Sikuli
date: 2019-01-05 22:06:55
updated: 2019-01-05 22:06:55
tags:
categories:
- python
- python模块
---

# 简介
~~~
sikuli是一个自动化测试的工具 可以使用截图的方式模拟操作
Sikuli是一个使用“视觉图像匹配”方法来自动化图形用户界面（GUI）的工具
~~~

# 相关链接
[sikuli官网](http://www.sikuli.org/)
[官网quickstart](http://www.sikulix.com/quickstart.html)
[官网tutorial](http://doc.sikuli.org/tutorials/index.html)
[Documentation](http://sikulix-2014.readthedocs.org/en/latest/index.html)
[api](http://sikulix-2014.readthedocs.org/en/latest/genindex.html)
[问答](https://answers.launchpad.net/sikuli)
[api-详细](http://nightly.sikuli.de/docs/index.html)

# sikuli安装
~~~
官网上的方式是下载安装文件，执行安装文件，这个安装文件会逐个下载sikuli所所需的库，
然后安装，因为有的库需要翻墙所有安装文件执行会阻塞,所以我的做法是下载完所有的库文件然后手动安装

步骤如下：
1.创建一个空文件夹作为sikuli的安装目录
2.下载安装文件 https://launchpad.net/sikuli/sikulix/1.1.0/+download/sikulixsetup-1.1.0.jar 复制到安装目录
3. 下载库文件
https://launchpad.net/raiman/sikulix2013+/1.1.0/+download/sikulixsetupIDE-1.1.0-forsetup.jar
https://launchpad.net/raiman/sikulix2013+/1.1.0/+download/sikulixsetupAPI-1.1.0-forsetup.jar
https://launchpad.net/raiman/sikulix2013+/1.1.0/+download/sikulixlibswin-1.1.0.jar
https://launchpad.net/raiman/sikulix2013+/1.1.0/+download/sikulixlibsmac-1.1.0.jar
https://launchpad.net/raiman/sikulix2013+/1.1.0/+download/sikulixlibslux-1.1.0.jar
http://repo1.maven.org/maven2/org/python/jython-standalone/2.7.0/jython-standalone-2.7.0.jar
http://repo1.maven.org/maven2/org/python/jython-standalone/2.5.4-rc1/jython-standalone-2.5.4-rc1.jar
http://repo1.maven.org/maven2/org/jruby/jruby-complete/1.7.22/jruby-complete-1.7.22.jar
http://tesseract-ocr.googlecode.com/files/tesseract-ocr-3.02.eng.tar.gz #需要翻墙
4.在安装目录下创建一个文件夹Downloads 将步骤3下载到的文件下复制到Downloads下
5.执行sikulixsetup-1.1.0.jar
~~~

# Python中调用sikuli
## 前置
~~~
sikuli是通过jython来进行脚本驱动，一些cpython的类库无法使用，因此需要让Python也可以驱动sikuli，驱动的方式是jpype
去jpype下载安装包然后 python setup.py install （这一步在虚拟机中出错 最好下载exe安装） 
在windows下需要安装VCForPython27
可以通过jd-gui.exe(轻量级反编译java代码)很方便查看Java类库sikuli-script.jar
~~~
## 简单示例 python代码
~~~
#coding=utf8

import os, shutil

from jpype import *
jvmPath = getDefaultJVMPath()
startJVM(jvmPath , '-ea', r'-Djava.class.path=D:\sikuli\sikulixapi.jar')  #启动java虚拟机，加载第三方类库 
#我的path中设置了java配置 又配置了JAVA_HOME 出错 删除JAVA_HOME解决该问题
Screen = JClass('org.sikuli.script.Screen') # org.sikuli.script.Screen这个具体去看jd-gui.exe打开sikulixapi.jar的数据
screen = Screen()
cap = screen.capture() # 抓屏
shutil.move(cap.getFilename(), os.path.join(os.path.abspath('.'), "some-name.png")) # 保存文件
shutDownJVM()
~~~
## sikuli对象获取
~~~
Region = JClass('org.sikuli.script.Region')
App = JClass('org.sikuli.script.App')
Location = JClass('org.sikuli.script.Location')
Button = JClass('org.sikuli.script.Button')
Pattern = JClass('org.sikuli.script.Pattern')
Key = JClass('org.sikuli.script.Key')
Mouse = JClass('org.sikuli.script.Mouse')
KeyModifier= JClass('org.sikuli.script.KeyModifier')
Finder = JClass('org.sikuli.script.Finder')
Match = JClass('org.sikuli.script.Match')
Screen = JClass('org.sikuli.script.Screen')
~~~
## 打开应用
~~~
#coding=utf8

from jpype import *
jvmPath = getDefaultJVMPath()
startJVM(jvmPath , '-ea', r'-Djava.class.path=D:\sikuli\sikulixapi.jar')  #启动java虚拟机，加载第三方类库
App = JClass('org.sikuli.script.App')
app = App(r'C:\Users\Dell\Desktop\piaotiejun\menghuanxiyou\my.exe')
app.open()
~~~
## 聚焦到梦幻西游
~~~
#coding=utf8

from jpype import *

jvmPath = getDefaultJVMPath()
startJVM(jvmPath , '-ea', r'-Djava.class.path=D:\sikuli\sikulixapi.jar')  #启动java虚拟机，加载第三方类库
App = JClass('org.sikuli.script.App')

app = App('my.exe')
app.open()
app = App('ONLINE')
app.open()
click('开始') # 点击开始图标
app = App('ONLINE') # 知道游戏id的话 参数为游戏id区别不同的app
app.open() # 聚焦到梦幻西游
#app.focus() 不生效
~~~

# 梦幻西游游戏中的实践
## 清除屏幕
~~~
def clear(location):
    """清除屏幕
    :param location: Location obj
    """
    if location:
        click(location)
        wait(2)
    keyDown(Key.CMD)
    type('h')
    keyUp(Key.CMD)
    type(Key.F9)
~~~
## 打开物品栏
~~~
def open_staff(reg=None, img=None):
    """
    打开物品栏
    :param reg: 物品栏整个区域
    :param img: 物品栏的人物头像
    """
    if not reg or reg.exists(img):
        return
    keyDown(Key.CMD)
    type('e')
    keyUp(Key.CMD)
~~~
## 鼠标漂移问题处理
~~~
def move_to():
    """
    http://www.2344.com/play/26577.htm
    http://bbs.anjian.com/showtopic.aspx?topicid=566788&forumpage=3&page=1 鼠标漂移
    1、先用MoveTo 移动到指定坐标
    2、在游戏中利用找图或者找色命令，找到游戏鼠标特征，从而得出鼠标当前位置
    3、计算指定坐标和鼠标当前位置的差值
    4、使用相对移动命令移动鼠标
    """
    click(Location(373, 79))
    person = find(Pattern("1461526963456.png").similar(0.80)).getCenter()
    mouseMove(person)
    print(person)
    mouse = Region(199,88,639,479).find(Pattern("1461534161137.png").similar(0.5)).getCenter()
    offx = person.x - mouse.x
    offy = person.y - mouse.y   
    mouseMove(offx, offy)
~~~
## 操作
~~~
#coding=UTF8

from jpype import *
import os
import time

jvmPath = getDefaultJVMPath()
startJVM(jvmPath , '-ea', r'-Djava.class.path=D:\sikuli\sikulixapi.jar')  # 启动java虚拟机，加载第三方类库
# startJVM(jvmPath , '-ea', r'-Djava.class.path=D:\sikuli\sikulix.jar')  # 启动java虚拟机，加载第三方类库
Region = JClass('org.sikuli.script.Region')
App = JClass('org.sikuli.script.App')
Location = JClass('org.sikuli.script.Location')
Button = JClass('org.sikuli.script.Button')
Pattern = JClass('org.sikuli.script.Pattern')
Key = JClass('org.sikuli.script.Key')
Mouse = JClass('org.sikuli.script.Mouse')
KeyModifier = JClass('org.sikuli.script.KeyModifier')
Finder = JClass('org.sikuli.script.Finder')
Match = JClass('org.sikuli.script.Match')
Screen = JClass('org.sikuli.script.Screen')
Image = JClass('org.sikuli.script.Image')
Env = JClass('org.sikuli.script.Env')

Settings = JClass('org.sikuli.basics.Settings')
HotkeyListener = JClass('org.sikuli.basics.HotkeyListener')
HotkeyEvent = JClass('org.sikuli.basics.HotkeyEvent')
HotkeyManager = JClass('org.sikuli.basics.HotkeyManager')


class menghuanApp(object):
    """
    :brief: 梦幻西游窗口类 640*480
    """
    def __init__(self, region, app, ids):
        self.region = region
        self.x = region.x
        self.y = region.y
        self.w = region.w
        self.h = region.h
        self.app = app
        self.id = ids
        self.secure_region = Region(self.x+50, self.y+50, 50, 50)
        self.status_region = Region(self.x+300, self.y+480, 350, 25)
        self.attact_icon = Region(self.x+300, self.y+480, 27, 25)
        self.inventory_icon = Region(self.x+327, self.y+480, 27, 25)
        """self.attact_region = Region(self.x+354, self.y+480, 27, 25)
        self.attact_region = Region(self.x+381, self.y+480, 27, 25)
        self.attact_region = Region(self.x+408, self.y+480, 27, 25)
        self.attact_region = Region(self.x+435, self.y+480, 27, 25)
        self.attact_region = Region(self.x+462, self.y+480, 27, 25)
        self.attact_region = Region(self.x+489, self.y+480, 27, 25)
        self.attact_region = Region(self.x+516, self.y+480, 27, 25)
        self.attact_region = Region(self.x+543, self.y+480, 27, 25)
        self.attact_region = Region(self.x+570, self.y+480, 27, 25)
        self.attact_region = Region(self.x+597, self.y+480, 27, 25)
        self.attact_region = Region(self.x+624, self.y+480, 27, 25)"""


class imagePoll(object):
    """
    :brief: 图片字典
    """
    def __init__(self, img_path):
        self.dic = {}
        for img in os.listdir(img_path):
            if img.rsplit('.', 1)[-1] != 'png':
                continue
            img_abs = os.path.join(img_path, img)
            setattr(self, img.split('.')[0], Pattern(img_abs))


img_poll = imagePoll('D:\sikuli\workapsce\menghuanshouyou.sikuli')
screen = Screen()


def enter_name_passwrd(region):
    # 输入账号、密码
    time.sleep(2)
    uname_region = region.find(img_poll.uname).getCenter()
    uname_region.offset(100, 0).click()
    time.sleep(1)
    for i in range(30):
        region.type(Key.DELETE)
    time.sleep(1)
    region.type('woyuchengfeng001')
    time.sleep(1)
    region.type('2', KeyModifier.KEY_SHIFT)
    time.sleep(1)
    region.type('126.com')
    region.type(Key.ENTER)

    time.sleep(2)
    region.find(img_poll.pwd).getCenter().offset(100, 0).click()
    time.sleep(1)
    for i in range(3):
        region.type(Key.DELETE)
    time.sleep(1)
    region.type('1990221')
    time.sleep(1)
    region.type(Key.ENTER)

    time.sleep(5)
    confirm = region.exists(img_poll.confirm)
    if confirm:
        confirm.click()
        return False

    return True


def load(path):
    # 打开梦幻西游程序
    old_path = os.path.abspath('.')
    os.chdir(path)
    app = App('my.exe')
    app.open()
    os.chdir(old_path)

    # 等待启动1 桌面程序出现
    if not screen.exists(img_poll.loading, 360.0):  # 必须是浮点数
        return False
    screen.find(Pattern(img_poll.loading)).click()

    # 等待启动2
    if not screen.exists(img_poll.enter, 360.0):
            return False
    region = App.focusedWindow()  # 获取当前窗体坐标
    app = App('ONLINE')  # 知道游戏id的话 参数为游戏id区别不同的app
    app.open()
    region.find(Pattern(img_poll.enter)).click()

    # 等待启动3
    if not region.exists(img_poll.next_step1, 360.0):
            return False
    app = App('ONLINE')  # 知道游戏id的话 参数为游戏id区别不同的app
    app.open()
    region.find(Pattern(img_poll.next_step1)).click()

    # 等待启动4
    if not region.exists(img_poll.next_step1, 360.0):
            return False
    app = App('ONLINE')  # 知道游戏id的话 参数为游戏id区别不同的app
    app.open()
    region.find(Pattern(img_poll.next_step1)).click()

    # 设置为账号密码登陆
    time.sleep(5)
    login = region.exists(img_poll.code_login)
    if not login:
        login = region.exists(img_poll.password_login)
        login.click()

    while True:
        if not enter_name_passwrd(region):
            time.sleep(3)
        else:
            break
    region.click(Location(region.getCenter().x, region.getCenter().y-80))

    return region, app


def clear_screen(app):
    app.region.type('h', KeyModifier.ALT)
    time.sleep(1)
    region.type(Key.F9)


def mouse_move_to(app, location):
    # TODO location处于边缘则特殊处理
    Mouse.move(location)
    x = location.x-100 if location.x-100 >= 0 else 0
    y = location.y-100 if location.y-100 >= 0 else 0
    temp_region = Region(x, y, 200, 200)
    m = Pattern(img_poll.mouse).similar(0.6)
    mouse_img_location = temp_region.find(m).getCenter()

    offx = location.x - mouse_img_location.x + 15
    offy = location.y - mouse_img_location.y + 15
    region.mouseMove(offx, offy)

    return Mouse.at()


def open_inventory(app):
    while True:
        temp = app.region.exists(img_poll.inventory_flag)
        if temp:
            return Region(temp.x, temp.y+temp.h, 280, 220)
        app.region.type('e', KeyModifier.ALT)


def close_inventory(app):
    """
    while True:
        temp = app.region.exists(img_poll.inventory_flag)
        if not temp:
            return
        else:
            app.region.type('e', KeyModifier.ALT)"""
    app.region.type('e', KeyModifier.ALT)


def use_item(app, item_name):
    region = open_inventory(app)
    location = region.find(getattr(img_poll, item_name)).getCenter()
    mouse_move_to(app, location).rightClick()
    time.sleep(3)
    close_inventory(app)


def ful_hp(app):
    mouse_move_to(app, Location(app.region.x+550, app.region.y+50))
    app.region.mouseMove(50, -13)
    Mouse.at().rightClick()
    app.region.mouseMove(0, 8)
    Mouse.at().rightClick()
    app.region.mouseMove(-100, 0)
    Mouse.at().rightClick()
    app.region.mouseMove(0, -8)
    Mouse.at().rightClick()
    #mouse_move_to(app, Location(app.region.x+610, app.region.y+35)).rightClick()
    #mouse_move_to(app, Location(app.region.x+610, app.region.y+45)).rightClick()



def goto_biaoju(app):
    clear_screen(app)
    use_item(app, 'blue_flag')
    # location = Region(app.region.x+400, app.region.y+100, 150,150).find(img_poll.biaoju).getCenter()
    location = app.region.find(img_poll.biaoju).getCenter()
    mouse_move_to(app, location).click()
    app.region.wait(img_poll.biaoju_flag)

    mouse_move_to(menghuan_app, Location(550, 100)).click()
    time.sleep(5)
    mouse_move_to(menghuan_app, Location(580, 220)).click()
    time.sleep(5)
    mouse_move_to(menghuan_app, Location(550, 220)).click()
    time.sleep(5)
    clear_screen(app)

    mouse_move_to(app, app.region.find(img_poll.zhengbiaoshi).getCenter()).click()
    time.sleep(3)
    mouse_move_to(app, Location(app.region.x+180, app.region.y+390)).click()
    time.sleep(3)
    mouse_move_to(app, Location(app.region.x+180, app.region.y+325)).click()


def test_app():
    global region
    global app
    region, app = Region(0, 0, 650, 510), App('ONLINE')
    app.open()

if __name__ == '__main__':
    import flask
    # region, app = load('D:\menghuanxiyou')  # 测试通过
    test_app()
    menghuan_app = menghuanApp(region, app, '26859558')
    #mouse_move_to(menghuan_app, Location(552, 53))
    time.sleep(2)
    #goto_biaoju(menghuan_app)
    ful_hp(menghuan_app)
~~~
## 偷卡1
~~~
def mouse_move_to(location):
    # TODO location处于边缘则特殊处理
    Mouse.move(location)
    x = location.x-100 if location.x-100 >= 0 else 0
    y = location.y-100 if location.y-100 >= 0 else 0
    temp_region = Region(x, y, 200, 200)
    #m = Pattern("1461797705947.png").similar(0.9)
    #m = Pattern("m.png").similar(0.9)

    m = Pattern("1461795242910.png").similar(0.6)
    mouse_img_location = temp_region.find(m).getCenter()

    offx = location.x - mouse_img_location.x + 15
    offy = location.y - mouse_img_location.y + 15
    mouseMove(offx, offy)

    return Mouse.at()

cnt = 0

def full():
    print('------------full')
    type(Key.F3)
    mouse_move_to(Location(369, 411)).click()
    wait(3)

def fight():
    global cnt
    cnt += 1
    type('q', Key.CMD)
    type('q', Key.CMD)
    wait(5)
    if exists("1461799387526.png"):
        if -1 < cnt%30 < 3:
            full()
    if exists("1461795647251.png"):
        fight()
  

def walk():
    from random import randint    
    wait(1)
    type(Key.TAB)
    hover(Location(400+randint(1, 250), 180+randint(1, 200)))
    Mouse.at().click()
    wait(3)
    type(Key.TAB)

    if exists("1461799387526.png"):
        walk()

click(Location(585, 65))

while True:
        
    if exists("1461795647251.png"):
        fight()
    if exists("1461799387526.png"):
        walk()
~~~
## 偷卡1
~~~
Region(268,56,642,502) 
Settings.Mouse = 0.1
wuyi = "wuyi.png"
wuyi = "wuyi.png"
way_flag = Pattern("way_flag.png").similar(0.90)
fight_flag = "fight_flag.png"

def mouse_move_to(location):
    # TODO location处于边缘则特殊处理
    Mouse.move(location)
    x = location.x-100 if location.x-100 >= 0 else 0
    y = location.y-100 if location.y-100 >= 0 else 0
    temp_region = Region(x, y, 200, 200)
    #m = Pattern("1461797705947.png").similar(0.90)
    #m = Pattern("m.png").similar(0.90)

    m = Pattern("1461795242910.png").similar(0.60)
    mouse_img_location = temp_region.find(m).getCenter()

    offx = location.x - mouse_img_location.x + 15
    offy = location.y - mouse_img_location.y + 15
    mouseMove(offx, offy)

    return Mouse.at()

cnt = 1

def fights():
    if not exists(way_flag):
        print('________________fight')
        type('q', Key.CMD)
        type('q', Key.CMD)
        wait(15)
        if not exists(way_flag):    
            fight()


def full():
    print('------------full')
    type(Key.F3)
    mouse_move_to(Location(369, 411)).click()
    wait(3)

    while True:
        type(Key.TAB)
        wait(1)
        #mouse_move_to(Location(535, 185)).click()
        mouse_move_to(Location(535, 246)).click()
        wait(1)
        type(Key.TAB)
        if not exists(way_flag):
            fights()
        global wuyi
        a = exists(wuyi)
        if a:
            wait(5)
            a = exists(wuyi)
            if a:
                mouse_move_to(a.getCenter()).click()
                mouse_move_to(Location(451, 416)).click()
                wait(1)
                mouse_move_to(Location(451, 416)).click()
                break
                        
def fight():
    if not exists(way_flag):
        print('________________fight')
        global cnt
        print cnt
        cnt += 1
        type('q', Key.CMD)
        type('q', Key.CMD)
        wait(15)
        if not exists(way_flag):    
            fight()


def walk():
    while True:
        wait(1)
        from random import randint
        if cnt%30 == 0:
            full()
        type(Key.TAB)
        wait(1)
        hover(Location(400+randint(1, 250), 180+randint(1, 200)))
        #hover(mouse_move_to(Location(753, 167)))
        #hover(mouse_move_to())
        Mouse.at().click()
        wait(0.5)
        type(Key.TAB)
        for i in range(5):
            if not exists(way_flag):
                fight()
                break    
            else:
                wait(1)

if __name__ == '__main__':
    click(Location(585, 65))
    walk()
    #full()
~~~


















