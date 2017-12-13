---
layout: post
title: 'iOS代码规范文档'
subtitle: 'iOS代码规范文档'
date: 2017-12-10
categories: 技术
cover: 'https://ss0.bdstatic.com/70cFuHSh_Q1YnxGkpoWK1HF6hhy/it/u=75153069,1926417559&fm=27&gp=0.jpg'
tags: pop动画
---
> 本文档用于长天资产iOS开发团队，希望团队成员能够认证查看以及遵守代码规范。便于开发以及以后的维护。


### 命名规范

总的来说, iOS命名两大原则是:可读性高和防止命名冲突(通过加前缀来保证). Objective-C 的命名通常都比较长, 名称遵循驼峰式命名法. 一个好的命名标准很简单, 就是做到在开发者一看到名字时, 就能够懂得它的含义和使用方法. 另外, 每个模块都要加上自己的前缀, 前缀在编程接口中非常重要, 可以区分软件的功能范畴并防止不同文件或者类之间命名发生冲突, 比如相册模块(PhotoGallery)的代码都以PG作为前缀: PGAlbumViewController, PGDataManager.
#### 1、常量的命名
对于常量的命名最好在前面加上字母k作为标记. 如:

```cpp
static const NSTimeInterval kAnimationDuration = 0.3;
```
定义作为NSDictionary或者Notification等的Key值字符串时加上const关键字, 以防止被修改. 如:

```cpp
NSString *const UIApplicationDidEnterBackgroundNotification
```


### 4、函数调用
函数调用的格式和书写差不多，可以按照函数的长短来选择写在一行或者分成多行：

```cpp
//写在一行
[myObject doFooWith:arg1 name:arg2 error:arg3];

//分行写，按照&#039;:&#039;对齐
[myObject doFooWith:arg1
               name:arg2
              error:arg3];

//第一段名称过短的话后续可以进行缩进
[myObj short:arg1
          longKeyword:arg2
    evenLongerKeyword:arg3
                error:arg4];

```

以下写法是错误的：

```cpp
//错误，要么写在一行，要么全部分行
[myObject doFooWith:arg1 name:arg2
              error:arg3];
[myObject doFooWith:arg1
               name:arg2 error:arg3];

//错误，按照:来对齐，而不是关键字
[myObject doFooWith:arg1
          name:arg2
          error:arg3];

```

### 5、协议（Protocols）
在书写协议的时候注意用<>括起来的协议和类型名之间是没有空格的，比如IPCConnectHandler()<IPCPreconnectorDelegate>,这个规则适用所有书写协议的地方，包括函数声明、类声明、实例变量等等：



###  书写规范
编码规范简单来说就是为了保证写出来的代码具备三个原则:可复用, 易维护, 可扩展. 这其实也是面向对象的基本原则. 可复用, 简单来说就是不要写重复的代码, 有重复的部分要尽量封装起来重用. 否则修改文件的时候得满地找相同逻辑的地方...这个就用no zuo no die来描述吧, 哈哈...易维护, 就是不要把代码复杂化, 不要去写巨复杂逻辑的代码, 而是把复杂的逻辑代码拆分开一个个小的模块, 这也是Do one thing的概念, 每个模块(或者函数)职责要单一, 这样的代码会易于维护, 也不容易出错. 可扩展则是要求写代码时要考虑后面的扩展需求
### 1、判断nil或者YES/NO
Preferred:

```cpp
if (someObject) { ... }
if (!someObject) { ... }
```
Not preferred:

```cpp
if (someObject == YES) { ...}
if (someObject != nil) { ...}
```
if (someObject == YES)容易误写成赋值语句, 自己给自己挖坑了...而且if (someObject)写法很简洁, 何乐而不为呢?

### 2、 条件赋值
Preferred:

```cpp
result = object ? : [self createObject];
```
Not preferred::

```cpp
result = object ? object : [self createObject];
```
如果是存在就赋值本身, 那就可以这样简写, 多简洁

### 3、 初始化方法
Preferred:

