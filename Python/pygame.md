[TOC]

# pygame

## pygame库的安装

![1565937280479](E:\Typora笔记\Python\assets\1565937280479.png)

## 创建一个游戏窗口

```python
import pygame
import sys
#初始化游戏，为使用硬件设备做准备
pygame.init()

#创建一个屏幕对象
screen = pygame.display.set_mode((600,400))

#设置游戏窗口标题
pygame.display.set_caption("初次见面")

#开始游戏的主循环
while True:
    #监控用户的事件
    for event in pygame.event.get():
        #检测是否点击窗口的X按钮
        if event.type == pygame.QUIT:
            #注销pygame库
            pygame.quit()
            #退出程序
            sys.exit()
            
    #更新一下画面
    pygame.display.flip()
```

- 首先导入了pygame和sys模块。模块pygame包含开发游戏所需的功能所以需要导入，导入sys模块是为了调用exit（）方法退出游戏
- 初始化pygame，pygame是一个package包含了各种功能的模块，这一步实际做一些硬件的初始化工作，让pygame能正确的工作
- 调用pygame.display.set_mode()方法创建一个游戏界面screen，注意必填参数是一个元组/列表类型数据，600，400表示游戏窗口的宽、高的像素，这个方法返回一个surface对象，在pygame中surface对象就表示一个2D图像，我们创建的游戏窗口也是一个图像但标题栏和边框属于surface
- 调用pygame.display.set_caption()方法为窗口设置一个标题，传入字符串即可
- 这里使用while True死循环是为了让程序可以一直运行
- 这个for循环是一个事件循环，事件就是用户玩游戏时执行的操作，例如移动鼠标、点击鼠标、按键盘等操作。使用pygame.event.get()捕获所有事件，再将事件逐一取出并赋值给event变量
- 使用if语句来检测特定的事件，并根据发生的事件执行相应的任务，上例表示当检测到用户点击窗口的x按钮就退出程序
- pygame.quit()与pygame.init()作用相反，它将注销pygame库，程序应该在调用sys.exit()前调用pygame.quit()
- 调用sys模块的exit（）方法退出程序
- 调用pygame.display.flip()让刚才绘制的内容显示在屏幕，每次执行while循环时都会绘制一个空屏幕，并擦去旧屏幕，使得新屏幕可见。例如我们移动游戏元素时，pygame.display.flip()将不断的更新屏幕，以显示元素的新位置，并在原来的位置上隐藏元素，从而营造平滑移动的效果

游戏状态可理解为程序中所有变量的值的集合，例如在飞机大战游戏中游戏状态包括飞机是否存活、位置等变量，如果飞机受到伤害，生命值将减少，我们称这些均为为游戏状态改变。游戏状态通常随事件的发生而改变，例如鼠标点击、键盘输入、或者时间的流失。游戏循环在一秒中执行多次来不停的检查是否有新事件产生，并且会根据事件来更新游戏状态，这通常叫做事件处理。

## 为游戏窗口设置颜色

pygame默认创建的黑色屏幕，这样太乏味了，

### 设置背景色

将游戏窗口设置为绿色

```python
import pygame
import sys

pygame.init()
screen = pygame.display.set_mode((600,400))
pygame.display.set_caption("初次见面")

#设置一个颜色RGB
bg = (0,255,0)

while True:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()
    #用背景色填充屏幕
    screen.fill(bg)
    
    pygame.display.flip()
```

- RGB颜色值(0,255,0)
- surface对象（窗口图像）调用fill()方法用填充背景色，这个方法只接收一个参数：一种颜色

![1565962504298](E:\Typora笔记\Python\assets\1565962504298.png)

## 屏幕上绘图

### 画一条直线

在绿色游戏屏幕上的指定位置画一条红色的只想

```python
import pygame
import sys

pygame.init()

screen = pygame.display.set_mode([600,400])

pygame.display.set_caption("显示一条直线")

bg_color = (0,255,0)

while True:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()
            
    screen.fill(bg_color)
    #在屏幕上绘制一条直线
    pygame.draw.line(screen,(255,0,0),(0,200),(600,200),6)
    pygame.display.flip()
```

调用pygame.draw.line()方法在屏幕上绘制直线，

第一个参数是surface对象，screen表示画在窗口上；

第二个参数是线条颜色，（255，0，0）表示红色；

第三个第四个参数是开始和结束的坐标（元组类型数据），像素坐标原点在左上方（0，0），（0，200）表示向右0像素，向下200像素的位置，（600，200）表示向右600像素，向下200像素的位置；

第五个可选参数是线条宽度（单位像素）默认为1；

![1565963728560](E:\Typora笔记\Python\assets\1565963728560.png)

### 画一个矩形

在屏幕上绘制一个矩形，我们需要先了解一下pygame中矩形对象(left,top,width,height)。下图标记两个矩形的四个角所在像素坐标(x,y)。其中绿色的矩形可以用一个四元素的元组(0,0,600,400)或者两个而元素的元组((0,0),(600,400))来表示，前两个数为左上坐标，后两位为矩形的宽高。所以黄色矩形对象并不是(100,150,400,300)而是(100,150,300,150)

![1565964060429](E:\Typora笔记\Python\assets\1565964060429.png)

```python
import pygame
import sys

pygame.init()

screen = pygame.display.set_mode([600,400])
pygame.display.set_caption("显示一个矩形")

bg_color = (0,255,0)

while True:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()
            
    screen.fill(bg_color)
    #在屏幕上绘制一个矩形
    pygame.draw.rect(screen,(255,255,0),(100,150,300,150),0)
    pygame.display.flip()
```

使用pygame.draw.rect()来创建一个矩形。

第一个参数是surface对象，"screen"表示画在窗口上

第二个参数是矩形的填充颜色(255,255,0)表示黄色

第三个参数是绘制的rect对象(100,150,300,150)用来定义矩形的位置和宽高

第四个可选参数是矩形边框的宽度，默认值0

![1565964379290](E:\Typora笔记\Python\assets\1565964379290.png)

## 设置背景图

```python
import pygame
import sys

pygame.init()
screen = pygame.display.set_mode((600,400))
pygame.display.set_caption("设置背景图")

#加载当前目录下的绿地.png
background = pygame.image.load("绿地.png")

while True:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()
            
    screen.blit(background,(0,0))
    
    pygame.display.flip()
```

调用pygame.image.load()方法加载一张本地图片，返回的也是一个surface对象，在本地练习时需要在脚本所在目录下保存一张图片作为背景图

surface对象调用blit()方法是将一个（背景）图像画在另一个（窗口）图像之上，第一个参数是要画的图像，第二个参数要画在什么位置，（0，0）表示从坐上角原点开始画

![1566002849834](E:\Typora笔记\Python\assets\1566002849834.png)

## 在指定位置显示图片

