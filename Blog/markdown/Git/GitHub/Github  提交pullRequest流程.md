---
title: Pull Request流程
date: 2017-01-10 23:12:43
tags:
- Github
- 笔记
categories: 
- Github
---
### Pull Request流程
1. 首先fork我的项目
2. 把fork过去的项目也就是你的项目clone到你的本地
3. 运行 git remote add looly git@github.com:looly/elasticsearch-definitive-guide-cn.git 把我的库添加为远端库
4. 运行 git pull looly master 拉取并合并到本地
5. 编写代码
6. commit后push到自己的库（git push origin master）
7. 登录Github在你首页可以看到一个 pull request 按钮，点击它，填写一些说明信息，然后提交即可。
1~3是初始化操作，执行一次即可。在翻译前必须执行第4步同步我的库（这样避免冲突），然后执行5~7既可。
