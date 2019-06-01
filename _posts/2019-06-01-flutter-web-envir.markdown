---
layout: post
title:  "Flutter Web环境搭建"
date:   2019-06-01 14:30:00
categories: jekyll default
tags: featured
image:
---
自从谷歌I/O2019发布[Flutter web](https://flutter.dev/web)版本之后，有好多开发者尝试搭建环境，我也不例外，先前在公司电脑上搭建过一次，只是没有总结出教程，趁着周末在家无聊，再次在家里电脑搭建环境，由于官方文档中推荐是Visual Studio Code创建的项目，所以我就顺便研究了完全用命令行创建web项目。

本文在Mac环境下完成，windows环境大同小异，可作为参考。

官方英文文档：[https://github.com/flutter/flutter_web](https://github.com/flutter/flutter_web)

To use the Flutter SDK with the flutter_web preview make sure you have upgraded Flutter to at least v1.5.4 by running flutter upgrade from your machine.
在使用flutter_web预览版之前，要保证电脑中安装的Flutter版本是大于1.5.4。

以下是我机器的flutter doctor信息：
```
Doctor summary (to see all details, run flutter doctor -v):
[✓] Flutter (Channel stable, v1.5.4-hotfix.2, on Mac OS X 10.14.5 18F132, locale zh-Hans-CN)
[✓] Android toolchain - develop for Android devices (Android SDK version 28.0.3)
[✓] iOS toolchain - develop for iOS devices (Xcode 10.2.1)
[✓] Android Studio (version 3.4)
[✓] VS Code (version 1.34.0)
[✓] Connected device (1 available)
• No issues found!
```
## 搭建步骤

### 1、从github上面把flutter_web项目克隆到本地
git clone https://github.com/flutter/flutter_web.git
存放目录随意，不过建议存放目录跟flutter sdk同级，日后更新维护好找

### 2、安装flutter_web的编译工具webdev
```
flutter pub global activate webdev
```
安装过程可能出现如下信息，此为网友在windows上配置时出现的
```
Resolving dependencies…
It looks like pub.dartlang.org is having some trouble
Pub will wait for a while before trying to connect again.
Got socket error trying to find package webdev at https://pub.dartlang.org.
pub finished with exit code 69
```
此问题我没碰到过，如果出现上诉问题是因为网络问题，建议多尝试几次。

如果出现如下信息，则安装成功
```
Precompiling executables…
Precompiled webdev:webdev.
Installed executable webdev.
Activated webdev 2.0.6
```
上述信息中可能有一个Warning提示需要配置环境变量，按提示配置环境变量即可

可以尝试执行命令
```
flutter pub global run webdev
```

到此环境搭建成功了。

### 3、创建和启动项目

#### 3.1使用现有项目
flutter_web目录下有examples几个demo项目，比如：hello_world
```
cd <flutter_web目录>/examples/hello_world
```
执行
```
flutter pub upgrade
或
flutter pub get
```
如果出现
```
RandyWeideMacBook-Pro:hello_world wei$ flutter pub get
! flutter_web 0.0.0 from path ../../packages/flutter_web                
! flutter_web_ui 0.0.0 from path ../../packages/flutter_web_ui          
Running "flutter packages get" in hello_world...                   21.9s
```
说明项目配置成功了，然后就是启动本地服务，官方的命令是：```webdev serve```
实际使用过程中这个命令并不对
需要使用
```
flutter pub global run webdev serve
```

出现以下信息就是成功了
```
RandyWeideMacBook-Pro:hello_world wei$ flutter pub global run webdev serve
[INFO] Building new asset graph completed, took 1.5s
[INFO] Checking for unexpected pre-existing outputs. completed, took 1ms
[INFO] Serving `web` on http://localhost:8080
[INFO] Running build completed, took 18.3s
[INFO] Caching finalized dependency graph completed, took 201ms
[INFO] Succeeded after 18.5s with 556 outputs (3217 actions)
[INFO] ------------------------------------------------------------------------
```
然后就可以在浏览器使用信息中的地址 http://localhost:8080 访问到了。

#### 3.2 创建新项目
官方给两种途径创建新项目
1）使用Visual Studio Code，具体配置Flutter Dart插件就不多说。使用命令面板Flutter: New Web Project，就可以创建一个新项目了，等配置完成后，按F5或者Debug -> Start Debugging，就可以启动服务并自动打开浏览器。

2）使用IntelliJ，没有研究

3）使用Android Studio，因为本身我是开发安卓的，习惯使用Studio进行开发。然而studio并不能创建一个普通的dart项目，或者flutter web项目，相信后期官方一定是能支持的。

目前想使用studio写代码的，可以使用Visual Studio Code，创建完项目完之后，再用Android Studio打开项目，也是可以的。

不过对于没有安装Visual Studio Code的同学来说的话，还可以用命令行来创建项目。

命令行创建web项目需要安装另一个工具
```
flutter pub global activate stagehand
```
跟安装webdev一样
安装成功后可以执行下面命令查看帮助

```
flutter pub global run stagehand

Stagehand will generate the given application type into the current directory.
usage: stagehand <generator-name>
    --[no-]analytics    Opt out of anonymous usage and crash reporting.
-h, --help              Help!
    --version           Display the version for stagehand.
    --author            The author name to use for file headers.
                        (defaults to "<your name>")

Available generators:
  console-full        - A command-line application sample.
  flutter-web-preview - A simple Flutter Web app.
  package-simple      - A starting point for Dart libraries or applications.
  server-shelf        - A web server built using the shelf package.
  web-angular         - A web app with material design components.
  web-simple          - A web app that uses only core Dart libraries.
  web-stagexl         - A starting point for 2D animation and games.

```
可以看到这个工具可以按照7种模版创建项目，我们使用的是flutter-web-preview，其他模版有兴趣的可以自行研究。

创建一个项目目录，然后
```
cd 目录
```
执行命令
```
flutter pub global run stagehand flutter-web-preview
```

一个web项目就创建完成了，可以使用studio打开项目，使用```flutter pub get```配置好完之后，可以进行正常的代码了。




[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