```python
import pygame
import sys 

pygame.init()
screen = pygame.display.set_mode((600,400))
pygame.display.set_caption("显示小蟒蛇")
#蓝色
bg = (0,0,255)

logo = pygame.image.load("logo_lofi.png")

#获取logo图片的矩形对象
logo_rect = logo.get_rect()

#获取游戏窗口的矩形对象
screen_rect = screen .get_rect()

#设置logo图片的位置在游戏窗口的中心
logo_rect.center = screen_rect.center

while True:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()
            
    screen.fill(bg)        
    #指定位置绘制Logo图片
    screen.blit(logo,logo_rect)
    pygame.display.flip()
```

- surface对象调用get_rect()方法获取Logo图像的位置，返回<reect(0,0,202,80)>加载的图像默认都从左上角开始显示
- 窗口的surface对象也调用get_rect()方法获取游戏窗口图像的位置，返回<rect(0,0,600,400)>
- 矩形对象都有center属性，表示图像的中心点，通过修改logo图像的这个属性，将logo图像的中心点设置成游戏窗口的中心点，从而将logo图像位置移动到窗口中心，这时候logo图像的位置变为<rect(199,160,202,80)>

![1566003543299](E:\Typora笔记\Python\assets\1566003543299.png)

- surface对象调用blit()将logo图像绘制在游戏窗口上。之前我们学习blit()方法第二个参数传入(0,0)从左上角开始控制。这章我们学习第二个参数传入一个矩形对象，取矩形对象的左上角的点(199,160)作为要开始画的地方，这与矩形对象的宽高(202,80)无关

![1566003734545](E:\Typora笔记\Python\assets\1566003734545.png)

## 移动图像

```python
import pygame
import sys

pygame.init()
bg = (255,255,255)
size = width,height = 600,400

screen = pygame.display.set_mode(size)
pygame.display.set_caption("初次见面")
logo = pygame.image.load("logo_lofi.png")
logo_rect = logo.get_rect()

#设置图像每次移动的举例，[2,2]表示向右移动2个像素，向下移动2个像素
speed = [2,2]

#创建一个时钟用来控制帧率
clock = pygame.time.Clock()

while True:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()
    #移动图像
    logo_rect = logo_rect.move(speed)
    
    #绘制窗口的景色
    screen.fill(bg)
    screen.blit(logo,logo_rect)
    pygame.display.filp()
    
    #设置每秒循环30次，也就是每33ms更新一次界面
    clock.tick(30)
```

- speed 列表存储每次循环时图像移动的像素值，第一个数是左/右移动的像素值，正数向右移动，负数向左移动，第二个数是上/下移动的像素值，正数向下移动，负数向上移动，例如[-2,1]就表示向左移动2个像素，向下移动1个像素

![1566009768939](E:\Typora笔记\Python\assets\1566009768939.png)

- 使用pygame.time.Clock()控制帧率，这就像一个定时器在控制时间进程，指出”现在开始下一个循环“。在使用pygame时钟之前，必须先创建Clock对象的一个实例clock = pygame.time.Clock()。如果我们不控制帧率，运行程序后小蟒蛇将会瞬间移出了窗口之外
- 矩形对象调用move()方法用来移动图像，返回移动后新的矩形对象
- 每次循环时都把整个窗口重新填充成白色，会覆盖之前绘制的图像
- 在主循环中使用clock对象的tick()方法设置游戏绘制的最大帧率，注意传入clock.tick()的数不是一个毫秒数而是每秒内循环要运行的次数，所以这个循环应当每秒运行30次，在这里只是说应当运行，因为循环只能按计算机能够保证的速度运行，



## 限制移动的范围

```python
import pygame
import sys

pygame.init()
bg = (255,255,255)
size = width,height = 600,400

screen = pygame.display.set_mode(size)
pygame.display.set_caption("初次见面")
logo = pygame.image.load("logo_lofi.png")
logo_rect = logo.get_rect()

#设置图像每次移动的举例，[2,2]表示向右移动2个像素，向下移动2个像素
speed = [2,2]

#创建一个时钟用来控制帧率
clock = pygame.time.Clock()

while True:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()
    #移动图像
    logo_rect = logo_rect.move(speed)
    
    #判断图像是否超出左边距或右边距
    if logo_rect.left<0 or logo_rect.right>width:
        #如果超出则反向运动
        speed[0]=-speed[0]
    #判断图像是否超出上边或下边
    if logo_rect.top<0 or logo_rect.bottom>height:
        #如果超出则反向运动
        speed[1]=-speed[1]
    
    #绘制窗口的景色
    screen.fill(bg)
    screen.blit(logo,logo_rect)
    pygame.display.filp()
    
    #设置每秒循环30次，也就是每33ms更新一次界面
    clock.tick(30)
```

- 判断边距时需要用到宽高，所以单独使用两个变量保存
- 矩形对象有left,right,top,bottom属性，使用if语句判断logo图像向左移动的位置是否小于0或者向右移动的位置是否大于窗口宽度
- 如果logo图像移动出左/右边缘，通过把移动像素的值进行负数处理，就可以更改反方向移动
- 使用if语句判断logo图像向上移动的位置是否小于0或者向下移动的位置是否大于窗口的高度
- 如果logo图像移动出了屏幕上/下边缘，通过把移动像素的值，进行负数处理，就可以更改为反方向移动

## 翻转图像

```
import pygame
import sys

pygame.init()
bg = (255,255,255)
size = width,height = 600,400

screen = pygame.display.set_mode(size)
pygame.display.set_caption("初次见面")
logo = pygame.image.load("logo_lofi.png")
logo_rect = logo.get_rect()

#设置图像每次移动的举例，[2,2]表示向右移动2个像素，向下移动2个像素
speed = [2,2]

#创建一个时钟用来控制帧率
clock = pygame.time.Clock()

while True:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()
    #移动图像
    logo_rect = logo_rect.move(speed)
    
    #判断图像是否超出左边距或右边距
    if logo_rect.left<0 or logo_rect.right>width:
        #水平翻转图像
        logo = pygame.transform.flip(logo,True,False)
        speed[0]=-speed[0]
        
    #判断图像是否超出上边或下边
    if logo_rect.top<0 or logo_rect.bottom>height:
        #垂直翻转图像
        logo = pygame.transform.flip(logo,False,True)
        speed[1]=-speed[1]
    
    #绘制窗口的景色
    screen.fill(bg)
    screen.blit(logo,logo_rect)
    pygame.display.filp()
    
    #设置每秒循环30次，也就是每33ms更新一次界面
    clock.tick(30)
```

- 使用pygame.transform.flip()方法可以翻转图像，这个方法第一个参数要翻转的图像（surface对象），第二个参数表示是否水平翻转（True/False），第三个参数表示是否垂直翻转
- 将logo图像的水平翻转，把pygame.transform.flip()方法第二个参数设置为True
- 将logo图像的垂直翻转，把pygame.transform.flip()方法第三个参数设置为True

