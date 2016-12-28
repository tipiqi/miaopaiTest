该测试项目主要分为monkey测试和appium写的脚本测试

monkey测试有一个单独的目录monkeyTest，用来存放monkey的测试脚本

其余的目录都用于appium的测试项目
MPTestCases目录存放appium的测试用例
start.cmd用于启动appium测试
startTest.py配置了要执行的测试用例
app目录用于存放要测试的应用

testTools存放了一些其他测试用的辅助脚本
##原网页编辑内容

最近也是在尝试用appium来实现公司app某些比较稳定的功能和页面的自动化测试<br>
项目地址：https://github.com/liyuanhong/miaopaiTest <br>
项目截图如下：<br>
<br>
其中主要的目录和文件为：<br>
/MPTestCases ----------- 存放测试用例 <br>
/errorScreenShot ------------ 用例执行失败生成的错误截图<br>
startTest.py ----------- 配置了要执行的测试用例<br>
start.cmd ----------- 用于双击启动测试（windows下）<br>

startTest.py代码如下：<br>

python
import unittest
import sys
import os

curDir = sys.path[0]

#windows下的写法
sys.path.append(curDir + '\\MPTestCases\\login')
sys.path.append(curDir + '\\MPTestCases\\shoot')
sys.path.append(curDir + '\\MPTestCases\\settingPage')
sys.path.append(curDir + '\\MPTestCases\\hotPage')
sys.path.append(curDir + '\\MPTestCases\\myPage')
sys.path.append(curDir + '\\MPTestCases\\detailPage')

#mac下的写法
sys.path.append(curDir + '/MPTestCases/login')
sys.path.append(curDir + '/MPTestCases/shoot')
sys.path.append(curDir + '/MPTestCases/settingPage')
sys.path.append(curDir + '/MPTestCases/hotPage')
sys.path.append(curDir + '/MPTestCases/myPage')
sys.path.append(curDir + '/MPTestCases/detailPage')


import MPlogin
import MPshoot
import MPsetting
import MPHotpage
import MPHotpageBanner
import MPmypage
import MPdetailPage
import MPmypageSetUserInfo


#MPlogin.suite("0")
#MPshoot.suite("0")
#MPsetting.suite("0")
#MPHotpage.suite("0")
#MPHotpageBanner.suite("0")
#MPmypage.suite("0")
#MPdetailPage.suite("0")
MPmypageSetUserInfo.suite("0")

我把测试用例都放在了MPTestCases目录下，一个大功能的测试用例都新建一个目录来存放测试用例<br>
MPTestCases 目录下有一个common目录，用来存放测试用例中会用到的公共模块，例如初始化用例，或开屏广告，登录、退出登录等模块；用例里面需要用到的时候就直接调用；并且如果该功能有改动，只需要改一个地方就好了。<br>

执行用例会生成一些错误截图，能够抓取到崩溃的截图；原理是每一个用例都用try ... except ... 包起来；一旦发生异常用例就会被判断执行失败；然后就在改执行失败的界面截图一张截图，截图的名字与用例的方法名相同；因此看截图就可以知道是哪个测试用例的那个方法发生了异常。如果程序崩溃了会有两种截图（1、带有xxx已停止运行的对话框截图 或者 2、截取到的图片为白屏或系统桌面）<br>
同时用例执行完会生成一个log文件，对比错误截图和log文件即可定位到用例执行失败的原因<br>

主要作用就是用来回归测试，验证UI或稳定的公共是否有异常<br>
