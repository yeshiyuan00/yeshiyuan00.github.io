---
title: react-native学习系列--弹性布局FlexBox
date: 2016-08-26 17:37:09
tags:
---
在Android原生开发中，我们经常要写各种各样的布局文件。最常用到的就是LinearLayout、RelativeLayout和FrameLayout这些布局空间了。我们之前写的代码中，都是一个View中包含各种子View，子View都是一个紧接着一个在垂直方向上排列。那么reac-native有没有提供给我们类似LinearLayout、RelativeLayout这种布局控件呢，答案时肯定的。  
我们在React Native中使用flexbox规则来指定某个组件的子元素的布局。FlexBox就是为在不同的屏幕尺寸上提供一致的布局二设计的。React Native中的Flexbox的工作原理和web上的CSS基本一致，当然也存在少许差异。

### 使用
我们通常使用flexDirection、alignItems和justifyContent的组合来实现我们的布局。

#### flexDirection
给组件的样式设置flexDirection属性可以指定组件布局的主轴方向。设置为"row"时为水平方向排列，设置为"column"时为垂直方向上排列，如果不设置的话，默认值为"column"，即默认为垂直排列，所以我们之前写的子View都是在垂直方向上排列的。示例代码如下

```
import React, { Component } from 'react';
import { AppRegistry, View } from 'react-native';

class FlexDirectionBasics extends Component {
  render() {
    return (
      // Try setting `flexDirection` to `column`.
      <View style={{flex: 1, flexDirection: 'row'}}>
        <View style={{width: 50, height: 50, backgroundColor: 'powderblue'}} />
        <View style={{width: 50, height: 50, backgroundColor: 'skyblue'}} />
        <View style={{width: 50, height: 50, backgroundColor: 'steelblue'}} />
      </View>
    );
  }
};

AppRegistry.registerComponent('AwesomeProject', () => FlexDirectionBasics

```
效果图如下：  
![](/img/6/flex-demo1.png)

### justifyContent

justifyContent可以决定其子元素沿着主轴的排列方式，比如是应该靠近主轴的起始端还是末尾段分布，对应有以下这些选项：flex-start、center、flex-end、space-around以及space-between。示例代码如下：

```

class JustifyContentBasics extends Component {
  render() {
    return (
      // Try setting `justifyContent` to `center`.
      // Try setting `flexDirection` to `row`.
      <View style={{
        flex: 1,
        flexDirection: 'column',
        justifyContent: 'space-between',
      }}>
        <View style={{width: 50, height: 50, backgroundColor: 'powderblue'}} />
        <View style={{width: 50, height: 50, backgroundColor: 'skyblue'}} />
        <View style={{width: 50, height: 50, backgroundColor: 'steelblue'}} />
      </View>
    );
  }
}


```

上面代码设置justifyContent的值为space-between，即设置空白在每个子View之间，效果图如下：  

![](/img/6/flex-demo2.png)


### alignItems

alignItems则是决定其子元素沿着次轴的排列方式(如果主轴时row，则次轴为cloumn，反之亦然)。对应的可选项有：flex-start、center、flex-end以及stretch。示例代码如下：

```

class AlignItemsBasics extends Component {
  render() {
    return (
      // Try setting `alignItems` to 'flex-start'
      // Try setting `justifyContent` to `flex-end`.
      // Try setting `flexDirection` to `row`.
      <View style={{
        flex: 1,
        flexDirection: 'column',
        justifyContent: 'center',
        alignItems: 'center',
      }}>
        <View style={{width: 50, height: 50, backgroundColor: 'powderblue'}} />
        <View style={{width: 50, height: 50, backgroundColor: 'skyblue'}} />
        <View style={{width: 50, height: 50, backgroundColor: 'steelblue'}} />
      </View>
    );
  }
}

```
上面的代码设置了子View在垂直方向上排列，主轴排列方式为中心排列，次轴排列方式也是中心排列。效果图如下：

![](/img/6/flex-demo3.png)


### flex

给子View设置flex值可以设置子View在父View中得权重，值越大，占的权重就越高。比如下面的代码：

```

class FlexDirectionBasics extends Component {
  render() {
    return (
      <View style={{flex: 1, flexDirection: 'row'}}>
        <View style={{flex: 1, height: 50, backgroundColor: 'powderblue'}} />
        <View style={{flex: 1, height: 50, backgroundColor: 'skyblue'}} />
        <View style={{flex: 1, height: 50, backgroundColor: 'steelblue'}} />
      </View>
    );
  }
  }


```

我们并没有给子View设置宽度，但是给每个子View的flex值设置为1,即在水平方向上每个子View所占的权重都一样。效果图如下：

![](/img/6/flex-demo4.png)