## python增加键盘事件

### 获取用户事件

游戏运行期间，用户会不时地做一些操作，如按键、在窗口中移动鼠标，这时，一个pygame.event.Event对象将产生，此对象定义在pygame.event模块中，可以使用pygame.event.get()函数来捕获这些事件，这将返回一个pygame.event.Event对象的列表。这个列表中包含了pygame.event.Event函数调用后的所有事件。如果没有事件产生，pygame.event.get将返回一个空的列表

我们可以获取用户的所有事件，再根据事件的类型分别进行处理。下面我们编写一段代码，把程序运行期间产生的所有事件都在控制台输出：

```python
import pygame
import sys

pygame.init()
size = width,height = 600,400
screen = pygame.display.set_mode(size)
pygame.display.set_caption("键盘事件")

while True:
    for event in pygame.event.get():
        #向屏幕输出事件的字符串内容
        print(str(event))
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()
```

- 这个示例不需要背景和图像，因此删减了所有不必要的代码，比较简洁
- 将pygame获取的每一个事件转化为字符串类型输出到控制台

运行代码，在窗口上移动鼠标、点击鼠标、按键等操作，控制台会不断的输出事件的字符串，每一行都是一个事件

![1566023859804](E:\Typora笔记\Python\assets\1566023859804.png)

#### 鼠标移动产生的事件

![1566023899187](E:\Typora笔记\Python\assets\1566023899187.png)

#### 第一行是pygame被激活或隐藏产生的事件，第二行是按下关闭按钮的事件

![1566023974015](E:\Typora笔记\Python\assets\1566023974015.png)

#### 键盘被按下或松开的产生的事件

![1566024003761](E:\Typora笔记\Python\assets\1566024003761.png)

#### 鼠标按键被按下或松开产生的事件

![1566024035921](E:\Typora笔记\Python\assets\1566024035921.png)

### 常用事件及其属性

| 事件            | 含义                     | 属性            |
| --------------- | ------------------------ | --------------- |
| QUIT            | 按下关闭按钮             | none            |
| ATIVEEVENT      | pygame被激活或隐藏       | gain,state      |
| KEYDOWN         | 键盘按键被按下           | unicode,key,mod |
| KEYUP           | 键盘按键被松开           | key,mod         |
| MOUSEMOTION     | 鼠标移动                 | pos,rel,buttons |
| MOUSEBUTTONDOWN | 鼠标按键被按下           | pos,button      |
| MOUSEBUTTONUP   | 鼠标按键被松开           | pos,button      |
| JOYAXISMOTION   | 游戏手柄上的摇杆移动     | joy,axis,value  |
| JOYBALLMOTION   | 游戏手柄上的轨迹球滚动   | joy,axis,value  |
| JOYHATMOTION    | 游戏手柄上的帽子开关移动 | joy,axis,value  |
| JOYBUTTONDOWN   | 游戏手柄按钮被按下       | joy,button      |
| JOYBUTTONUP     | 游戏手柄按钮被松开       | joy,button      |
| VIDEORESIZE     | 用户调整窗口尺寸         | size,w,h        |
| VIDEOEXPOSE     | 部分窗口需要重新绘制     | none            |
| USEREVENT       | 用户定义的事件           | code            |

### 按键控制图像移动（一）

```python
import pygame
import sys

pygame.init()

bg = (255,255,255)
size = width,height = 600,400
screen = pygame.display.set_mode(size)
pygame.display.set_caption("键盘事件")
logo = pygame.image.load("logo_lofi.png")
logo_rect = logo.get_rect()

#设置图像的方向
r_head = logo
l_head = pygame.transform.flip(logo,True,False)

clock = pygame.time.Clock()
while True:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()
            
        #监控键盘按键被按下的事件
        if event.type == pygame.KEYDOWN:
            #按向上箭头，图像向上移动40像素
            if event.key == pygame.K_UP:
                speed = [0,-40]
                logo_rect = logo_rect.move(speed)
            #按向下箭头，图像向下移动40像素
            if event.key == pygame.K_DOWN:
                speed = [0,40]
                logo_rect = logo_rect.move(speed)
            #按向左箭头，蛇头向左移动40个像素
            if event.key == pygame.K_LEFT:
                logo = l_head
                speed = [-40,0]
                logo_rect = logo_rect.move(speed)
            #按向右箭头，蛇头向右移动40个像素
            if event.key == pygame.K_RIGHT:
                logo = r_head
                speed = [40,0]
                logo_rect = logo_rect.move(speed)
                
       	#图像左侧位置超出窗口左侧不能继续移动
    	if logo_rect.left < 0:
            logo_rect.left = 0
            
        #图像左侧位置超出窗口右侧不能继续移动
    	if logo_rect.right < width:
            logo_rect.right = width
            
        #图像左侧位置超出窗口左侧不能继续移动
    	if logo_rect.top < 0:
            logo_rect.top = 0
            
        #图像左侧位置超出窗口左侧不能继续移动
    	if logo_rect.bottom < height:
            logo_rect.bottom = height
       	
        screen.fill(bg)
        screen.blit(logo,logo_rect)
        pygame.display.flip()
        clock.tick(30)
     
```

- 以上代码删减了小蟒蛇自动移动的相关代码
- 为了让小蟒蛇向左移动时蛇头在左边，向右移动时蛇头在右边，所以设置了两个变量保存图像方向，因为小蟒蛇默认情况头在右，所以需要把头在左的图像进行水平翻转
- 在所有的事件的循环中增加对”键盘按键被按下“事件的监控
- 如果事件的key值等于”K_UP“（上箭头的Key ASCII），图像向上移动40像素，
- 判断图像的左/右位置是否超出窗口的左/右边缘，超出则将图像固定在窗口左/右边缘的位置

### 按键控制图像移动（二）

通过修改rect对象top,bottom,left,right属性值来移动图像，实现原理：例如判断向上移动时，直接修改rect.top属性值等于移动前rect.top加上移动速度