```cpp
NSArray *names = @[@"Brian", @"Matt", @"Chris", @"Alex", @"Steve"];
NSDictionary *productManagers = @{@"iPhone" : @"Kate", @"iPad" : @"Kamal"};
NSNumber *shouldUseLiterals = @YES;
NSNumber *buildingZIPCode = @10018;
```
第一个好处还是简洁, 第二个好处是可以防止初始化进去nil值造成crash

### 4、 定义属性
Preferred:

```cpp
@property (nonatomic, readwrite, copy) NSString *name;
```
建议定义属性的时候把所有的参数写全, 尤其是如果想定义成只读的(防止外面修改)那一定要加上readonly, 这也是代码安全性的一个习惯.

如果是内部使用的属性, 那么就定义成私有的属性(定义到.m的class extension里面)

对于拥有Mutable子类型的对象(e.g. NSString, NSArray, NSDictionary)一定要定义成copy属性. Why? 示例: NSArray的array = NSMutableArray的mArray; 如果mArray在某个地方改变了, 那array也会跟着改变.

尽量不要暴露mutable类型的对象在public interface, 建议在.h定义一个Inmutable类型的属性, 然后在.m的get函数里面返回一个内部定义的mutable变量.

### 5、 BOOL赋值
Preferred:

```cpp
BOOL isAdult = age > 18;
```
Not preferred:

```cpp
BOOL isAdult;
if (age > 18)
{
    isAdult = YES;
}
else
{
    isAdult = NO;
}
```
### 6、 拒绝死值
Preferred:

```cpp
if (car == Car.Nissan)
or
const int adultAge = 18; if (age > adultAge) { ... }

```
Not preferred:

```cpp
if (carName == "Nissan")
or
if (age > 18) { ... }
else
{
    isAdult = NO;
}
```
死值每次修改的时候容易被遗忘, 地方多了找起来就悲剧了. 而且定义成枚举或者static可以让错误发生在编译阶段. 另外仅仅看到一个数字, 完全不知道这个数字代表的意义.

### 7、 复杂的条件判断

Preferred:


### 8、嵌套判断

Preferred:

```cpp
if (!user.UserName) return NO;
if (!user.Password) return NO;
if (!user.Email) return NO;

return YES;
```
Not preferred:

```cpp
BOOL isValid = NO;
if (user.UserName)
{

    if (user.Password)
    {

        if (user.Email) isValid = YES;
    }

}
return isValid;
```
一旦发现某个条件不符合, 立即返回, 条理更清晰


### 9、参数过多

Preferred:

```cpp
- (void)registerUser(User *user)
{

     // to do...

}
```
Not preferred:



一旦发现某个条件不符合, 立即返回, 条理更清晰

## Block的循环引用问题
Block确实是个好东西, 但是用起来一定要注意循环引用的问题,

```cpp
__weak typeof(self) weakSelf = self;
dispatch_block_t block =  ^{
    [weakSelf doSomething]; // weakSelf != nil
    // preemption, weakSelf turned nil
    [weakSelf doSomethingElse]; // weakSelf == nil
};
```

如此在上面定义一个weakSelf, 然后在block体里面使用该weakSelf就可以避免循环引用的问题. 那么问题来了...是不是这样就完全木有问题了? 很不幸, 答案是NO, 还是有问题。问题是block体里面的self是weak的, 所以就有可能在某一个时段self已经被释放了, 这时block体里面再使用self那就是nil


2.在一个自定义的View中,或者自定义cell中,modal出一个控制器建议:

```cpp
[UIApplication sharedApplication].keyWindow.rootViewController
```
3、建议:用CGSizeZero 代替 CGSizeMake(0,0);CGRectZero代替CGRectMake(0, 0, 0, 0);CGPointZero代替CGPointMake(0, 0)


## 关于我们的项目(快车财富)
我们的项目基于MVC的框架，采用cocoaPad的第三方管理平台。
### 1、MVC特征
1）Modal 模型对象：

