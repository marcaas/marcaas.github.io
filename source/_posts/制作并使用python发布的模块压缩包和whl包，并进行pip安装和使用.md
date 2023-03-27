---
title: 制作并使用python发布的模块压缩包和whl包，并进行pip安装和使用
date: 2023-03-27 13:28:21
tags:
    - [python]
    - [package]
---

把自己的程序打包给别人去用，此处的包和打包的概念不一样，而是python中的package，是指发布的模块压缩包或whl包

<!-- more -->

# 几个概念

* 模块
* 包

## 模块

模块是Python程序架构的一个核心概念，每一个以扩展名.py结尾的Python源代码文件都是一个模块，在模块中定义的全局变量、函数、类都是提供给外界直接使用的工具，用import进行模块的导入。

## 包（Package）

包是一个包含多个模块的特殊目录（文件夹），目录下有一个特殊的文件__init__.py，目的是为了import + 包名可以一次性导入包中所有的模块。

# ```__init__.py```

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
