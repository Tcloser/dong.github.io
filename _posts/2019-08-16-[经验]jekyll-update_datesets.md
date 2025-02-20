---
layout: post
title: "[小冬传经验]-深度学习中数据集启发+Jekyll环境配置+Hexo配置"
subtitle: 'How to enrich datasets'
author: "xudongdong"
header-img: img/jassica/jassica1.gif
catalog: true
tags:
  - Deep learning
  - Jekyll Blog
  - Object detection
  - [小冬传经验]
---

# 0、前言

> 从8月初到现在一直没怎么更新，除了几个简单的talk之外技术一点没怎么讲，因为我发现前期工作做好之后我就成了一个调参工程师！？我的天= =有点累 盯着屏幕改改learning_rate、batch_size？ 出来学习肯定不能只是调调参数了事，搞点事情！
> 另一个呢 因为Blog首页上面图片的显示问题让我特别苦恼...显示不全、重复、容器太小、编译错误搞得头大，最重要的是github编译完成后到页面刷新出来新的效果还要3-4分钟的等待时间= =哇难受..周五下午正好不忙于是就动了想在localhost调试的念头，当然，这也是我当初取巧搭站之后还要补上的步骤 呜呜呜

# 1、令人头疼的数据集

- 和不少前辈们一样，起初7月份的时候为了找表情数据集忙东忙西，不过效率高一上午找了100G+的表情(突然想起来我还有affectnet数据集在电脑里面吃灰= =)，花了两天时间下载下来只用了Fer2013跑了半个月= = 说实话这个数据集里面问题还是蛮大的，大家都是人自己做的数据集难免有这样那样的问题我就不说人家的东西不好了哈～不过我把excel像素点复原后发现人为打标签还是存在一定问题的。
- 由于做的项目是关于学校课堂学生上课状态下的表情识别,说实话这里超级想画一个流程图↓，整个流程这些部分都不是很难，github上开源项目改改都能用，但是！从班级图片再到切出每个人的截图这一物体检测过程是很困难的。从一张人数众多的图片中尽可能多地识别到一个物体有不少人做过研究，github上貌似看到过开源项目，因为我来这边的时候已经做好了，所以具体细节不讲，只是大致了解了一下。

<img src="/img/190816post/liuchengtu.jpg" >

> 说到数据集，从我自己整理的数据集来看，主要有以下几个问题：

###  (1)怎么定义`分类`

- 表情这个东西我在一开始分类的时候加上了个人情感，多是一些认真、走神、瞌睡疲惫之类的标签，后来采用：理解-微笑(正脸微笑漏出牙齿)、倾听-无表情(正常无表情脸部可有小角度倾斜)，这样的标准来制作数据集一方面给标签更加准确的定义，同时后期其他人公事也可以按相同标准来补充。

###  (2)怎样提高网络训练后识别的`准确度`

- 对我的网络来说，epoch=100最后准确率能达到99%..好高的感觉，在不断地补充其他相同图片的过程中预测的准确度也不断上升，其中有几个关键点：
一个是对新的数据集先预测再矫正，只取准确率教高的部分图片进行分类
二是把数据集进行角度的变化，水平、垂直的翻转,能有什么作用看看下面老板说的～

```
    11号听了一个讲人深度神经网络的一个报告，凭记忆简要列几点有参考价值的：
    1、用augmented data（就是把通过加噪声、旋转平移等处理后的数据增加到训练数据集里）能提高准确率；
    2、相比ResNet（就是在一个block里可以把输入加到经过卷积和池化之后的输出上—-效果是一方面是保持一些上一层的信息，另一方面是向后求导时可避免梯度等于零的情况），denseNet（就是把前几层的输入都加进来）的收敛性和准确率表现会更好一些；
    3、现在的cnn一般都得用BN（batch normalization）技巧，这个可以允许更大的learning rate；
    4、用pruning等技术可以减少减小模型和网络；
    5、在训练好一个网络之后，再用adversarial training（防（像素）敏感性攻击的训练）过的网络具有更高的强壮性和防攻击性。

```

# 2、20190816:一直被忽视然后真香的Jekyll
- 说实话嗷~~，我一直以为这个不太好用也不知道到底什么用，一直到安装的时候还在怀疑怎么会这么麻烦，又是下载又是版本冲突..还好这篇文在编辑的时候只用`$jekyll serve`便可以`http://localhost:4000/`看效果了～速度炒鸡快！
说的简单点就是新建一terminal<br>
```
$ jekyll sever
```
打开浏览器窗口即可查看markdown实时预览的效果
当然这个方法还是要不断地刷新才能看到做出的最新更改<br> 
- 具体过程参考下面的链接吧～有错误记得百度或者问我哈哈哈哈哈 问题肯定不少～

<img src="/img/190816post/jekyllpeizhi.jpg">

# 3、20190816:把自己吹上天的`“强力驱动Hexo”`
> 国际惯例：

- [Hexo环境配置流程亲测](https://gcchen.cn/2019/07/31/create-hexo-in-ubuntu/#more)
- 按步走下去肯定可以的，trust me！ 不过我再送你一个链接，不过不要全看完，直接拉到最底下吧～
- [How to solve - hexo command not found](https://www.jianshu.com/p/1851395f1f5f?utm_campaign=maleskine&utm_content=note&utm_medium=seo_notes&utm_source=recommendation)


> 如果还是不可以 可以尝试通过 sudo npm install hexo-cli -g `重装`一下hexo

 因为当时我重装完之后真的没问题了= =我好难啊..


# 4、20200412:VIM-Plug markdown-preview

最近习惯了使用vim进行文件的编辑以及代码的调试之后发现了这个插件，是真的棒好吗!
完美解决了markdown实时预览的烦恼，只需要简单地键入
```
:markdownpreview
```
甚至直接打个`markdown`然后Tab提示补全内容，在浏览器中会自动打开效果预览，我太爱了:)


## 参考文档
- [Jekyll本地搭建开发环境以及Github部署流程](https://www.jianshu.com/p/f37a96f83d51)
- [搭建本地 Jekyll 编译环境时问题汇总](https://blog.csdn.net/wudalang_gd/article/details/74619791)


