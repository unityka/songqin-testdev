# Python 作业 9



```python


要求大家用面向对象的设计编写一个python程序，实现一个文字游戏系统。

动物园里面有10个房间，房间号从1 到 10。

每个房间里面可能是体重200斤的老虎或者体重100斤的羊。
游戏开始后，系统随机在10个房间中放入老虎或者羊。

然后随机给出房间号，要求游戏者选择敲门还是喂食。

如果选择喂食：
喂老虎应该输入单词 meat，喂羊应该输入单词 grass
喂对了，体重加10斤。 喂错了，体重减少10斤

如果选择敲门：
敲房间的门，里面的动物会叫，老虎叫会显示 ‘Wow !!’,羊叫会显示 ‘mie~~’。 动物每叫一次体重减5斤。


游戏者强记每个房间的动物是什么，以便不需要敲门就可以喂正确的食物。 
游戏3分钟结束后，显示每个房间的动物和它们的体重。


实现过程中，有什么问题，请通过课堂上讲解的调试方法，尽量自己发现错误原因。
```


## 参考答案，往下翻
<br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br>

#### 单线程版
```python
# coding=utf8
from random import randint
import time
class Tiger():
    classname = 'tiger'

    def __init__(self,weigth=200):
        self.weight = weigth


    def roar(self):
        print 'wow!!'
        self.weight -= 5

    def feed(self,food):
        if food == 'meat':
            self.weight += 10
            print 'ok, +10'
        else:
            self.weight -= 10
            print 'bad, -10'



class Sheep():
    classname = 'Sheep'

    def __init__(self,weigth=100):
        self.weight = weigth


    def roar(self):
        print 'mie~~'
        self.weight -= 5

    def feed(self,food):
        if food == 'grass':
            self.weight += 10
            print 'ok, +10'
        else:
            self.weight -= 10
            print 'bad, -10'


class Room:
    def  __init__(self,num,animal):
        self.num = num
        self.animal = animal

#  房间列表。里面有10个房间
rooms = []
for no in range(10):
    if randint(0,1) == 0:
        ani = Tiger()
    else:
        ani = Sheep()

    rooms.append(Room(no+1,ani))


gameStartTime = time.time()
while True:
    roomno = randint(0,9)
    room = rooms[roomno]
    print u'现在来到房间 #%s, 敲门(y)还是喂食？' % room.num

    ch = raw_input('')
    if ch == 'y':
        room.animal.roar()
    else:
        print u'请喂食'
        food = raw_input()
        room.animal.feed(food)

    curTime = time.time()
    if curTime - gameStartTime  > 180 :
        print '******* game over *********'
        break

for room in rooms:
    print 'room:%s, %s, %s' % (room.num, room.animal.classname,room.animal.weight)
```

#### 多线程中文版，以后学过多线程后再看
```python
# coding=utf8
from random import  randint
import time,os
import threading


class Tiger:
    classname = 'tiger'
    def __init__(self):
        self.weight = 200

    def roar(self):
        print 'wow!!!'
        self.weight -= 5

    def feed(self,food):
        if food == 'meat':
            self.weight += 10
            print 'good, weight + 10'
        else :
            self.weight -= 10
            print 'bad, weight - 10'

class Sheep:
    classname = 'sheep'
    def __init__(self):
        self.weight = 100

    def roar(self):
        print 'mie~~'
        self.weight -= 5

    def feed(self,food):
        if food == 'grass':
            self.weight += 10
            print 'good, weight + 10'
        else :
            self.weight -= 10
            print 'bad, weight - 10'\


class Room:
    def __init__(self,num,animal):
        self.num = num
        self.animal = animal

rooms = []

for no in range(10):
    if randint(0,1) == 0:
        ani = Tiger()
    else:
        ani = Sheep()

    room = Room(no+1,ani)
    rooms.append(room)


def count_thread():
    # 记录下游戏开始时间
    startTime   = time.time()
    while True:
        time.sleep(0.1)
        curTime = time.time()
        if (curTime - startTime) > 20:
            break

    print u'游戏结束'
    for room in rooms:
        print u'房间%s, 里面是%s,体重%s' % (room.num,
                               room.animal.classname,
                                             room.animal.weight)
    os._exit(0)

t = threading.Thread(target=count_thread)
t.start()


# 循环做如下事情
while True:
    # 提示房间号，让用户选择 敲门 还是 喂食
    curRoomIdx = randint(0,9)
    room = rooms[curRoomIdx]
    print u'当前来到房间%s,敲门【q】还是喂食【w】'% room.num
    ch = raw_input()
    # 如果选择敲门:......
    if ch == 'q':
        room.animal.roar()
    # 如果选择喂食:......
    elif ch == 'w':
        print u'请输入食物:'
        food = raw_input()
        room.animal.feed(food)



```
