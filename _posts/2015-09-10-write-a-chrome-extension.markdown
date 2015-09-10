---
layout: post
title:  "怎样写 Chrome 扩展"
date:   2015-09-10 Thu 08:20:51
categories: software
---

这里针对我写的[URL-to-QR-code](https://github.com/fjdu/URL-to-QR-code)这个扩展讲讲怎么写一个Chrome扩展。可以在[Chrome web store](https://chrome.google.com/webstore/detail/url-to-qr-code-converter/cnbelooiikkmhknahgkckaikndlmlhfd)安装这个扩展。用途是把当前打开的网页的网址转成二维码。

跟着Chrome官网的[Getting Started](https://developer.chrome.com/extensions/getstarted)做一遍就可以写一个最简单的扩展(extension)了。基本的步骤是这样的：

1. 编辑[```manifest.json```](https://github.com/fjdu/URL-to-QR-code/blob/master/src/manifest.json)文件。这个文件里列举了你的这个extension的基本信息，包括名字，功能描述，版本号，以及需要的权限，图标文件，启动这个程序时打开的html页面源文件，以及需要的javascript源文件，等等。 这个文件的内容之后可以不断调整，不需要开始就完全确定。

2. 做个图标，```png```格式，19*19 pixel的。开始的时候就用[Getting Started](https://developer.chrome.com/extensions/examples/tutorials/getstarted/icon.png)提供的图标好了，可以之后再改。

3. 修改[```popup.html```](https://github.com/fjdu/URL-to-QR-code/blob/master/src/popup.html)文件。这个文件负责的是点击扩展的图标后出现的那个气泡页面的内容。在[URL-to-QR-code](https://github.com/fjdu/URL-to-QR-code)里，只是简单的加了两个```<div>```元素和一个```<a>```元素。第一个```<div>```元素用来显示一句话（没什么用），第二个```<div>```元素用来显示生成的QR code的图片(其实是一个```canvas```对象)，最后，那个```<a>```元素用来显示一个链接，点击这个链接可以把QR code下载为图片。

4. 修改[```popup.js```](https://github.com/fjdu/URL-to-QR-code/blob/master/src/popup.js)文件。这个文件最重要的一段就是添加了一个```EventListener```，也就是这句 ```document.addEventListener('DOMContentLoaded', function()```。它的作用是，当网页内容已经载入后，执行一个函数，这个函数的功能是获取当前页面的网址，把这个网址转换成二维码显示到```popup.html```的相应页面上，然后把二维码的图形数据转换成一个数据链接供下载为图片。

5. 如何在自己电脑上测试？Chrome的[Extension](chrome://extensions/)页面有个"Load unpacked extension"按钮，点击这个按钮可以载入自己电脑上的源代码，很方便测试你写的程序的功能；修改后点击reload就可以看修改的效果。

6. 写程序的过程中肯定会出现各种错误，这就需要用Chrome自带的```Inspect Element```功能来调试。点击extension的图标，弹出气泡窗口后在里面点击右键就可以看到```Inspect Element```了。

7. 调试完毕，觉得满意之后就可以提交到[Chrome web
   store](https://chrome.google.com/webstore/category/apps)了。需要先注册，注册需要$5，填一些表格，上传源代码和截屏图片，之后就可以提交了。提交完成大概几个小时后才会出现在Chrome
   web store的搜索结果里（这可能是因为背后有人工审核过程）。

刚开始需要注意的：

1. [```manifest.json```](https://github.com/fjdu/URL-to-QR-code/blob/master/src/manifest.json)文件里列出依赖的```javascript```文件时，不同文件的顺序至关重要。被依赖的文件要放在前面，提前载入。在[URL-to-QR-code](https://github.com/fjdu/URL-to-QR-code)这个扩展里，排第一的```javascript```是```jquery.min.js```，第二是```qrcode.js```；它们两个之间并无依赖关系，但是第三个```jquery.qrcode.js```依赖于前两个，而第四个```popup.js```依赖于第一个和第三个，所以放在最后。
2. 监听正确的事件。这个需要查询Chrome的文档，并且需要尝试。
3. 在```javascript```里修改正确的元素。这个也需要尝试。比如，通过```document.getElementById```获得一个元素后，你真正需要的内容很可能不是这个元素本身，而是它的子元素，所以必须再用```children[0]```获得（第一个）子元素。
