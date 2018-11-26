---
layout: post
title: 虚拟环境安装及使用（virtualenv和virtualenvwrapper）
categories: Python
description: python模块的使用
keywords: Python,Python模块,虚拟环境,virtualenv,virtualenvwrapper 
---

参考资料：

[官方文档](https://pythonguidecn.readthedocs.io/zh/latest/dev/virtualenvs.html)

[vscode设置虚拟环境官方文档](https://code.visualstudio.com/docs/python/environments)

***

### 1 什么是虚拟环境
&emsp;&emsp;由于python很多项目都有不同的版本，我们需要使用虚拟环境来适应不同项目的开发环境同时不会相互影响。


### 2 virtualenv和virtualenvwrapper

#### 2.1 virtualenv
&emsp;&emsp;virtualenv 是一个创建隔绝的Python环境的 工具。virtualenv创建一个包含所有必要的可执行文件的文件夹，用来使用Python工程所需的包。
```c
pip install virtualenv
```

#### 2.2 virtualenvwrapper
##### 2.2.1 安装
&emsp;&emsp;相比之下,[virtualenvwrapper](https://virtualenvwrapper.readthedocs.io/en/latest/index.html)提供了一系列命令使得和虚拟环境工作变得愉快许多。它把您所有的虚拟环境都放在一个地方。
&emsp;&emsp;安装之前**确保 virtualenv 已经安装了**
```c
pip install virtualenvwrapper-win
```
&emsp;&emsp;virtualenvwrapper会使用C:\Users\user\Envs来存放虚拟环境。

##### 2.2.2 使用
（1）创建虚拟环境
```
mkvirtualenv my_project
```

（2）在虚拟环境上工作
```
workon my_project
```
> 注意这一步在vscode的键入无效，具体使用方式见下。

（3）停止虚拟环境
```
deactivate
```
（4）删除虚拟环境
```
rmvirtualenv my_project
```

### 3 在vscode上使用虚拟环境
#### 3.1 设置解释器

&emsp;&emsp;首先，我们需要通过cril+shift+p，输入`select interpreter`选择我们虚拟对应的虚拟环境文件。
> 当我们刚创建虚拟环境后，需要重启一下vscode，然后env文件夹下面的虚拟环境会被vscode自动检索到。

在下方我们可以看到解释器已经改变：

![](/images/blog/python/model/virtualenv-1.png)

#### 3.2 设置虚拟终端

&emsp;&emsp;vscode的默认终端为cmd，当我们设置完解释器后，需要在vscode资源管理器中右键选择在终端打开，但是第一次可能会提示错误，如下：

    ```
    PS C:\dev\loadtest> & c:/dev/loadtest/env/Scripts/activate.ps1
    & : File C:\dev\loadtest\env\Scripts\activate.ps1 cannot be loaded because running scripts is disabled on this system. For more information, see about_Execution_Policies at https:/go.microsoft.com/fwlink/?LinkID=135170.
    At line:1 char:3
    + & c:/dev/loadtest/env/Scripts/activate.ps1
    +   ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : SecurityError: (:) [], PSSecurityException
        + FullyQualifiedErrorId : UnauthorizedAccess
    ```

&emsp;&emsp;根据[GitHub上面的解决方案](https://github.com/Microsoft/vscode-python/issues/2559)，这可能是vscode的问题。我们需要通过管理员身份打开powerSheel（注意不是cmd），然后输入`Set-ExecutionPolicy RemoteSigned`,选择y，如下：

![](/images/blog/python/model/virtualenv-2.png)

&emsp;&emsp;然后再次回到vscode，选择终端运行即可正常运行，如下：

![](/images/blog/python/model/virtualenv-3.png)






