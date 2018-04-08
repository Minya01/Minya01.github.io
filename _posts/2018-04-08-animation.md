---
layout: post
title:  "学习animation动画"
date:   2018-04-08
categories: animation
---



### Animations基础模块

1.**Keyframes（关键帧）** - 定义动画的阶段和样式

2.**Animation Properties（动画属性）** - 给@keyframes分配特定的css并定义动画。



### Keyframes

keyframes(关键帧)是css动画的基础。可以定义动画时间轴上每个阶段的动画效果。

每个`@keyframes`包括：

1.**名称** - 如下例中的`pulse`

2.**阶段** - 动画的每个阶段以百分比（%）表示。`0%`表示动画开始状态，`100%`表示动画结束状态，可以在0-100之间添加多个中间状态。

3.**css属性** - 未动画时间轴的每个阶段定义css属性

例如：

```
@keyframes pulse {
  0% {
    transform: scale(0.1);
    opacity: 0;
  }
  60% {
    transform: scale(1.2);
    opacity: 1;
  }
  100% {
    transform: scale(1);
  }
}
```

>上述动画名称为：pulse。
>
>公有三个阶段
>
>>第一阶段（0%）：元素透明度为0，并缩小至默认大小的10%。
>>
>>第二阶段（60%）：元素完全透明，并放大到默认大小的120%。
>>
>>最后阶段（100%）：元素略微缩小至默认大小。



### Animation Properties（动画属性）

动画属性可以完成：

1.将关键帧`@keyframes`分配给想要制作动画的元素。

2.定义如何动画。

给元素（css选择器）添加想要的动画，必须添加以下两个动画属性才能使动画生效：

1.`animation-name` - 在动画中定义的动画名称 *@keyframes*

2.`animation-duration` - 动画持续时间，以秒（如2s）或者毫秒（如200ms）为单位

例如：

```
div {
    animation-duration:2s;
    animation-name:pulse;
}
```

简写：

```
div {
    animation: pulse 2s;
}
```

> 通过添加@keyframes和动画属性，完成了一个简单的动画。



#### 属性汇总

`animation-name` - 声明@keyframes要处理的动画名称。

`animation-duration` - 动画完成一个周期所需的时间长度。

`animation-timing-function` - 建立预设的加速曲线，如**ease**或**linear**。

`animation-delay` - 动画延时，元素被加载和动画序列开始之间的时间。

`animation-direction` - 设置循环动画的方向。应是向前播放、反向播放还是以交替循环播放。

`animation-iteration-count` - 执行动画的次数

`animation-fill-mode` - 设置在动画之前或之后应用的值。

`animation-play-state` - 暂停/播放动画。

例如：

```
@keyframes stretch {
  /* 动画 */
}

div {
  animation-name: stretch;
  animation-duration: 1.5s; 
  animation-timing-function: ease-out; 
  animation-delay: 0s;
  animation-direction: alternate;
  animation-iteration-count: infinite;
  animation-fill-mode: none;
  animation-play-state: running; 
}
```

或者

```
div {
  animation: 
    stretch
    1.5s
    ease-out
    0s
    alternate
    infinite
    none
    running;
}
```

> 以上属性的完整列表：
>
> | `animation-timing-function` | ease, ease-out, ease-in, ease-in-out, linear, cubic-bezier(x1, y1, x2, y2) (例如 cubic-bezier(0.5, 0.2, 0.3, 1.0)) |
> | --------------------------- | ------------------------------------------------------------ |
> | `animation-duration`        | Xs or Xms                                                    |
> | `animation-delay`           | Xs or Xms                                                    |
> | `animation-iteration-count` | X                                                            |
> | `animation-fill-mode`       | forwards, backwards, both, none                              |
> | `animation-direction`       | normal,reverse, alternate,alternate-reverse                  |
> | `animation-play-state`      | playing - 正在播放，paused - 已暂停                          |



### 多个动画

要给元素添加多个动画，只需要用逗号分隔，如：

```
div {
    animation: slideIn 2s,rotate 1.5s;
}

或者

div {
  animation: 
    slideIn 2s ease infinite alternate, 
    rotate 1.5s linear infinite alternate;
}
```