模型对象封装了应用程序的数据，并定义操控和处理该数据的逻辑和运算。例如，模型对象可能是表示商品数据 list。用户在视图层中所进行的创建或修改数据的操作，通过控制器对象传达出去，最终会创建或更新模型对象。模型对象更改时（例如通过网络连接接收到新数据），它通知控制器对象，控制器对象更新相应的视图对象。
2）View 视图对象：

视图对象是应用程序中用户可以看见的对象。视图对象知道如何将自己绘制出来，可能对用户的操作作出响应。视图对象的主要目的就是显示来自应用程序模型对象的数据，并使该数据可被编辑。尽管如此，在 MVC 应用程序中，视图对象通常与模型对象分离。

在iOS应用程序开发中，所有的控件、窗口等都继承自 UIView，对应 MVC 中的 V。UIView 及其子类主要负责 UI 的实现，而 UIView 所产生的事件都可以采用委托的方式，交给 UIViewController 实现。

3）Controller 控制器对象：

在应用程序的一个或多个视图对象和一个或多个模型对象之间，控制器对象充当媒介。控制器对象因此是同步管道程序，通过它，视图对象了解模型对象的更改，反之亦然。控制器对象还可以为应用程序执行设置和协调任务，并管理其他对象的生命周期。

控制器对象解释在视图对象中进行的用户操作，并将新的或更改过的数据传达给模型对象。模型对象更改时，一个控制器对象会将新的模型数据传达给视图对象，以便视图对象可以显示它。

对于不同的 UIView，有相应的 UIViewController，对应 MVC 中的 C。例如在 iOS 上常用的 UITableView，它所对应的 Controller 就是UITableViewController。

### 2、cocoaPod
1、iOS开发时，项目中会引用许多第三方库，CocoaPods（https://github.com/CocoaPods/CocoaPods）可以用来方便的统一管理这些第三方库。
2、pod使用，可自行百度。通过命令行，可进行安装和使用。方法可查看该链接
（http://blog.csdn.net/showhilllee/article/details/38398119/）
### 快车财富的框架
![屏幕快照 2017-01-12 下午3.31.46](media/14837809856167/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202017-01-12%20%E4%B8%8B%E5%8D%883.31.46.png)
1) App目录：里面就是放了AppDelegate.h和AppDelegate.m文件，另起文件夹保留目录层级；
2) category目录：放入iOS的类目，以及扩展方法
3) config目录：放入配置，比如版本号，比如接口域名，以及使用第三方平台的key以及Secret
4) baseclassess目录：放入父类，可以继承使用；
5) api目录：接口的请求；
6) utils目录：工具类，比如时间解析，加密工具；
7) Modules目录：放入ViewController,view,model,里面再按照首页，理财，我的财富，更多进行区分；
8) ThildParts目录：第三方的代码，可以自己修改；
9) pod目录：通过pod使用的第三方平台，不能够修改

### 快车财富在我的眼中的功能：

1）调用网络API。
我们目前使用AFNetWorking的网络获取框架，目前github star是 28000.链接https://github.com/AFNetworking/AFNetworking  让我们开发更简单。

2）页面展示
页面很好的组织，才能尽可能降低业务方代码的耦合度，尽可能降低业务方开发界面的复杂度，提高软件的效率，不卡顿。这是开发经验决定了，我认为app开发和前端开发还是有很大的区别的，我们必须考虑到内存的释放问题。这个就不多做解释了

3）数据的本地持久化。
本地持久化，我们这边这样的，用户登陆的后，我们会储存用户的基本信息。从而让下次启动时候，直接登陆。 当没网的情况下，我们获取接口的缓存。但是，暂时还没加入预加载功能，下次更新的时候，会多某些页面实现预加载。

4）动态部署方案。
因为苹果审核周期很长，我记得最开始的时候 是一周以上，甚至是半个月，所以动态部署也对我们很重要。这个 bang大神的jspath平台就是一个很好的动态部署平台。这是通过js语言编写的。但是我不提倡通过js来部署页面，建议用来修改bug.我们1.6.5以及1.6.4版本的几个崩溃bug，我采用jspath修复的，来降低软件的崩溃率。还是显而易见的。