```python
import pygame
import sys
#导入Pygame提供的常量
from pygame.locals import *

pygame.init()

bg=(255,255,255)
size = width,height = 600,400

screen = pygame.display.set_mode(size)
pygame.display.set_caption("键盘事件")
logo = pygame.image.load("logo_lofi.png")
logo_rect = logo.get_rect()

r_head = logo
l_head = pygame.transform.flip(logo,True,False)

#设置图像移动速度
speed = 40

clock = pygame.time.Clock()

while True:
    for event in pygame.event.get():
        if event.type ==QUIT:
            pygame.quit()
            sys.exit()
            
        if event.type == KEYDOWN:
            if event.key == K_UP:
                #判断图像顶部位置是否到达窗口顶部，更新图像顶部位置
                if logo_rect.top > 0:
                    logo_rect.top = logo_rect.top - speed
                else:
                    logo_rect.top = 0
            
            if event.key == K_DOWN:
                #判断图像底部位置是否到达窗口底部，更新图像底部位置
                if logo_rect.bottom<height:
                    logo_rect.bottom = logo_rect.bottom+speed
                else:
                    logo_rect.bottom = height
                
            if event.key == K_LEFT:
                logo = l_head
                #判断图像左侧位置是否到达窗口左侧，更新图形左侧的位置
                if logo_rect.left > 0:
                    logo_rect.left = logo_rect.left - speed
                else:
                    logo_rect.left = 0
                    
            if event.key == K_RIGHT:
                logo=r_head
                #判断图像右侧位置是否到达窗口右侧，更新图像右侧的位置
                if logo_rect.right < width:
                    logo_rect.right = logo_rect.right + speed
                else:
                    logo_rect.right=width
                    
        screen.fill(bg)
        screen.blit(logo,logo_rect)
        pygame.display.flip()
        clock.tick(30)
```

- pygame.locals包含了许多Pygame提供的常量，from pygame.locals import * 使我们不用在常量名称前添加pygame前缀，如用QUIT来替代pygame.QUIT
- 定义一个变量保存图像移动速度
- 如果图像顶部位置未到达窗口顶部，则更新图像向上移动的rect.top值，如果到达窗口顶部，把图像固定在窗口顶部的位置

## pygame精灵

在pygame中精灵可以认为是一个个小图片，一种可以在屏幕上移动的图形对象，并且可以与其他图形对象交互，精灵图像可以是使用pygame绘制函数绘制的图像，也可以是本地的图像文件，在pygame.sprite模块里面包含了一个名为Sprite类，它是pygame本身自带的一个精灵

### 创建三个精灵

```python
import pygame
from pygame.locals import *
import sys,random

pygame.init()

size = width,height = 600,400
bg = (255,255,255)

#使用列表保存三个精灵左上角的位置
location_group = ([100,100],[300,100],[500,100])

screen = pygame.display.set_mode(size)
pygame.display.set_caption("动画精灵")

#定义一个类，继承sprite的基类
class Apple(pygame.sprite.Sprite):
    def __init__(self,position):
        #调用基类的init方法，完成初始化
        pygame.sprite.Sprite.__init__(self)
        #显示本地的精灵图像
        self.image = pygame.image.load("apple.png")
        #获取精灵的矩阵对象
        self.rect = self.image.get_rect()
        #设置精灵的位置
        self.rect.topleft=position
        
#创建一个空的精灵列表
Applegroup = []

#实例化三个精灵,添加到精灵组中
for p in location_group:
    Applegroup.append(Apple(p))
    
while True:
    for event in pygame.event.get():
        if event.type==QUIT:
            pygame.quit()
            sys.exit()
            
    screen.fill(bg)
    
    #绘制这三个精灵
    for apple in Applegroup:
        screen.blit(apple.image,apple.rect)
        
    pygame.display.flip()
```

- 在pygame.sprite模块里面包含了一个名为Sprite类，它是pygame本身自带的一个精灵，但是这个类的功能比较少，因此我们新建一个类对其继承，在sprite类的基础上丰富，以便我们的使用
- 调用基类的init（）方法，完成初始化
- self.image是sprite中主要且常用的变量，负责显示什么，pygame.image.load("apple.png")显示本地的apple.png图像
- self.rect是sprite中主要且常用的变量，负责设定显示的位置，一般需要先用self.rect = self.image.get_rect()获得图像的矩形大小
- self.rect.topleft来设定某一个角的显示位置，也可以用self.rect.top等，分别表示上下左右
- 创建一个空列表，用来存放精灵
- 实例化三个精灵，添加到精灵列表中
- 依次在屏幕上绘制这三个精灵图像

![1566216185717](E:\Typora笔记\Python\assets\1566216185717.png)

### 使用精灵组来实现多个图像

pygame还提供了精灵组，它很适合处理一组精灵，有添加，移除，绘制，更新等方法

下面我们修改一下前面的代码，只需要两行：把列表改为精灵组

```python
import pygame
from pygame.locals import *
import sys,random

pygame.init()

size = width,height = 600,400
bg = (255,255,255)

#使用列表保存三个精灵左上角的位置
location_group = ([100,100],[300,100],[500,100])

screen = pygame.display.set_mode(size)
pygame.display.set_caption("动画精灵")

#定义一个类，继承sprite的基类
class Apple(pygame.sprite.Sprite):
    def __init__(self,position):
        #调用基类的init方法，完成初始化
        pygame.sprite.Sprite.__init__(self)
        #显示本地的精灵图像
        self.image = pygame.image.load("apple.png")
        #获取精灵的矩阵对象
        self.rect = self.image.get_rect()
        #设置精灵的位置
        self.rect.topleft=position
        
#创建一个组
Applegroup = pygame.sprite.Group()

#实例化三个精灵,添加到精灵组中
for p in location_group:
    Applegroup.add(Apple(p))
    
while True:
    for event in pygame.event.get():
        if event.type==QUIT:
            pygame.quit()
            sys.exit()
            
    screen.fill(bg)
    
    #绘制这三个精灵
    for apple in Applegroup:
        screen.blit(apple.image,apple.rect)
        
    pygame.display.flip()
```

- 使用pygame.sprite.Group()方法创建一个精灵组
- 调用精灵组的add()方法添加精灵到精灵组中，精灵组还有remove()、kill()、alive()等方法
- 运行代码，和上一个效果完全一样，使用方法不同

### 让精灵动起来

让上面三个小苹果以不同的速度向下降落，并且当到达窗口底部时，再从顶部出现一直循环

```python
import pygame
from pygame.locals import *
import sys,random

pygame.init()

size = width,height = 600,400
bg = (255,255,255)

#使用列表保存三个精灵左上角的位置
location_group = ([100,100],[300,100],[500,100])

screen = pygame.display.set_mode(size)
pygame.display.set_caption("动画精灵")

#定义一个类，继承sprite的基类
class Apple(pygame.sprite.Sprite):
    def __init__(self,position):
        #调用基类的init方法，完成初始化
        pygame.sprite.Sprite.__init__(self)
        #显示本地的精灵图像
        self.image = pygame.image.load("apple.png")
        #获取精灵的矩阵对象
        self.rect = self.image.get_rect()
        #设置精灵的位置
        self.rect.topleft=position
        #设置精灵的移动速度
        self.speed=[0,random.randint(1,10)]
        
    #为精灵增加一个向下移动的方法
    def move(self):
        self.rect = self.rect.move(self.speed)
        #当苹果的到达窗口底部时，让苹果从上面出来
        if self.rect.top>height:
            self.rect.bottom = 0
        
#创建一个组
Applegroup = pygame.sprite.Group()

#实例化三个精灵,添加到精灵组中
for p in location_group:
    Applegroup.add(Apple(p))
    
clock = pygame.time.Clock()
    
while True:
    for event in pygame.event.get():
        if event.type==QUIT:
            pygame.quit()
            sys.exit()
            
    screen.fill(bg)
    
    #绘制这三个精灵
    for apple in Applegroup:
        #调用精灵的move方法
        apple.move()
        screen.blit(apple.image,apple.rect)
        
    pygame.display.flip()
    clock.tick(30)
```

