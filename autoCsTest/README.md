## Payment Gateway 测试


##### 准备

请在 Python3 下运行

依赖安装请使用 `pip install -r requirements.txt`

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



