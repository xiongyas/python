## Payment Gateway 测试


##### 准备

请在 Python3 下运行

依赖安装请使用 `pip install -r requirements.txt`
卸载 `pip uninstall pymysql`
查看当前已安装的 `pip list`
由于网络原因下载失败，可以指定下载源地址 `pip install 模块名 -i https://pypi.douban.com/simple/ --trusted-host pypi.douban.com`
                       更新下载版本 `pip install 模块名 -U -i https://pypi.douban.com/simple/ --trusted-host pypi.douban.com`

数据库的配置从文件中读取

在项目目录下创建 `database.yaml` 文件，里边按照这样的格式填写

```
mysql:
  host: xxxx
  user: xxxx
  password: xxxxx

mongodb:
  hosturl: mongodb://xxxxx
```


##### 使用方法

在项目目录下

执行 `behave` 代表执行所有用例

执行 `behave features/BluePay.feature` 代表执行 BluePay.feature 中的所有用例

执行 `behave -n "{Scenario name}"` 代表执行某一条用例

##### behave 简介

.feature 文件是用例描述，Scenario 相当于用例名称，Given 相当于准备数据，When 相当于测试步骤，Then 相当于预期结果

steps 目录中的 .py 文件是具体用例实现
##### pytest框架简介
python unitest/pytest
java   junit/testng
成熟的python测试用例框架，结合工具或其他框架，selenium,requests,appnium实现多种自动化测试

**主要插件**：
```
pytest-html             生成html报告
pytest-xdist            多线程
pytest-ordering         标记测试用例执行顺序
pytest-returnFailures   失败用例重跑
pytest-base_url         管理基础路径如dev/test/online 环境
allure-pytest           生成allure报告
requests
pytest
pyyaml
```
依赖安装请使用 `pip install -r requirements1.txt`

__outline__基于pytest单元测试框架规则
**测试框架**
1.输入
   1.1用例 yaml excel json python key_word
     手动编写
     自动生成
   1.2配置文件  .ini/.json/.yaml
2.执行
   日志 
   分组
   等待
   失败重试
3、输出 
   邮件 需要smtp服务器
   html allure
   

pytest框架@pytest.fixture装饰器以及通过conftest.py实现全局前置应用       
执行测试用例前后置
第一种：和unitest相似
```
def setup_class(self):
base_url = 'https://www.baidu.com'
print("每个类之前执行")
def teardown_class(self):
print('每个类之后执行')
def setup(self):
base_url = 'https://www.baidu.com'
print("用例之前执行")
def teardown(self):
print('用例之后执行')
```
第2种：fixture固件，作用就是可以任意使用前后置
```
@pytest.fixture(scope="",params="",autouse="",isd="",name="")
#scope 作用域
   # function(函数，用例，默认)
   # class    类
   # module   模块
   # package/session 会话
#params 数据驱动 
参数化 params=["",""]
定义函数需要使用：request参数和yield request.param
#autouse 自动or手动作用 
   #True自动调用，False手动调用（需要在参数里面传入固件名称）
#ids 当数据驱动时更改参数名
#name fixture的别名
```
conftest是专门用来存放固件的，项目中可以设置多个conftest文件，单个文件内可以设置多个fixture固件配置
```
@pytest.fixture(scope="function",autouse=False,params=generead_yaml(),ids=['bi','nan','yq'],name="SS测试")
# @pytest.fixture(scope="function",autouse=True)
def exe_assert(request):
print("测试用例执行之前，查询数据库用于判断等：")
yield "request.params"
print("测试用例执行之，查询数据库：")
```
pytes跳过测试用例
1.无条件跳过
@pytest.mark.skip(reason="时返回然后给")
2.有条件跳过
@pytest.mark.skipif(age>=18,reason="成第三方年")

###读取用例
1.yaml文件用途，结构，数据类型详解
yaml是一种数据类型，它可以和json之间灵活切换，支持注释、换行、字符串、裸字符串（整形，列表等）
用途：
1.1配置文件
1.2编写测试用例
数据结构：
 1.map对象（键：（空格）/值）， 如name: liyue
 2.数组（list） 用一组横线“-”开头表示，如下
-name1: liyue
-name2: xiongy
-name3: 药膳

2.pytest用例管理框架数据驱动详解
 2.1数据源驱动装饰器
    @pytest.mark.paramertize(args1,args2)
       args1:参数名：用于传值
       args2:参数值（可为列表 元祖 字典列表 字典元祖），数据中有多少值用例跑多少次）
 2.2.pytest结合yaml实现数据驱动
   1.如果有接口关联，在下一接口里无法直接调用Python里的方法。而需要通过在下一接口里通过调用方法覆盖
   2.如何在yaml中调用随机数
   3.一个接口对应一个yaml,如果接口有很多反例，nameyaml里会有很多数据
   4.断言
4.pytest结合allure生成报告
   1 下载allure,https://repo.maven.apache.org/maven2/io/qameta/allure/allure-commandline/2.19.0/
      下载后解压，再配置path环境变量,即bin路径配置到path
      path路径配置：E:\allure-2.19.0\bin
      验证 allure --version dos和pycharm都需要验证，pycharm验证不过也用不起可重启下，让环境生效
   
   
   2 生成临时的json报告
   清除临时文件，在pytest.ini增加配置
   --alluredir=./temps --clean -alluredir
   3 生成html的allure报告

##pytest测试框架用例执行(3种)
1、命令行pytest
2、主函数
自动探测
```
import pytest
if __name__ == '__main__':
pytest.main()
```
3、通过配置文件pytest.ini来改变及执行用列
不管是命令行还是主函数都会读取pytest.ini配置文件来执行

配置pytest.ini,放置在根目录下
###[pytest]
```
[pytest]
#命令行参数
addopts = -vs

#改变默认的测试路径用例
testpaths = ./testcase

#改变测试用例模块名规则
python_files = test_*.py

#改变测试类规则
python_class = Test*

#改变测试用例名规则
python_functions = test_*

#设置基础路径
base_url = http://www.baidu.com

#标记 执行：-m "smoke or others"
marker = 
smoke:冒烟测试
login:登录用例
product_manage:商品管理
...
```
yaml接口自动化实战
1.断言的封装
2.allure报告的定制
3.关键字驱动和数据驱动结合实现
4.python的反射  
    正常：先初始化对象，再调方法
    反射：通过对象得到类，然后通过类对象调用方法
5.jenkin的持续集成和allure报告集成，并且根据自动化的报告和错误率发送邮件

Selenium自动化环境搭建
1. Selenium IDE （录制、调试测试用例）
2. Selenium WebDriver （执行用例）
3. Selenium Grid （远程、并行执行用例）
---Selenium自动化测试环境搭建
 pip install selenium
--换成国内的pip源,如阿里云，-i http://mirrors.aliyun.com/pypi/simple/ 阿里云
pip install fonttools -i http://pypi.douban.com/simple/ --trusted-host pypi.douban.com
pip install webdriver-helper