- 设置精灵移动的速度，因为是向下移动所以speed列表的第一个数是0，第二个数取1-10之间的随机整数
- 为精灵增加一个移动的方法，调用rect对象的move()方法、
- 使用if语句判断精灵的顶部位置是否大于窗口高度，如果是则表示精灵移出了窗口底部，那么就把精灵底部的位置设为窗口顶部的位置

![1566217962852](E:\Typora笔记\Python\assets\1566217962852.png)

- 精灵调用move()方法向下移动图像

### 让精灵四处游荡

首先将苹果增加到5个，让这5个苹果显示在随机位置上然后开始随机方向+随机速度的移动起来，营造出苹果满天飞的效果，同时处理苹果如果飘出窗口就从反方向再穿越回来

```python
import pygame
from pygame.locals import *
import sys
from random import *

pygame.init()

size = width,height = 600,400
bg = (255,255,255)

screen = pygame.display.set_mode(size)
pygame.display.set_caption("动画精灵")

#定义一个类，继承sprite的基类
class Apple(pygame.sprite.Sprite):
    def __init__(self):
        #调用基类的init方法，完成初始化
        pygame.sprite.Sprite.__init__(self)
        #显示本地的精灵图像
        self.image = pygame.image.load("apple.png")
        #获取精灵的矩阵对象
        self.rect = self.image.get_rect()
        
        #设置精灵左上角的位置随机
        self.rect.topleft=randint(0,width-50),randint(0,height-50)
        #设置精灵的移动速度
        self.speed=[randint(-10,10),randint(10,10)]
        
    #为精灵增加一个向下移动的方法
    def move(self):
        self.rect = self.rect.move(self.speed)
        #当苹果的到达窗口底部时，让苹果从上面出来
        if self.rect.top>height:
            self.rect.bottom = 0
        #当苹果的到达窗口部时，让苹果从上面出来
        if self.rect.bottom<0:
            self.rect.top = height
        #当苹果的到达窗口左侧时，让苹果从右边出来
        if self.rect.right<0:
            self.rect.left = width
        #当苹果的到达窗口右侧时，让苹果从左边出来
        if self.rect.left>width:
            self.rect.right = 0
        
#创建一个组
Applegroup = pygame.sprite.Group()

#实例化三个精灵,添加到精灵组中
for p in range(5):
    Applegroup.add(apple)
    
clock = pygame.time.Clock()
    
while True:
    for event in pygame.event.get():
        if event.type==QUIT:
            pygame.quit()
            sys.exit()
            
    screen.fill(bg)
    
    #移动和绘制者5个苹果
    for apple in Applegroup:
        #调用精灵的move方法
        apple.move()
        screen.blit(apple.image,apple.rect)
        
    pygame.display.flip()
    clock.tick(30)
```

- 随机生成一个窗口内部的左上角位置，使用width-50和height-50是为了防止精灵显示在窗口之外
- 随机生成一个精灵的移动方向与速度，取值(-10,10)可以保证精灵的方向随机。移动速度在1-10之间取随机值

## pygame显示文字

### 显示一行文字

下面实现：窗口中间显示一行绿色的文字

```python
import pygame
import sys

pygame.init()

#设置游戏窗口显示在桌面中间
os.environ['SDL_VIDEO_CENTERED'] = '1'

screen = pygame.display.set_mode((600,400))
pygame.display.set_caption("显示文字")
screen_rect = screen.get_rect()

#创建字体对象
cur_font = pygame.font.SysFont("simhei",15,False,False)

#把文本渲染成图片
text_surface = cur_font.render("哈哈哈，你看到我了吗？",True,(0,255,0))

#设置文本显示的位置
text_rect = text_surface.get_rect()
text_rect.center = screen_rect.center

while True:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()
            
    #窗口绘制文字图像
    screen.blit(text_surface,text_rect)
    
    pygame.display.flip()

```

* 设置系统环境变量os.environ['环境变量名称']="环境变量值"，其中key和value均为string类型

  环境变量"SDL_VIDEO_CENTERED"='1'可以让窗口啊显示在屏幕中间，也可以通过os.environ['SDL_VIDEO_WINDOW_POS']="%d,%d"%(x,y)将窗口固定在指定的位置

* 创建字体对象（使用系统的字体）

  * 第一个参数指定系统字体的名字
  * 第二个参数指定字体的大小
  * 第三个可选参数设置是否加粗属性
  * 第四个可选参数是否斜体属性

* 使用字体对象Font.render函数创建一个Surface，里面包含写出来的文字

  * 第一个参数是要写的文字，文字只能包含一行，换行符不会被画出来
  * 第二个参数被指定是否使用抗锯齿效果如果是True字符会有光滑的边缘
  * 第三个参数是字体的颜色（RGB）

* 获取文本的矩形对象，设置文本的位置在窗口中间

* 使用blit()方法把文字Surface对象画到另一个surface对象(screen)上才能正常显示出来

![1566284644433](E:\Typora笔记\Python\assets\1566284644433.png)

注：

* 同一种字体在Windows和Mac OS上显示的效果也许并不相同
* 正常情况下系统里都会有arial字体，如果没有会使用默认字体，本地练习时需要选择一种当前系统支持的字体，查看系统支持的字体可通过pygame.font.get_fonts获取
* 如果想要显示中文，那么在创建Font对象时第一个参数必须是可以使用中文的字体，如宋体、黑体等，或者直接用自己的中文ttf文件

### 让文字更酷炫

让文字的颜色随机显示，让文字的大小逐步变大，从而产生动感的效果

