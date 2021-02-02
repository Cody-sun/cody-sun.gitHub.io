---
title: 利用 pyautogui 实现的简单的明日方舟脚本
tags: ["python","pyautogui"]
---

## 准备工作

1. 安装 pyautogui

2. 安装 opencv

   opencv 不是必需的，至于为什么，这点可以接着看后面。当然我还是推荐安装

## 设计思路

首先需要明确我们要做什么，其实就是模拟人使用模拟器点击不同按钮来刷关卡：

1. 点开模拟器

   ![2020-11-19-1](C:\Users\Administrator\QingJun3.github.io\_posts\img\2020-11-19-1.png)

2. 点击蓝色开始行动

   ![2020-11-19-2](C:\Users\Administrator\QingJun3.github.io\_posts\img\2020-11-19-2.png)

3. 点击红色开始行动

   ![2020-11-19-3](C:\Users\Administrator\QingJun3.github.io\_posts\img\2020-11-19-3.png)

4. 等待关卡打完

5. 点击结束行动(这里不是有特定按钮，但是为了方便，我们选择点击这里结束行动)

   ![2020-11-19-4](C:\Users\Administrator\QingJun3.github.io\_posts\img\2020-11-19-4.png)

6. 根据设定的次数重复执行 2~5步

## 使用到的函数的介绍

1. locateCenterOnScreen()函数

   它有两个参数，一个是你要寻找的图片，一个是置信度(其实不一定叫这个，暂且这么称呼它吧)，置信度是可选参数，如果你没装 opencv 的话就用不了，该函数的作用就是在你电脑的当前屏幕中寻找与给定图片相似的区域的中点，如果找不到就返回 None。

   这里就可以说为什么要装 opencv 了，因为如果你要使用置信度参数，就一定要装 opencv。为什么我推荐装 opencv 呢，因为实测下来，不用置信度的话，有可能找不到。

2. click()函数

   也有两个参数，分别是某个点在屏幕上的横纵坐标，该函数完成在该点模拟鼠标左键点击一次。

## 完整代码实现及注释：

~~~python
import pyautogui	#导入包
import time

pyautogui.PAUSE = 0.5		#为所有的 PyAutoGUI 函数增加延迟。默认延迟时间是0.1秒。这里我不想要运行太快，所以改成0.5秒
pyautogui.FAILSAFE = True	#默认是 False 的，改为 True 之后，在程序运行过程中，只要你把鼠标移到屏幕左上角，就会终止程序(其实是产生中断，我直接投机取巧当终止了)


while True:					#点开模拟器，用循环是想找不到的时候就继续找，直到找到
    point = pyautogui.locateCenterOnScreen('simulator.png', confidence=0.9)
    if(point != None):		#找到了，跳出循环
        break
pyautogui.click(point)		#点开模拟器


num = 45					#执行次数，即你想刷多少次当前关卡

for i in range(num):		 #根据设定的执行次数循环
    while True:				#点击蓝色开始行动，用循环的原因跟上面一样
        point = pyautogui.locateCenterOnScreen('start1.png', confidence=0.9)
        if(point != None):
            break
    pyautogui.click(point)
    
    while True:				#点击红色开始行动，用循环的原因跟上面一样
        point = pyautogui.locateCenterOnScreen('start2.png', confidence=0.9)
        if(point != None):
            break
    pyautogui.click(point)
    
    while True:				#点击结束行动，用循环的原因跟上面一样，当然这里还有等待关卡打完的功能，因为一直在等嘛，直到出现结束行动
        point = pyautogui.locateCenterOnScreen('end.png', confidence=0.9)
        if(point != None):
            break
    time.sleep(3)   		#结算界面有动画，所以我们稍微等一下(3s)
    pyautogui.click(point)
    print("第 %d 次完成" %(i))	#显示完成了多少次
~~~

## 附录：我所试用的图片

1. simulator.png

   ![simulator](https://gitee.com/Cody-sun/cloud-img/raw/master/img/simulator.png)

2. start1.png

   ![start1](https://gitee.com/Cody-sun/cloud-img/raw/master/img/start1.png)

3. start2.png

   ![start2](https://gitee.com/Cody-sun/cloud-img/raw/master/img/start2.png)

4. end.png

   ![end](https://gitee.com/Cody-sun/cloud-img/raw/master/img/end.png)