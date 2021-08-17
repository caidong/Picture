---
title: python library , module , package
date: 2017-07-16 23:12:40
tags:
- 笔记
categories: 
- Python
---
背景
Python中有一些基本的名词，很多人，尤其是一些初学者，可能听着就很晕。

此处，简单总结一下，module，library，package之间的大概区别。

 

Python中的module的简介
module，中文翻译为：模块

Python中的module，说白了，就是Python文件，而python文件一般后缀为py，所以就是你的xxx.py而已。

 

library简介
library，中文翻译为：库，也常称为：库文件

之所以此处不说是Python中的library，那是因为，本身library这个词，一般都是针对其他的编译型语言，比如C，C#等语言来说的。

常见的C/C#等语言中的library，一般指的就是：

静态的库文件：xxx.a

动态的库文件：xxx.dll

 

Python中的Package的简介
package，中文翻译为：包

Python中的package，可以简单的理解为，一组的module，一堆（相关的）module组合而成的；

 

Python中module和library之间的区别
对于library和module，说白了，都是提供了一定的功能供别人调用。

从这方面来说，也可以理解为：

Python中library等价于module；

只不过，Python中，很少说library，正常的话，都是说module；

所以，简而言之：

library多数都是指的是C，C#等语言中的库，库文件；
Python中，很少用library这个词；
Python中的“库”，“库文件”的叫法，叫做module，模块；
不论你是Python的初学者还是高手，个人建议，都还是继续沿用，官方的，通用的叫法，使用 module这个词，而不要使用用library这个词；
 

Python中的module和package之间的区别
导入单个的module，一般是这样的：

?
1
import my_module
导入package一般是这样的：

?
1
from my_package.timing.danger.internets import function_of_love
 

可以简单理解为：

module：单个的模块，一般是单个（偶尔为多个）python文件；
package：多个相关的module的组合。肯定是多个，相关的，Python文件的组合；package是用来把相关的模块组织在一起，成为一个整体的；