```python
import pygame
import sys,random

pygame.init()

#设置游戏窗口显示在桌面中间
os.environ['SDL_VIDEO_CENTERED'] = '1'

screen = pygame.display.set_mode((600,400))
pygame.display.set_caption("显示文字")
screen_rect = screen.get_rect()

bg = (0,0,0)

#定义一个函数实现：创建字体对象、创建文字surface对象、设置文字位置

def show_text(size,colour):
    #使用自己的ttf文件，创建一个字体对象
    my_font = pygame.font.Font("LoveRabbit.ttf",size)
    text_surface = my_font.render("哈哈哈，你又看到我了",True,colour)
    text_rect = text_surface.get_rect()
    text_rect.center = screen_rect.center
    
    return text_surface,text_rect

clock = pygame.time.Clock()

while True:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()
    
    #让文字大小始终保持20-60之间变化
    for size in range(20,60):
        #设置字体随机的颜色
        colour = (random.randint(0,255),random.randint(0,255),random.randint(0,255))
        #填充游戏背景为黑色，这样更容易凸显文字颜色
        screen.fill(bg)
        #绘制文字
        screen.blit(show_text(size,colour)[0],show_text(size,colour)[1])
        
        pygame.display.flip()
        clock.tick(60)
```

* 在之前的基础上修改代码：定义一个函数show_text方便while循环中调用，该函数返回文字surface对象和文字位置，因为在窗口绘制文字时需要这两个参数
* 创建一个文字对象，使用当前目录下自己下载的字体文件LoveRabbit.ttf（该字体支持中文）
* 每次循环时让文字大小从20不断增大到60，从而产生动画效果
* 每次循环时让RGB值取0-255之间的随机整数，就可以让文字颜色变得五彩斑斓
* 游戏窗口填充黑色，初学者注意：一定要重新绘制背景才能清除上一次绘制的文本，否则会显示文本的重叠效果
* 在窗口绘制文字，两个参数通过调用show_text函数获取

## pygame碰撞检测

### 精灵之间的碰撞

实现案例：一个精灵是pygame的小蟒蛇，另一个精灵是小苹果，他们以随机方向+速度在窗口内移动（到达边缘则反方向移动），如果发生碰撞则游戏停止

```python
import pygame
import sys,os
from random import *

#游戏窗口固定在桌面中间
os.environ['ADL_VIDEO_CENTERED']='1'

pygame.init()
bg_size = width,height=600,400
screen = pygame.display.set_mode(bg_size)
pygame.display.set_caption("小蟒蛇碰撞苹果")
bg = (255,255,255)

#定义一个蟒蛇类，继承sprite基类
class Snake(pygame.spirite.Sprite):
    def __init__(self):
        pygame.sprite.Sprite.__init__(self)
        self.image = pygame.image.load("logo_lofi.png")
        self.rect = self.image.get_rect()
        self.rect.topleft=randint(0,width-50),randint(0,height-50)
        self.speed=[randint(-10,10),randint(-10,10)]
        
    def move(self):
        self.ret = self.rect.move(self.speed)
        #如果到达窗口边缘则反方向移动
        if self.rect.left<0 or self.rect.right>width:
            self.speed[0] = -self.speed[0]
        if self.rect.top<0 or self.rect.bottom>height:
            self.speed[1] = -self.speed[1]
            
#定义一个苹果类，继承sprite类
class Apple(pygame.sprite.Sprite):
    def __init__(self):
        pygame.sprite.Sprite.__init__(self)
        self.image = pygame.image.load("apple.png")
        self.rect = self.image.get_rect()
        self.rect.topleft = randint(0,width-50),randint(0,height-50)
        self.speed=[randint(-10,10),randint(-10,10)]
        
    def move(self):
        self.rect=self.rect.move(self.speed)
        #如果到达窗口边缘则反方向移动
        if self.rect.left<0 or self.rect.right>width:
            self.speed[0]=-self.speed[0]
        if self.rect.top<0 or self.rect.bottom>height:
            self.speed[1]=-self.speed[1]

#实例化一个苹果，一个蟒蛇
apple = Apple()
snake = Snake()

clock = pygame.time.Clock()

#定义一个游戏是否运行的变量
ranning = True
while running:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()
            
    screen.fill(bg)
    apple.move()
    snake.move()
    
    screen.blit(apple.image,apple.rect)
    screen.blit(snake.image,snake.rect)
    
    #检测蟒蛇与苹果是否发生碰撞
    if pygame.sprite.collide_rect(apple,snake):
        #发生碰撞停止循环
        running = False
        
    pygame.display.flip()
    clock.tick(60)

```

* 导入os库，使用os.environ['SDL_VIDEO_CENTERED']='1'设置游戏窗口在桌面的中间
* 由于pygame.sprite.collide_rect()这个函数接收两个精灵作为参数。所以需要定义一个Snake类和一个Apple类，都继承自pygame.sprite.Sprite,这个函数要求每个精灵必须有一个rect属性
* 定义一个游戏是否运行的变量，默认值为True，这样就可以在游戏运行中通过修改running的状态为False来停止游戏
* 调用pygame.sprite.collide_rect()函数，传入两个精灵作为参数，这个函数返回是一个布尔值
* 发生碰撞则修改游戏运行状态为False停止游戏运行
* pygame.sprite.collide_rect()函数进行碰撞检测，它是以图片的矩形区域作为检测范围，检测的是两个精灵的rect是否产生重叠
* 

![1566289552342](E:\Typora笔记\Python\assets\1566289552342.png)

### pygame.sprite.collide_mask()

针对这个情况pygame还为我们提供了一个更加精确的检测：pygame.sprite.collide_mask()，这个函数可以用于两个精灵之间的像素遮罩检测，这个函数也是接收两个精灵作为参数，返回值是一个布尔值

注意：这个函数要求每一个精灵必须有一个mask属性，用于指定检测范围，在pygame的mask模块使用from_surface()函数可以将一个surface对象中的透明部分标志为mask并返回

修改代码使用pygame.sprite.collide_mask()函数进行两个精灵之间的完美碰撞检测

