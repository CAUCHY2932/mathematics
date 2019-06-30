## ios sdk

------

### 1. 接入SDK

使用CocoaPods安装

### 1.1 打开终端，安装 CocoaPods工具

```
gem install cocoapods
```

### 1.2 cd进入你工程的.xcodeproj所在目录，执行 pod init

```
pod init
```

### 1.3 在podfile文件中，加入一行代码

```
pod 'BaiduMobStatCodeless' // 无埋点SDK
```

### 1.4 执行如下代码，CocoaPods会自动安装SDK

```
pod install // 安装SDK
```

### 2. 编辑配置文件

打开主工程下Supporting Files文件夹下的info.plist文件，按照如下示例，新增mtj_appkey、mtj_deubglog两行参数，并写入以下appKey

```
c017888fbf // appKey
```

示例（下图为Demo工程中info.plist文件所在位置，供参考）

![工程图片](/Users/young/codes/invest/docs/15_%E6%9D%82%E8%B0%88/%E7%9B%AE%E5%BD%95.png)

### 3. 数据接入检测


编译项目，并启动运行

