---
layout: post
title:  "基于腾讯云播放器封装的Flutter Video Player插件"
date:   2019-05-26 12:00:00
categories: jekyll default
tags: featured
image:
---
Flutter版本现在已经更新到了v1.5.4-hotfix.2，在国内也越来越火。
在之前项目中有视频播放功能，使用了flutter官方出的video_player库完成实现。flutter官方所有插件，详见https://github.com/flutter/plugins
官方的video_player，安卓是基于ExoPlayer，苹果是基于原生的AVPlayer，目前项目使用中也没什么毛病。

在闲暇之余，出于学习开发flutter plugin目的，基于[腾讯云播放器](https://cloud.tencent.com/document/product/881)封装了一个flutter视频播放器。
目前项目在开发测试阶段，还没发布到pub平台上，可通过git配置使用。

项目github地址：[https://github.com/RandyWei/flt_video_player](https://github.com/RandyWei/flt_video_player)

播放器开放了部分腾讯云播放器的功能，如：倍速、静音等。

参考资料:
[https://github.com/An-uking/Flutter_IJKPlayer](https://github.com/An-uking/Flutter_IJKPlayer)
[https://github.com/CaiJingLong/flutter_ijkplayer](https://github.com/CaiJingLong/flutter_ijkplayer)
[https://github.com/flutter/plugins/tree/master/packages/video_player](https://github.com/flutter/plugins/tree/master/packages/video_player)

在项目开发过程中碰到了以下问题，作了一下总结：

1、出现label标签冲突相关问题，如下错误
```
Attribute application@label value=(flutter_plugin_demo_example) from AndroidManifest.xml:13:9-52
	is also present at [LiteAVSDK_Player_6.3.7089.aar] AndroidManifest.xml:19:9-41 value=(@string/app_name).
	Suggestion: add 'tools:replace="android:label"' to <application> element at AndroidManifest.xml:11:5-35:19 to override.
```
这个错误是在安卓端出现的，因为使用了LiteAVSDK，类似这种使用了各种第三方库包含```android:label```的，所以冲突了

解决办法：其实错误代码提示也很是明确，或者直接上谷歌去百度一下就可以得到答案，在mainfest Application标签中添加```tools:relpace=”label”```即可。

2、在使用EventChannel进行Flutter和Native通信过程中出现以下错误：
```
E/EventChannel#flutter_plugin_demo.event.status(15177): Failed to open event stream
E/EventChannel#flutter_plugin_demo.event.status(15177): java.lang.IllegalArgumentException: Parameter specified as non-null is null: method kotlin.jvm.internal.Intrinsics.checkParameterIsNotNull, parameter o
E/EventChannel#flutter_plugin_demo.event.status(15177): 	at com.chinahrt.flutter_plugin_demo2.FlutterPluginDemo2Plugin$onMethodCall$1.onListen(Unknown Source:2)
E/EventChannel#flutter_plugin_demo.event.status(15177): 	at io.flutter.plugin.common.EventChannel$IncomingStreamRequestHandler.onListen(EventChannel.java:181)
E/EventChannel#flutter_plugin_demo.event.status(15177): 	at io.flutter.plugin.common.EventChannel$IncomingStreamRequestHandler.onMessage(EventChannel.java:160)
E/EventChannel#flutter_plugin_demo.event.status(15177): 	at io.flutter.view.FlutterNativeView$PlatformMessageHandlerImpl.handleMessageFromDart(FlutterNativeView.java:188)
E/EventChannel#flutter_plugin_demo.event.status(15177): 	at io.flutter.embedding.engine.FlutterJNI.handlePlatformMessage(FlutterJNI.java:202)
E/EventChannel#flutter_plugin_demo.event.status(15177): 	at android.os.MessageQueue.nativePollOnce(Native Method)
E/EventChannel#flutter_plugin_demo.event.status(15177): 	at android.os.MessageQueue.next(MessageQueue.java:326)
E/EventChannel#flutter_plugin_demo.event.status(15177): 	at android.os.Looper.loop(Looper.java:160)
E/EventChannel#flutter_plugin_demo.event.status(15177): 	at android.app.ActivityThread.main(ActivityThread.java:6718)
E/EventChannel#flutter_plugin_demo.event.status(15177): 	at java.lang.reflect.Method.invoke(Native Method)
E/EventChannel#flutter_plugin_demo.event.status(15177): 	at com.android.internal.os.RuntimeInit$MethodAndArgsCaller.run(RuntimeInit.java:493)
E/EventChannel#flutter_plugin_demo.event.status(15177): 	at com.android.internal.os.ZygoteInit.main(ZygoteInit.java:858)
I/flutter (15177): ══╡ EXCEPTION CAUGHT BY SERVICES LIBRARY ╞══════════════════════════════════════════════════════════
I/flutter (15177): The following PlatformException was thrown while activating platform stream on channel
I/flutter (15177): flutter_plugin_demo.event.status:
I/flutter (15177): PlatformException(error, Parameter specified as non-null is null: method
I/flutter (15177): kotlin.jvm.internal.Intrinsics.checkParameterIsNotNull, parameter o, null)
I/flutter (15177):
I/flutter (15177): When the exception was thrown, this was the stack:
I/flutter (15177): #0      StandardMethodCodec.decodeEnvelope (package:flutter/src/services/message_codecs.dart:564:7)
I/flutter (15177): #1      MethodChannel.invokeMethod (package:flutter/src/services/platform_channel.dart:302:33)
I/flutter (15177): <asynchronous suspension>
I/flutter (15177): #2      EventChannel.receiveBroadcastStream.<anonymous closure> (package:flutter/src/services/platform_channel.dart:490:29)
I/flutter (15177): <asynchronous suspension>
I/flutter (15177): #7      VideoPlayerController.initialize (package:flutter_plugin_demo2/video_player.dart:33:10)
I/flutter (15177): <asynchronous suspension>
I/flutter (15177): #8      _MyAppState.initState (package:flutter_plugin_demo2_example/main.dart:25:9)
I/flutter (15177): #9      StatefulElement._firstBuild (package:flutter/src/widgets/framework.dart:3846:58)
I/flutter (15177): #10     ComponentElement.mount (package:flutter/src/widgets/framework.dart:3711:5)
I/flutter (15177): #11     Element.inflateWidget (package:flutter/src/widgets/framework.dart:2956:14)
I/flutter (15177): #12     Element.updateChild (package:flutter/src/widgets/framework.dart:2759:12)
I/flutter (15177): #13     RenderObjectToWidgetElement._rebuild (package:flutter/src/widgets/binding.dart:933:16)
I/flutter (15177): #14     RenderObjectToWidgetElement.mount (package:flutter/src/widgets/binding.dart:904:5)
I/flutter (15177): #15     RenderObjectToWidgetAdapter.attachToRenderTree.<anonymous closure> (package:flutter/src/widgets/binding.dart:850:17)
I/flutter (15177): #16     BuildOwner.buildScope (package:flutter/src/widgets/framework.dart:2253:19)
I/flutter (15177): #17     RenderObjectToWidgetAdapter.attachToRenderTree (package:flutter/src/widgets/binding.dart:849:13)
I/flutter (15177): #18     _WidgetsFlutterBinding&BindingBase&GestureBinding&ServicesBinding&SchedulerBinding&PaintingBinding&SemanticsBinding&RendererBinding&WidgetsBinding.attachRootWidget (package:flutter/src/widgets/binding.dart:736:7)
I/flutter (15177): #19     runApp (package:flutter/src/widgets/binding.dart:780:7)
I/flutter (15177): #20     main (package:flutter_plugin_demo2_example/main.dart:8:16)
I/flutter (15177): #21     _runMainZoned.<anonymous closure>.<anonymous closure> (dart:ui/hooks.dart:189:25)
I/flutter (15177): #26     _runMainZoned.<anonymous closure> (dart:ui/hooks.dart:180:5)
I/flutter (15177): #27     _startIsolate.<anonymous closure> (dart:isolate/runtime/libisolate_patch.dart:300:19)
I/flutter (15177): #28     _RawReceivePortImpl._handleMessage (dart:isolate/runtime/libisolate_patch.dart:171:12)
I/flutter (15177): (elided 8 frames from package dart:async)
I/flutter (15177): ════════════════════════════════════════════════════════════════════════════════════════════════════
```
这个问题还是出现在安卓平台上，因为安卓项目使用了kotlin支持，在写kotlin代码实现EventChannel.StreamHandler时
{% highlight kotlin %}
override fun onListen(o: Any?, sink: EventChannel.EventSink?) {
 }
override fun onCancel(o: Any?) {
}
{% endhighlight %}
参数应该可空。因误写成不可空，导致报错。
所以解决办法就是加?就可以

3、在iOS项目podspec上配置腾讯云播放器TXLiteAVSDK_Player支持时，出现以下问题
```
Undefined symbols for architecture x86_64:
      "vtable for __cxxabiv1::__si_class_type_info", referenced from:
          typeinfo for bssl::(anonymous namespace)::ECKeyShare in TXLiteAVSDK_Player(ssl_key_share.o)
          typeinfo for bssl::(anonymous namespace)::X25519KeyShare in TXLiteAVSDK_Player(ssl_key_share.o)
      NOTE: a missing vtable usually means the first non-inline virtual member function has no definition.
      "_sqlite3_column_text", referenced from:
          _qcloud_ijktsdb_meta_select in TXLiteAVSDK_Player(ijktsdb.o)
      "_sqlite3_bind_blob", referenced from:
          _qcloud_ijktsdb_insert in TXLiteAVSDK_Player(ijktsdb.o)
      "_sqlite3_bind_text", referenced from:
          _qcloud_ijktsdb_select in TXLiteAVSDK_Player(ijktsdb.o)
          _ijktsdb_check in TXLiteAVSDK_Player(ijktsdb.o)
          _qcloud_ijktsdb_insert in TXLiteAVSDK_Player(ijktsdb.o)
          _qcloud_ijktsdb_meta_select in TXLiteAVSDK_Player(ijktsdb.o)
          _qcloud_ijktsdb_meta_insert in TXLiteAVSDK_Player(ijktsdb.o)
      "_sqlite3_column_bytes", referenced from:
          _qcloud_ijktsdb_select in TXLiteAVSDK_Player(ijktsdb.o)
          _ijktsdb_check in TXLiteAVSDK_Player(ijktsdb.o)
      "_sqlite3_column_blob", referenced from:
          _qcloud_ijktsdb_select in TXLiteAVSDK_Player(ijktsdb.o)
      "_sqlite3_errcode", referenced from:
          _databaseError in TXLiteAVSDK_Player(ijktsdb.o)
      "_sqlite3_close", referenced from:
          _qcloud_ijktsdb_open in TXLiteAVSDK_Player(ijktsdb.o)
          _qcloud_ijktsdb_close in TXLiteAVSDK_Player(ijktsdb.o)
```
使用framwork配置无论怎样都配置不成功，如果有大佬知道怎么配置的，可以教教
最后还是使用了```pod：s.dependency 'TXLiteAVSDK_Player'``` 配置成功了。

参考资料：
[https://guides.cocoapods.org/syntax/podspec.html#dependency](https://guides.cocoapods.org/syntax/podspec.html#dependency)


4、在开发过程中出现以下错误：
```
Exception: ideviceinfo returned an error:
ERROR: Could not connect to lockdownd, error code -8

#0      IMobileDevice.getInfoForDevice (package:flutter_tools/src/ios/mac.dart:141:9)
<asynchronous suspension>
#1      IOSDevice.getAttachedDevices (package:flutter_tools/src/ios/devices.dart:156:55)
<asynchronous suspension>
#2      IOSDevices.pollingGetDevices (package:flutter_tools/src/ios/devices.dart:110:57)
#3      PollingDeviceDiscovery.devices (package:flutter_tools/src/device.dart:186:52)
<asynchronous suspension>
#4      DeviceManager.getAllConnectedDevices (package:flutter_tools/src/device.dart:114:46)
<asynchronous suspension>
#5      DeviceValidator.validate (package:flutter_tools/src/doctor.dart:705:54)
<asynchronous suspension>
#6      Doctor.startValidatorTasks (package:flutter_tools/src/doctor.dart:129:52)
#7      Doctor.diagnose (package:flutter_tools/src/doctor.dart:200:41)
#8      _AsyncAwaitCompleter.start (dart:async-patch/async_patch.dart:49:6)
#9      Doctor.diagnose (package:flutter_tools/src/doctor.dart:190:24)
#10     DoctorCommand.runCommand (package:flutter_tools/src/commands/doctor.dart:48:39)
#11     _AsyncAwaitCompleter.start (dart:async-patch/async_patch.dart:49:6)
#12     DoctorCommand.runCommand (package:flutter_tools/src/commands/doctor.dart:34:42)
#13     FlutterCommand.verifyThenRunCommand (package:flutter_tools/src/runner/flutter_command.dart:559:18)
#14     _asyncThenWrapperHelper.<anonymous closure> (dart:async-patch/async_patch.dart:77:64)
#15     _rootRunUnary (dart:async/zone.dart:1132:38)
#16     _CustomZone.runUnary (dart:async/zone.dart:1029:19)
#17     _FutureListener.handleValue (dart:async/future_impl.dart:126:18)
#18     Future._propagateToListeners.handleValueCallback (dart:async/future_impl.dart:639:45)
#19     Future._propagateToListeners (dart:async/future_impl.dart:668:32)
#20     Future._complete (dart:async/future_impl.dart:473:7)
#21     _SyncCompleter.complete (dart:async/future_impl.dart:51:12)
#22     _AsyncAwaitCompleter.complete.<anonymous closure> (dart:async-patch/async_patch.dart:33:20)
#23     _rootRun (dart:async/zone.dart:1124:13)
#24     _CustomZone.run (dart:async/zone.dart:1021:19)
#25     _CustomZone.bindCallback.<anonymous closure> (dart:async/zone.dart:947:23)
#26     _microtaskLoop (dart:async/schedule_microtask.dart:41:21)
#27     _startMicrotaskLoop (dart:async/schedule_microtask.dart:50:5)
#28     _runPendingImmediateCallback (dart:isolate-patch/isolate_patch.dart:115:13)
#29     _RawReceivePortImpl._handleMessage (dart:isolate-patch/isolate_patch.dart:172:5)
```
执行命令： ```ideviceinfo -d ```出现如下错误
```
13:18:45 idevice.c:295 idevice_connect(): ERROR: Connecting to usbmuxd failed: -9 (Bad file descriptor)
13:18:45 lockdown.c:663 lockdownd_client_new(): could not connect to lockdownd (device 1a95bb66796dad5ebf6a99d1c87101b5c2214963)
13:18:45 lockdown.c:698 lockdownd_client_new_with_handshake(): failed to create lockdownd client.
ERROR: Could not connect to lockdownd, error code -8
```
关于”Could not connect to lockdownd”这个错误，网上出现了很多解决办法，但每个人碰到的error code -8，错误码都不一样。有部分解决办法就是修改权限，对于错误码为 -8 的，一直没有搜到相关解决办法。折腾了一天，其实原因是，开发过程中，我碰到了手机，导致手机数据线松了，然后我更换了一个USB接口，总之是因为手机和电脑没有连接好。如果有碰到这个问题，换个接口，然后撤销iphone手机和电脑的信任，直到连接电脑时弹出信任授权对话框，才算是正常连接成功。一旦出现这个错误，连安卓设备都运行不了。

5、
```
Assertion failed: (AMDeviceIsPaired(device)), function handle_device, file /tmp/ios-deploy-20181117-87168-1goz8ak/ios-deploy-1.9.4/src/ios-deploy/ios-deploy.m, line 1598.
Could not install build/ios/iphoneos/Runner.app on 1a95bb66796dad5ebf6a99d1c87101b5c2214963.
Try launching Xcode and selecting "Product > Run" to fix the problem:
  open ios/Runner.xcworkspace

Error launching application on iPhone (5).
```
出现这个错误，直接按提示使用Xcode运行一下项目即可。

6、还有最后一个严重的内存泄漏的问题

错误日志：```Signal 11 was raised.```

在iOS实现将画面数据传送到Flutter，使用到了CVPixelBufferRef数据。对于这块目前不是很清楚，大概是因为线程安全的问题，在腾讯云播放器回调的数据，随时可能会被释放，而在Flutter回调中使用了已被释放的数据，出现了问题。最后参考了ijk代码，使用了OSAtomicCompareAndSwapPtrBarrier对数据进行验证比较得以解决，对于这个OSAtomicCompareAndSwapPtrBarrier方法也不是很理解，如果有大佬了解的，可以教教我。



[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
