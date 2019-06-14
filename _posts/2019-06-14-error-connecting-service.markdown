---
layout: post
title:  "Error connecting to the service protocol: HttpException: Connection closed before full header was received, uri = http://127.0.0.1:54627/p1NdakJReqU=/ws"
date:   2019-06-14 15:00:00
categories: jekyll default
tags: error
image:
---
很多人运行flutter项目时，出现如下错误
```

Error connecting to the service protocol: HttpException: Connection closed before full header was received, uri = http://127.0.0.1:54627/p1NdakJReqU=/ws

```

这个问题目前发现在安卓模拟器上出现频率较多，之前为了简单方便，都建议使用真机测试。
今天有时间寻找研究一下这个问题，多谢网友【itskamui】(网名)的协作一起研究。

情况1：
​![](https://www.bughub.dev/assets/article_images/2019-06-14-error-connecting-service/0.png) ​
可以看到日志里面有```No connected devices```，判断应该是模拟器没有链接成功，可以尝试各种
```adb devices -l```
或者
```flutter devices```
查看是不是能看到模拟器，如果看不到，重启模拟器，再试。

情况2：
通过正常的flutter run运行得到简单日志
```
Launching lib/main.dart on Android SDK built for x86 in debug mode...
Initializing gradle...
Resolving dependencies...
Running Gradle task 'assembleDebug'...
Built build/app/outputs/apk/debug/app-debug.apk.
Installing build/app/outputs/apk/app.apk...
Error connecting to the service protocol: HttpException: Connection closed before full header was received, uri = http://127.0.0.1:54627/p1NdakJReqU=/ws

```

使用flutter run -v可以输出更完整的日志
```
....
[ +1 ms] Stopping app 'app.apk' on Android SDK built for x86.
[ ] executing: /Users/wei/Library/Android/sdk/platform-tools/adb -s emulator-5554 shell am force-stop pt.tribeiro.flutter_pdf_viewer_example
[ +89 ms] executing: /Users/wei/Library/Android/sdk/platform-tools/adb -s emulator-5554 shell pm list packages pt.tribeiro.flutter_pdf_viewer_example
[ +31 ms] package:pt.tribeiro.flutter_pdf_viewer_example
[ +3 ms] executing: /Users/wei/Library/Android/sdk/platform-tools/adb -s emulator-5554 shell cat
/data/local/tmp/sky.pt.tribeiro.flutter_pdf_viewer_example.sha1
[ +26 ms] 01742bfabba2b904071ee66da7111e5f6bd5e7f2
[ ] Latest build already installed.
[ ] Android SDK built for x86 startApp
[ +1 ms] executing: /Users/wei/Library/Android/sdk/platform-tools/adb -s emulator-5554 shell am start -a android.intent.action.RUN -f 0x20000000
--ez enable-background-compilation true --ez enable-dart-profiling true --ez enable-checked-mode true --ez verify-entry-points true
pt.tribeiro.flutter_pdf_viewer_example/pt.tribeiro.flutter_plugin_pdf_viewer_example.MainActivity
[ +65 ms] Starting: Intent { act=android.intent.action.RUN flg=0x20000000
cmp=pt.tribeiro.flutter_pdf_viewer_example/pt.tribeiro.flutter_plugin_pdf_viewer_example.MainActivity (has extras) }
[ +1 ms] Waiting for observatory port to be available...
[ +939 ms] Observatory URL on device: http://127.0.0.1:41575/z9Mve5wOFL8=/
[ +1 ms] executing: /Users/wei/Library/Android/sdk/platform-tools/adb -s emulator-5554 forward tcp:0 tcp:41575
[ +8 ms] 53415
[ ] Forwarded host port 53415 to device port 41575 for Observatory
[ +7 ms] Connecting to service protocol: http://127.0.0.1:53415/z9Mve5wOFL8=/
[ +22 ms] Error connecting to the service protocol: HttpException: Connection closed before full header was received, uri = http://127.0.0.1:53415/z9Mve5wOFL8=/ws
[ +3 ms] "flutter run" took 9,137ms.
[ ] "flutter run" took 9,137ms.
....

```

Dart Observatory (语句级的单步调试和分析器) 是调试工具也要靠这个工具启动的服务来实现flutter热加载，有兴趣的可以查看文档[[https://dart-lang.github.io/observatory/](https://dart-lang.github.io/observatory/)]([https://dart-lang.github.io/observatory/](https://dart-lang.github.io/observatory/))研究一下

所以一开始以为是端口问题，使用参数```--observatory-port=```指定了端口，发现是无效的。
flutter sdk 的issues 上有好多类似问题。
[https://github.com/flutter/flutter/issues/6724](https://github.com/flutter/flutter/issues/6724)
[https://github.com/flutter/flutter/issues/13747](https://github.com/flutter/flutter/issues/13747)
有说需要用PowerShell运行的，经过尝试无效。
有说需要管理员身份运行的，尝试无效。

后来发现模拟器的系统镜像版本是Android 10或者是Android Q或者是Android 9.+(api 29)
​![](https://www.bughub.dev/assets/article_images/2019-06-14-error-connecting-service/1.png) ​

然后尝试下载使用低版本的系统，重新创建一个模拟器，问题就解决了。

猜测应该是Android Q目前还是beta版本，所以都会出现这个问题，目前在Android Q下还是没有办法能解决这个问题。

还有不排除设置了代理的可能性。

所以目前解决这个问题的办法是：
1、使用低版本的系统镜像
2、使用真机吧、真机吧、真机吧

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
