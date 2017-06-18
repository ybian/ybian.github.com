---
layout: post
title: "在 iPhone 上尝鲜 status.im"
date: 2017-06-18 13:36
comments: true
categories: 
---

## 1. status.im 是什么？

关于 status.im 究竟是什么，大家可以去它的官网[https://status.im](https://status.im)或者通过其它网络资料了解。下面给出我的定义：

> status.im 就是运行在去中心化网络上的微信。

这句话有两层含义：

1. status.im 就是微信，它可以或者有潜力做目前微信可以做的任何事情：即时消息、社区、钱包、转账、应用入口，等等。

2. 但是 status.im 跟微信的基础架构完全不一样：微信运行在中心化的网络（腾讯就是那个 Big Brother），而 status.im 运行在以以太坊技术构建的去中心化网络中。

怎么样？是不是很牛？这想象空间，懂的自然懂……

## 2. 怎么尝试 status.im？

当然，目前 status.im 还处在很早期的阶段，大家如果想尝鲜的话，只能去安装 alpha 版。安装方式：

1. 如果你用的是 Android 手机，可以直接在 Google Play 中下载
2. 如果你用的是 iPhone，需要在 TestFlight 中申请成为测试用户。

但是……但是……方法2现在基本上不可用，因为 TestFlight 测试用户人数不能超过 2000 人，现在早已满额。所以，如果你是 iPhone 用户，又迫切想尝鲜这个潜在的微信颠覆者，现在只剩下两条路可走：

1. 专门为了 status.im 搞一个 Android 手机
2. 自己动手，丰衣足食，自己下载源代码 build 然后安装到你的 iPhone 上

作为一名铁（nao）杆（can）果粉兼码农，我选择的是2，如果你也跟我一样的选择，请继续读下去。

## 3. 在 Mac 上自己 build 的步骤

虽然[官方wiki](https://wiki.status.im/contributing/development/building-status/)上有详细的 build 步骤，但是一则是英文的，二则不是针对 Mac 平台，三则写得也不是很清楚，所以，我认为还是必要把我的步骤简单归纳一下，方便大家。

### 前期准备
status.im 的 build 需要依赖很多工具，列表如下。如果你是一个开发者，可能很多都已经有了，有了就跳过，没有就按照给出的命令行安装。

* Homebrew: `brew update` 然后 `brew tap caskroom/cask`
* Node & NPM: `brew install node watchman`
* Lein: brew install leiningen
* react-native: `npm install -g react-native-cli`
* Latest JDK: `brew cask install java`
* Maven: `brew install maven`
* Cocoapods: `brew install cocoapods` （注意：官方 wiki 上给的安装方式是 `sudo gem install cocoapods`，但是我这样安装后找不到`pod`命令）

### Build 步骤

1. `git clone git@github.com:status-im/status-react.git -b master && cd status-react`
2. `lein deps`
3. `npm install`
4. `./re-natal deps`
5. `lein generate-externs`
6. `./re-natal use-figwheel`
7. `lein re-frisk use-re-natal`
8. `./re-natal enable-source-maps`
9. `mvn -f modules/react-native-status/ios/RCTStatus dependency:unpack`
10. `cd ios && pod install`

### 运行
 
如果上面各个步骤都成功了，就进入最后一步了。用 Xcode 打开 `ios/StatusIm.xcworkspace` 文件，然后点击 `Build & Run` 按钮，就可以在真机（前提是有开发者帐号并且正确配置）或者模拟器上运行了！

{% img /images/status_launch_screen.jpg %}

欢迎进入下一代微信，慢慢摸索吧，少年。:-)


