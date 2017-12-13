---
layout: post
title: 'pop动画开发'
date: 2017-12-13
author: 简书
cover: 'https://ss1.bdstatic.com/70cFvXSh_Q1YnxGkpoWK1HF6hhy/it/u=1191204864,115906393&fm=27&gp=0.jpg'
tags: pop动画
---

> 动画在APP开发过程中，大家多多少少都会接触到，而且随着iOS7的扁平化风格启用之后，越来越多的APP开始尝试加入各种绚丽的动画交互效果以增加APP的用户体验。(当然，还是以国外的APP居多)
有过相关开发经验的同学肯定知道在iOS中，动画相关的部分都是基于Core Animation，但是今天我们不讨论Core Animation。今天的主角是POP -来自于Facebook的动画引擎(其实我不喜欢把POP定义为动画引擎 我愿意称它为函数发生器)

### 介绍

官方介绍(翻译版)

POP是一个在iOS与OS X上通用的极具扩展性的动画引擎。它在基本的静态动画的基础上增加的弹簧动画与衰减动画，使之能创造出更真实更具物理性的交互动画。POP的API可以快速的与现有的ObjC代码集成并可以作用于任意对象的任意属性。

POP是个相当成熟且久经考验的框架，Facebook出品的令人惊叹的Paper应用中的所有动画和效果即出自POP。

安装方式还是推荐使用CocoaPod。

```cpp
 pod 'pop', '~> 1.0'
 ```
POP的神奇之处在于，它是独立与Core Animation的存在。所以，忘记Core Animation吧，忘记Layer Tree吧，迎接一个简单的明天！(LOL 开玩笑的~:) 很多地方还是会需要Core Animation的，不过说不定哪天苹果大发善心，将动画相关的部分向POP借鉴一点也不是不可能的(比如SpriteKit就借鉴了Cocos2D :)

### 使用

POP默认支持三种动画，但同时也支持自定义动画。

POPBasicAnimation

POPSpringAnimation

POPDecayAnimation

POPCustomAnimation //自定义动画

这里我们只讨论前三种(因为自定义动画我也没用过 :) 先来看看官方的示例代码吧。
```cpp
//Basic animations can be used to interpolate values over a specified time period. To use an ease-in ease-out animation to animate a view's alpha from 0.0 to 1.0 over the default duration:
POPBasicAnimation *anim = [POPBasicAnimation animationWithPropertyNamed:kPOPViewAlpha];
anim.fromValue = @(0.0);
anim.toValue = @(1.0);
[view pop_addAnimation:anim forKey:@"fade"];
//Spring animations can be used to give objects a delightful bounce. In this example, we use a spring animation to animate a layer's bounds from its current value to (0, 0, 400, 400):
POPSpringAnimation *anim = [POPSpringAnimation animationWithPropertyNamed:kPOPLayerBounds];
anim.toValue = [NSValue valueWithCGRect:CGRectMake(0, 0, 400, 400)];
[layer pop_addAnimation:anim forKey:@"size"];
//Decay animations can be used to gradually slow an object to a halt. In this example, we decay a layer's positionX from it's current value and velocity 1000pts per second:
POPDecayAnimation *anim = [POPDecayAnimation animationWithPropertyNamed:kPOPLayerPositionX];
anim.velocity = @(1000.);
[layer pop_addAnimation:anim forKey:@"slide"];
 ```

### POPSpringAnimation
POPSpringAnimation也许是大多数人使用POP的理由，其提供一个类似弹簧一般的动画效果(使用后，APP立马就活泼起来了，有木有?!)
```cpp
POPSpringAnimation *anSpring = [POPSpringAnimation animationWithPropertyNamed:kPOPLayerPositionX];
anSpring.toValue = @(self.square.center.y+300);
anSpring.beginTime = CACurrentMediaTime() + 1.0f;
anSpring.springBounciness = 10.0f;
[self.square pop_addAnimation:anSpring forKey:@"position"];
 ```
POPSpringAnimation可配置的属性与默认值为

springBounciness:4.0    //[0-20] 弹力 越大则震动幅度越大
springSpeed     :12.0   //[0-20] 速度 越大则动画结束越快
dynamicsTension :0      //拉力  接下来这三个都跟物理力学模拟相关 数值调整起来也很费时 没事不建议使用哈
dynamicsFriction:0      //摩擦 同上
dynamicsMass    :0      //质量 同上

注意:POPSpringAnimation是没有duration字段的，其动画持续时间由以上几个参数决定


###小结

其实只需要熟练掌握POP自带的三种动画，即可完成大部分的动画效果。如果实在是无法满足你的需求的话，自定义动画也基本可以满足你的要求。可以说POP化繁为简的出现，极大的方便了我们这些苦逼的coder。(http://www.cocoachina.com/ios/20150728/12734.html)

当然，就像我说的，POP不仅仅是一个动画引擎。相信经过我最后一个例子，大家可以得到一点启示，POP能做的事情还不少 :)
