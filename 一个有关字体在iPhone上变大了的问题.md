title: 一个有关字体在iPhone上变大了的问题
date: 2015-12-31 23:33:41
tags: [issue,CSS]
showLanguage: false
postLink: an-issue-about-font-be-huge-on-iphone
index: 当我部署完自己的第一篇技术博用手机测试时,发现其中的代码部分字体明显变大了,和周围格格不入.因为博客主题是自己设计实现的,这个明显的样式问题就像是一根来挑衅的鱼刺,让人甚是不爽...
---
当我部署完自己的第一篇技术博用手机测试时,发现其中的代码部分字体明显变大了,和周围格格不入.因为博客主题是自己设计实现的,这个明显的样式问题就像是一根来挑衅的鱼刺,让人甚是不爽.

具体表现如下图左

![Image 1](/css/images/iphone-font-size-issue/all.png)

这到底是怎么回事呢?
我对该部分的相关样式属性定义如下:

``` css
font-size: 0.8rem;
font-family: PT Mono, Consolas, Monaco, Menlo, monospace;
```
按理来说,在手机上代码部分也应该按比例缩小才对,为什么不起作用呢?而此时我又发现该问题貌似只出现在了iPhone手机上,而在android手机上表现良好.

原来,当你给某一元素字体应用一个回退(fallback)系统时,浏览器会从前到后逐一尝试去支持该字体,一旦不支持前面的某些字体时,由于字体间的分辨率不同,即使定义同样的`font-size`也会有不同的视觉大小,这就是上述现象产生的原因.