```python
import pygame
import sys,os
from random import *

#游戏窗口固定在桌面中间
os.environ['ADL_VIDEO_CENTERED']='1'

pygame.init()
bg_size = width,height=600,400
screen = pygame.display.set_mode(bg_size)
pygame.display.set_caption("小蟒蛇碰撞苹果")
bg = (255,255,255)

#定义一个蟒蛇类，继承sprite基类
class Snake(pygame.spirite.Sprite):
    def __init__(self):
        pygame.sprite.Sprite.__init__(self)
        self.image = pygame.image.load("logo_lofi.png")
        self.rect = self.image.get_rect()
        self.rect.topleft=randint(0,width-50),randint(0,height-50)
        self.speed=[randint(-10,10),randint(-10,10)]
        self.width,self.height = bg_size
        #增加一个mask属性
        self.mask = pygame.mask.from_surface(self.image)
        
    def move(self):
        self.ret = self.rect.move(self.speed)
        #如果到达窗口边缘则反方向移动
        if self.rect.left<0 or self.rect.right>width:
            self.speed[0] = -self.speed[0]
        if self.rect.top<0 or self.rect.bottom>height:
            self.speed[1] = -self.speed[1]
            
#定义一个苹果类，继承sprite类
class Apple(pygame.sprite.Sprite):
    def __init__(self):
        pygame.sprite.Sprite.__init__(self)
        self.image = pygame.image.load("apple.png")
        self.rect = self.image.get_rect()
        self.rect.topleft = [300,300]
        self.speed=[randint(-10,10),randint(-10,10)]
        self.width,self.height = bg_size
        #增加一个mask属性
        self.mask = pygame.mask.from_surface(self.image)
        
    def move(self):
        self.rect=self.rect.move(self.speed)
        #如果到达窗口边缘则反方向移动
        if self.rect.left<0 or self.rect.right>width:
            self.speed[0]=-self.speed[0]
        if self.rect.top<0 or self.rect.bottom>height:
            self.speed[1]=-self.speed[1]

#实例化一个苹果，一个蟒蛇
apple = Apple(bg_size)
snake = Snake(bg_size)

clock = pygame.time.Clock()

#定义一个游戏是否运行的变量
ranning = True
while running:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()
            
    screen.fill(bg)
    apple.move()
    snake.move()
    
    screen.blit(apple.image,apple.rect)
    screen.blit(snake.image,snake.rect)
    
    #检测蟒蛇与苹果是否发生碰撞
    if pygame.sprite.collide_mask(apple,snake):
        #发生碰撞停止循环
        running = False
        
    pygame.display.flip()
    clock.tick(60)

```

* 为两个精灵添加一个mask属性，获取图像的掩膜用以更加精确的碰撞检测，在程序中并不会直接使用这个属性，但是必不可少
* pygame.sprite.collide_mask()函数用于两个精灵之间的像素遮罩检测，接收两个精灵作为参数，返回值是一个布尔值

### 精灵与组之间的碰撞

案例：小蟒蛇是单个精灵 ，小苹果是一个精灵组，他们以随机方向+速度在窗口移动，如果小蟒蛇碰撞到苹果，就把苹果吃掉

```python
import pygame
import sys,os
from random import *

os.environ['SDL_VIDEO_CENTERED']='1'

pygame.init()
bg_size = width,height=600,400
screen = pygame.display.set_mode(bg_size)
pygame.display.set_caption("小蟒蛇吃苹果")
bg = (255,255,255)

class Snake(pygame.sprite.Sprite):
    def __init__(self):
        pygame.sprite.Sprite.__init__(self)
        self.image = pygame.image.load("logo_lofi.png")
        self.rect = self.image.get_rect()
        self.rect.topleft = randint(0,width-50),randint(0,height-50)
        self.speed = [randint(-10,10),randint(-10,10)]
        self.mask = pygame.mask.from_surface(self.image)
        
    def move(self):
        self.rect = self.rect.move(self.speed)
        #如果到达窗口边缘则反向运动
        if self.rect.left<0 or self.rect.right>width:
            self.speed[0] = -self.speed[0]
        if self.rect.top<0 or self.rect.bottom>height:
            self.speed[1] = -self.speed[1]
            
class Apple(pygame.sprite.Sprite):
    def __init__(self):
        pygame.sprite.Sprite.__init__(self)
        self.image = pygame.image.load("apple.png")
        self.rect = self.image.get_rect()
        self.rect.topleft = randint(0,width-50),randint(0,height-50)
        self.speed=[randint(-10,10),randint(-10,10)]
        self.mask = pygame.mask.from_surface(self.image)
        
    def move(self):
        self.rect = self.rect.move(self.speed)
        #如果到达窗口边缘则反方向穿越回来
        if self.rect.top>height:
            self.rect.bottom=0
        if self.rect.bottom<0:
            self.rect.top=height
        if self.rect.right<0:
            self.rect.left=width
        if self.rect.left>width:
            self.rect.right=0
            
#创建精灵组，实例化10个小苹果添加到精灵组
apple_num = 10
apple_group = pygame.sprite.Group()
for i in range(apple_num):
    apple = Apple()
    apple_group.add(apple)
    
#实例化一个小蟒蛇
snake = Snake()

clock = pygame.time.Clock()
running = True

while running:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()
            
    screen.fill(bg)
    
    #移动和绘制10个苹果
    for a in apple_group:
        a.move()
        screen.blit(a.image,a.rect)
        
    #移动和绘制一个小蟒蛇
    snake.move()
    screen.blit(snake.image,snake.rect)
    
    #检测小蟒蛇与小苹果们是否发生完美碰撞，碰撞后小苹果消失
    pygame.sprite.spritecollide(snake,apple_group,True,pygame.sprite.collide_mask)
    pygame.display.flip()
    clock.tick(60)
```

调用pygame.sprite.spritecollide()函数把苹果精灵组中的所有苹果逐个地对小蟒蛇精灵进行碰撞检测，发生碰撞的小苹果会作为一个列表返回

* 第一个参数指定被检测的单个精灵
* 第二个参数指定一个精灵组，由sprite.Group()生成
* 第三个参数是一个bool值，设置是否从组中删除检测到碰撞的精灵，当为True时，会删除组中所有冲突的精灵，False不会删除
* 第四个参数为可选参数，用于设置一个回调函数可以用于定制特殊的检测方法：collided = pygame.sprite.collide_mask用于精灵和组之间的像素遮盖检测，是最为精准的检测方法

### 精灵组之间的碰撞

pygame的sprite模块提供了一个groupcollide()函数，这个函数可以检测两个组之间的碰撞，它返回一个字典

下面将完成：一组苹果与一组绿苹果之间的碰撞检测，如果发生碰撞则反方向移动

