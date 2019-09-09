---
layout: post
title: flutter与android原生交互遇到的问题
permalink: /:title
tags: flutter
toc: true
desc: 首次使用flutter集成原生as项目
---

## flutter 混合开发运行报错：VM snapshot must be valid. /Check failed: vm. Must be able to initialize the VM.

`VM snapshot must be valid. Check failed: vm. Must be able to initialize the VM.`

gradle 根据 mainModuleName 去找 mergeAssets，如果 Android 项目没有配置 project.rootProject.ext.mainModuleName，就会默认用"app"这个名字去找 mergeAssets，而我们公司项目的 app module 的名字不是"`app`"，也没有配置 project.rootProject.ext.mainModuleName，所以没找到 mergeAssets，mergeAssets 为 null，就没有去执行 copyFlutterAssets。

至此原因找到，在 Android project 根目录 build.gradle 配置下 mainModuleName，clean Android module 和 flutter module，重新编译运行，搞定

这个问题阿里大佬也发现了 [阿里在 flutter 提交的 pr](https://github.com/flutter/flutter/pull/27154) 如下的 pr :

`include ':example' setBinding(new Binding([gradle: this, mainModuleName: 'example']))// new evaluate(new File( // new settingsDir.parentFile, // new 'my_flutter/.android/include_flutter.groovy' // new ))// new`

参考的 blog : [Android 原生页面跳转 flutter 页面时报错 Must be able to initialize the VM](https://blog.csdn.net/weixin_34413802/article/details/88174973)

完成后如下 可以正常运行安卓项目了 :
![非主流鼻祖](http://www.djangofreeman.com/assets/images/p/1561357881338.jpg)
