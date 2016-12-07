---
layout: post
title:  "简易翻墙方法"
date:   2016-12-07 Wed 01:26:04
categories: misc
---

# Windows 系统

在任何文件夹下 (比如桌面) 新建一个文本文件，比如叫做 `fanqiang.txt`，然后把下述内容拷贝到这个文件里并保存：

{% highlight vbscript %}
Set WshShell = WScript.CreateObject("WScript.Shell")
if WScript.Arguments.Length = 0 Then
    Set ObjShell = CreateObject("Shell.Application")
    ObjShell.ShellExecute "wscript.exe" _
      , """" & WScript.ScriptFullName & """ RunAsAdministrator", , "runas", 1
    WScript.Quit
end if


dim xHTTP: set xHttp = createobject("Microsoft.XMLHTTP")
dim bStrm: set bStrm = createobject("Adodb.Stream")
dim Fso
Set Fso = WScript.CreateObject("Scripting.FileSystemObject")

XHttp.Open "GET", "https://raw.githubusercontent.com/racaljk/hosts/master/hosts", False
XHttp.Send

with bStrm
    .type = 1 '//binary
    .open
    .write xHttp.responseBody
    Fso.MoveFile "C:\Windows\System32\drivers\etc\hosts", _
        "C:\Windows\System32\drivers\etc\hosts_backup" & _
        Replace(Replace(Replace(FormatDateTime(Now), "/", "_"), ":", "_"), " ", "_")
    .savetofile "C:\Windows\System32\drivers\etc\hosts", 2 '//overwrite
end with
{% endhighlight %}

然后，把文件的后缀名从本来的 `txt` 改为 `vbs`。然后双击运行，会提示是否允许运行；点击允许；稍等一会儿 (几秒到十几秒的样子)，应该就可以翻墙了。

这种方法可以让你顺利使用 Google (包括 Gmail, Google Drive), Facebook, Twitter, Dropbox 等网站。Google 偶尔可能需要输入验证码，照做就好。

使用 Google 时建议使用这个网址：[https://www.google.com/ncr](https://www.google.com/ncr)

如果觉得不好使了，尝试再次双击运行这个程序即可。

# Mac 系统

在终端执行下面的命令即可：
{% highlight bash %}
wget -O /etc/hosts https://raw.githubusercontent.com/racaljk/hosts/master/hosts
{% endhighlight %}
如果出现问题，可尝试
{% highlight bash %}
sudo wget -O /etc/hosts https://raw.githubusercontent.com/racaljk/hosts/master/hosts
{% endhighlight %}
后一种情况需要输入密码。

<img src="{{ site.url }}/pictures/2016-12-07-ladder.jpg" id="Ladder">
