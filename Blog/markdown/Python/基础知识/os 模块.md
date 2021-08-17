---
title: python os 模块
date: 2017-06-10 23:12:43
tags:
- 笔记
categories: 
- Python
---
#### os 模块
os 模块 包含函数的信息 本地路径 文件进程 环境变量

1.1 获取当前目录路径 
- os.getcwd() 

    current working directory

```
>>> import os
>>> os.getcwdb() # byte字符返回
b'C:\\Users\\zhucaidong_vendor'
>>> os.getcwd()
'C:\\Users\\zhucaidong_vendor'
>>>
```
1.2 切换路径
```
>>> os.chdir('../')
>>> os.getcwd()
'C:\\Users'
```

1.3 组合路径名文件名
```
print(os.path.join('c:/work','/homework/'))
c:/homework/
```
1.4 获取当前用户home路径
```
 os.path.expanduser('~')
'C:\\Users\\zhucaidong_vendor'
```

1.5 分割路径名
```
>>> pathname = "/Users/K/dir/subdir/k.py"
>>> os.path.split(pathname)
('/Users/K/dir/subdir', 'k.py')
>>> (dirname, filename) = os.path.split(pathname)  # 返回元祖
>>> dirname
'/Users/K/dir/subdir'
>>> pathname
'/Users/K/dir/subdir/k.py'
>>> filename
'k.py'
>>> (shortname, extension) = os.path.splitext(filename)  # 分割文件名和类型
>>> shortname
'k'
>>> extension
'.py'
```

1.6列出指定文件路径

```
>>> os.listdir()
['Administrator', 'All Users', 'Default', 'Default User', 'defaultuser0', 'desktop.ini', 'dushengguang', 'Public', 'sensetime', 'zhucaidong_vendor']
```
1.7 遍历文件树

os.walk(top, topdown=True, onerror=None, followlinks=False)

```
import os
from os.path import join, getsize
for root, dirs, files in os.walk('python/Lib/email'):
    print(root, "consumes", end=" ")
    print(sum(getsize(join(root, name)) for name in files), end=" ")
    print("bytes in", len(files), "non-directory files")
    if 'CVS' in dirs:
        dirs.remove('CVS')  # don't visit CVS directories
```
1.9 系统信息
```
#  CPU 个数

>>> os.cpu_count()
4
```

1.10 检查文件是否存在
```
>>> os.path.exists('./pyt.py')
False
```

1.11 返回绝对路径
```
>>> os.path.abspath('../')
'C:\\'
```
1.12 返回路径的基础名
```
>>> os.path.basename('./pyt.py')
'pyt.py'
```
1.13文件字节大小 
```
os.path.getsize(path)
```
1.14 判断文件类型
```
os.path.isdir()
os.path.isfile()
os.path.islink()
```

1.15 相同文件判断
```
os.path.samefile(path1, path2)

```
