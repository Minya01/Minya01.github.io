---
layout: post
title:  "使用react-native开发expo应用"
date:   2018-04-01
categories: react-native
---

react native官方现在推荐的项目构建方式是create-react-native-app，并且建议使用expo这个app来查看效果。

首先，需要安装：

```
sudo npm install -g create-react-native-app
```

快速构建一个项目：

```
create-react-native-app MyReactApp

cd MyReactApp
npm start
```

出现如下选项，自行选择打开方式：

![](/assets/images/react-native-start.jpg)

使用手机expo软件打开如我本机的项目（exp://192.168.2.113:19002），修改app.js会立即自动重新reload：

![](/assets/images/expo-android.png)

在此之前需要安装Expo:

![](/assets/images/expo.jpg)

使用ios虚拟机:

```
//使用ios虚拟机
npm run ios
```

即可快速启动虚拟机，打开项目：

![](/assets/images/ios-expo.jpg)



据说使用Android安装expo的apk扫描QR Code的过程中出现了很多问题。（https://github.com/react-community/create-react-native-app/issues/270）

主要是二维码扫描经常没有效果外，而且打开时偶尔会报错。

![](/assets/images/expo-wrong.png)

注：另外可以使用expo xde直接在电脑运行启动项目。