```python
import pygame
import sys,os
from random import *

os.environ['SDL_VIDEO_CENTERED']='1'

pygame.init()
bg_size = width,height = 600,400
screen = pygame.display.set_mode(bg_size)
pygame.display.set_canption("小蟒蛇碰撞小苹果")
bg = (255,255,255)

#定义一个红苹果类，继承sprite基类
class R_apple(pygame.sprite.Sprite):
    def __init__(self):
        pygame.sprite.Sprite.__init__(self)
        self.image = pygame.image.load("apple.png")
        self.rect = self.image.get_rect()
        self.rect.topleft = randint(0,width-50),randint(0,height-50)
        self.speed = [randint(-10,10),randint(-10,10)]
        self.mask = pygame.mask.from_surface(self.image)
    def move(self):
        self.rect = self.rect.move(self.speed)
        #如果到达窗口边缘则反方向穿越回来
         if self.rect.top>height:
            self.rect.bottom=0
        if self.rect.bottom<0:
            self.rect.top=height
        if self.rect.right<0:
            self.rect.left=width
        if self.rect.left>width:
            self.rect.right=0
            
            
#定义一个绿苹果类，继承sprite基类
class G_apple(pygame.sprite.Sprite):
     def __init__(self):
        pygame.sprite.Sprite.__init__(self)
        self.image = pygame.image.load("g_apple.png")
        self.rect = self.image.get_rect()
        self.rect.topleft = randint(0,width-50),randint(0,height-50)
        self.speed = [randint(-10,10),randint(-10,10)]
        self.mask = pygame.mask.from_surface(self.image)
    def move(self):
        self.rect = self.rect.move(self.speed)
        #如果到达窗口边缘则反方向穿越回来
         if self.rect.top>height:
            self.rect.bottom=0
        if self.rect.bottom<0:
            self.rect.top=height
        if self.rect.right<0:
            self.rect.left=width
        if self.rect.left>width:
            self.rect.right=0
apple_num = 3
r_apple_group = pygame.sprite.Group()
g_apple_group = pygame.sprite.Group()

#实例化三个红苹果和绿苹果
for i in range(apple_num):
    r = R_apple()
    g = G_apple()
    r_apple_group.add(r)
    g_apple_group.add(g)
    
clock = pygame.time.Clock()

running =True
while running:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()
            
    screen.fill(bg)
    #移动和绘制这两组苹果
    for r,g in zip(r_apple_group,g_apple_group):
        r.move()
        g.move()
        screen.blit(r.image,r.rect)
        screen.blit(g.image,g.rect)
        
    #检测两组精灵之间是否发生完美碰撞，定义一个变量保存碰撞结果
    collide_dict = pygame.sprite.groupcollide(r_apple_group,g_apple_group,False,False,collided = pygame.sprite.collide_mask)
    #遍历发生碰撞的精灵列表
    for a in collide_dict.values():
        #获取精灵对象
        a = a[0]
        #修改精灵的移动方向
        a.speed[0] = -a.speed[0]
        a.speed[1] = -a.speed[1]
        
	pygame.display.flip()
    clock.tick(60)
```

* 使用pygame.sprite.groupcollide()函数检测红苹果组与绿苹果组之间是否发生碰撞
  * group1:精灵组1
  * group2:精灵组2
  * dokill1:发生碰撞时，是否销毁精灵组1中的发生碰撞的精灵，True为销毁
  * dokill2:发生碰撞时，是否销毁精灵组2中的发生碰撞的精灵，True为销毁
  * collided:用于设置一个回调函数可以用于定制特殊的检测方法:collided = pygame.sprite.collide_mask用于精灵和组之间的像素遮盖检测，是最精准的检测方法
* 返回一个字典，键是精灵组1（红苹果）中发生碰撞的精灵，值是精灵组2中与该精灵发生碰撞的精灵列表，如：{<R_apple sprite(in 1 groups)>: [<G_apple sprite(in 1 groups)>, <G_apple sprite(in 1 groups)>]}
* 获取发生碰撞的精灵列表，如，dict_values([[<G_apple sprite(in 1 groups)>], [<G_apple sprite(in 1 groups)>]]) 。遍历这个列表来获取每个精灵对象。需要注意这个列表中的每一个元素是一个子列表。
* 通过列表索引值[0]来获取到真正的精灵对象
* 修改精灵对象的移动方向，通过修改speed[0]和speed[1]这两个数字进行负数处理，就可以让精灵反方向移动

有些时候两个苹果撞击之后会一直抖动或者没有反方向弹开，这是因为个别苹果移动速度太快，导致两个苹果碰撞重叠的区域太大，无法通过一次碰撞就完全分离出来，所以进行了多次原地碰撞（反弹->碰撞->反弹->碰撞...)后才真正的分离出来，多次的碰撞导致看起来没有反方向弹开。

### 播放音乐

### 播放背景音乐

```python
import pygame
import sys
pygame.init()

#初始化mixer模块
pygame.mixer.init()

size = width,height=600,400
screen = pygame.display.set_mode(size)
pygame.display.set_caption("播放音乐和音效")

#加载本地的音乐文件
pygame.mixer.music.load("天空之城.ogg")
#设置音量
pygame.mixer.music.set_volume(0.5)
#播放音乐
pygame.mixer.music.play()

#设置音乐播放状态的默认值：播放
pause = False

while True:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()
        if event.type == pygame.KEYDOWN:
            #如果按下空格键，修改播放状态的值取反
            if event.key == pygame.K_SPACE:
                pause = not pause
        #如果pause的值为True则暂停音乐
        if pause:
            pygame.mixer.music.pause()
        #否则播放音乐’
        else:
            pygame.mixer.music.unpause()
```

* 初始化声音加载和回放的mixer模块
* 播放背景音乐需要使用music模块，music模块是mixer模块中的一个特殊实现，因此我们使用pygame.mixer.music来调用该模块下的方法，pygame.mixer.music调用load()方法加载本地的音乐文件，参数是ogg文件的路径
* （转化音乐格式网页：https://www.media.io/zh/）
* pygame.mixer.music调用set_volume()方法设置背景音乐的音量大小，音量大小的范围为0.0-1.0
* pygame.mixer.music调用play()方法就可以开始播放音乐了，play()方法
  * 第一个可选参数是重复播放次数，正数n表示共播放n+1次，-1表示无限循环；
  * 第二个可选参数是歌曲开始播放的音乐的位置，以秒为单位
* 设置一个音乐是否播放的布尔值，是为了方便按空格键根据这个值决定暂停还是播放
* 判断用户是否按下空格键，按下则将音乐播放状态的值取反
* 根据音乐播放状态进行判断，正在播放改为暂停，暂停改为播放

### 播放音效

下面实现：按鼠标左键播放一个音效，按右键播放另一个音效，按鼠标中间键暂停两个音效播放

```python
import pygame
from pygame.locals import *
import sys

pygame.init()
pygame.mixer.init()
screen = pygame.display.set_mode((600,400))
pygame.display.set_caption("播放音乐和音效")

#创建两个音声对象，并为其设置音量
soundwav1 = pygame.mixer.Sound("dog.wav")
soundwav1.set_volume(0.1)
soundwav2 = pygame.mixer.Sound("bird.wav")
soundwav2.set_volume(0.5)

while True:
    for event in pygame.event.get():
        if event.type == QUIT:
            sys.exit()
        #检测鼠标按键
        if event.type == MOUSEBUTTONDOWN:
            #鼠标左键，暂停音效2播放，播放音效1
            if event.button == 1:
                soundwav2.stop()
                soundwav1.play()
            #鼠标右键，暂停音效1播放，播放音效2
            if event.button == 3:
                soundwav1.stop()
                soundwav2.play()
            #鼠标中建，暂停播放音效1和音效2
            if event.button == 2:
                soundwav1.stop()
                soundwav2.stop()
```

* 首先加载本地的wav声音文件创建新的Sound对象，再调用Sound对象的set_volume()方法设置音量，运行程序前学员需要先下载两个wav文件保存在当前目录
* 增加一个事件检测，判断用户是否点击鼠标按键