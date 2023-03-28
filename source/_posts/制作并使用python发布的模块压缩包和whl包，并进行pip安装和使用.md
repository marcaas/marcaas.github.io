---
title: 制作并使用python发布的模块压缩包和whl包，并进行pip安装和使用
date: 2023-03-27 13:28:21
tags:
    - [python]
    - [package]
---
![](https://raw.githubusercontent.com/marcaas/hexoPicgo/master/wallhaven-9mjoy1.png)

把自己的程序打包给别人去用，此处的包和打包的概念不一样，而是python中的package，是指发布的模块压缩包或whl包

<!-- more -->

# 几个概念

* 模块
* 包
* 第三方模块
* pip
* ```__init__```

## 模块

模块是Python程序架构的一个核心概念，每一个以扩展名.py结尾的Python源代码文件都是一个模块，在模块中定义的全局变量、函数、类都是提供给外界直接使用的工具，用import进行模块的导入。

## 包（Package）

包是一个包含多个模块的特殊目录（文件夹），目录下有一个特殊的文件__init__.py，目的是为了import + 包名可以一次性导入包中所有的模块。

## 第三方模块

第三方模块通常是指由第三方开发团队开发的并且被程序员广泛使用的Python包/模块。

## pip

pip是一个通用的Python包管理工具，提供了对Python包的查找、下载、安装、卸载等功能。

## ```__init__.py```

是一个对外的接口，若需要把我们的模块在外界使用，则需要编辑配置init文件，来指定对外的列表，例如：

我建立了一个结构如下的文件夹

![](https://raw.githubusercontent.com/marcaas/hexoPicgo/master/20230327140115.png)

其中__init__.py为接口文件，test1和test2是两个模块，main文件用来测试，先在__init__文件中进行编辑

```py
from . import test1
from . import test2
```

然后在main文件里测试，输入

```py
import major1

major1.
```

可以看到，代码提示中出现test1和test2

![](https://raw.githubusercontent.com/marcaas/hexoPicgo/master/20230327140600.png)

此时我们如果把__init__.py中的test1一行注释掉，则会观察到，代码提示中只剩下了test2

![](https://raw.githubusercontent.com/marcaas/hexoPicgo/master/20230327140736.png)

以上我们可以看出__init__.py文件的作用。

# 构建自己的第三方库（whl格式）

* 安装setuptools
* 创建并编辑setup.py文件
* 打包
* 安装
* 卸载

首先，我的文件结构如下

![](https://raw.githubusercontent.com/marcaas/hexoPicgo/master/20230327161801.png)

其中__init__.py中引入hellotest.py和helloworld.py两个模块

```py
from . import hellotest
from . import helloworld
```

## 安装setuptools
```sh
pip install setuptools
```

## 创建setup.py文件

setup.py文件为打包的配置文件，内容如下，注意这里的打包的py文件和包文件夹的目录结构要和真实文件的结构相对应。

```py
# 打包成模块压缩包
from setuptools import setup
from setuptools import find_packages

setup(
    name="hello",  # 包名
    version="0.1",  # 版本
    # 最重要的就是py_modules和packages
    py_modules=["hello.hellotest","hello.helloworld"],  # py_modules : 打包的.py文件
    packages=["hello"],  # packages: 打包的python文件夹
    # keywords=("AI", "Algorithm"),  # 程序的关键字列表
    description="AIAgorithmPack",                 # 简单描述
    long_description="AIAgorithmPack for python", # 详细描述
    # license="MIT Licence",  # 授权信息
    url="https://marcaas.github.io/",  # 官网地址
    author="marcaas",  # 作者
    author_email="thy777marcaas@gmail.com",  # 作者邮箱
    # packages=find_packages(), # 需要处理的包目录（包含__init__.py的文件夹）
    # platforms="any",  # 适用的软件平台列表
    # install_requires=[],  # 需要安装的依赖包
    # 项目里会有一些非py文件,比如html和js等,这时候就要靠include_package_data和package_data来指定了。
    # scripts=[],  # 安装时需要执行的脚本列表
    # entry_points={     # 动态发现服务和插件
    #     'console_scripts': [
    #         'jsuniv_sllab = jsuniv_sllab.help:main'
    #     ]
    # }

)

```

## 打包

cd到setup.py文件所在目录，执行命令

```sh
python setup.py bdist_wheel
```

运行后可以看到文件结构发生了改变

![](https://raw.githubusercontent.com/marcaas/hexoPicgo/master/20230327162956.png)

我们关注dist文件夹中的whl文件，这就是我们打包好的本地第三方库。

## 安装

我们新建一个文件夹，并把whl文件放到里面

![](https://raw.githubusercontent.com/marcaas/hexoPicgo/master/20230327163408.png)

cd到testy文件夹，命令行运行

```sh
pip install hello-0.1-py3-none-any.whl
```

可以看到pip先在远程仓库寻找叫这个名字的包，没找到则在本地目录中找并安装。

![](https://raw.githubusercontent.com/marcaas/hexoPicgo/master/20230327163553.png)

下面我们测试一下，命令行输入

```sh
python
```

进入python环境

输入如下指令，可以看到我们的模块被运行了

![](https://raw.githubusercontent.com/marcaas/hexoPicgo/master/20230328085954.png)

## 卸载

使用指令

```sh
pip list
```

查看已安装的包，可以看到我们之前构建的hello包赫然在列

![](https://raw.githubusercontent.com/marcaas/hexoPicgo/master/20230328085324.png)

现在我们来卸载它

使用命令

```sh
pip uninstall hello
```

卸载成功

![](https://raw.githubusercontent.com/marcaas/hexoPicgo/master/20230328085549.png)

至此。
