---
title: react-native学习系列-界面导航器(Navigator)
date: 2016-09-09 17:54:37
tags: react-native
---
### 前言 
在Android原生开发中，我们的App往往由许多个界面构成，这些个界面往往又是一个个Activity，一个界面即一个Activity，当然也有可能是一个Activity里面的多个Fragment。我们应用使用过程中就需要不断的切换这些个界面，在原生开发中，我们切换Activity非常简单，只需要在AndroidManifest中注册我们的Activity，然后实现如下代码：  

```
Intent intent = new Intent(FirstActivity.this,SecondActivity.class);
startActivity(intent);
```
就是这个简单，如果需要在不同的Activity之间传递数据，只需要在intent中传入我们需要的数据即可，这不在本文的讨论范围内。  
我们知道，当我们使用react-native开发时，只是在一个Activity中绑定了一个'index.android.js',这个js模块只是其中一个界面，那么要怎么实现多个界面呢？这个时候我们的Navigator就登场了！

### 概述
在官方文档中对Navigator的描述如下：  

```
Navigator handles the transition between different scenes in your app. It is implemented in JavaScript and is available on both iOS and Android. If you are targeting iOS only, you may also want to consider using NavigatorIOS as it leverages native UIKit navigation
```
我的英文不大好，就不翻译了，大体的意思就是说Navigator它控制了不同页面之间的跳转。我们一般将Navigator作为整个应用的根View，这样方便控制应用中各个不同界面的跳转。

###  使用
下面是官方提供的一个例子：
   
```
import React, { Component } from 'react';
import { Text, Navigator } from 'react-native';

export default class NavAllDay extends Component {
  render() {
    return (
      <Navigator
        initialRoute={{ title: 'Awesome Scene', index: 0 }}
        renderScene={(route, navigator) =>
          <Text>Hello {route.title}!</Text>
        }
        style={{padding: 100}}
      />
    );
  }
}
```

renderScene方法渲染各个界面的布局，我们需要为每一个界面啊都提供一个独一无二的route，这些route包含个每个界面所需要的信息。
上面的例子可能还不够直观，下面我们实现一个界面跳转的例子：  

```
render() {
  const routes = [
    {title: 'First Scene', index: 0},
    {title: 'Second Scene', index: 1},
  ];
  return (
    <Navigator
      initialRoute={routes[0]}
      initialRouteStack={routes}
      renderScene={(route, navigator) =>
        <TouchableHighlight onPress={() => {
          if (route.index === 0) {
            navigator.push(routes[1]);
          } else {
            navigator.pop();
          }
        }}>
        <Text>Hello {route.title}!</Text>
        </TouchableHighlight>
      }
      style={{padding: 100}}
    />
  );
}
```
可以看到我们提供了两个route，代表了有两个界面。我们在renderScene中根据route的内容来决定渲染的界面，navigator.push(route)将弹出一个新的界面，并将其压入栈中，弹出的界面由route来决定。navigator.pop()方法将当前的界面跳出，跳转到栈的上一个界面中。效果图如下：  
![](/img/9/firstpage.png)  
![](/img/9/secondpage.png)  
如上图，我们在第一个界面点击屏幕，跳转到第二个界面，第二个界面中点击屏幕，第二个界面将被弹出，然后跳转到栈中的上一个界面即第一个界面。  
### 方法
使用navigator默认的界面跳转动画是从右边进入的动画，要更改界面的跳转动画，可以通过设置configureScene来实现：  

```
<Navigator
  renderScene={(route, navigator) =>
    // ...
  }
  configureScene={(route, routeStack) =>
    Navigator.SceneConfigs.FloatFromBottom}
/>
```

上面的代码将跳转动画改为了下面进入的动画。当然，Navigator还为我们提供了很多其他的切换动画。    
navigator的方法：  

* getCurrentRoutes()    该进行返回存在的路由列表信息
* jumpBack()    该进行回退操作  但是该不会卸载(删除)当前的页面
* jumpForward()    进行跳转到相当于当前页面的下一个页面
* jumpTo(route)    根据传入的一个路由信息，跳转到一个指定的页面(该页面不会卸载删除)
* push(route)     导航切换到一个新的页面中，新的页面进行压入栈。通过jumpForward()方法可以回退过去
* pop()   当前页面弹出来，跳转到栈中下一个页面，并且卸载删除掉当前的页面
* replace(route)   只用传入的路由的指定页面进行替换掉当前的页面
* replaceAtIndex(route,index)     传入路由以及位置索引，使用该路由指定的页面跳转到指定位置的页面
* replacePrevious(route)    传入路由，通过指定路由的页面替换掉前一个页面
* resetTo(route)  进行导航到新的界面，并且重置整个路由栈
* immediatelyResetRouteStack(routeStack)   该通过一个路由页面数组来进行重置路由栈
* popToRoute(route)   进行弹出相关页面，跳转到指定路由的页面，弹出来的页面会被卸载删除
* popToTop()  进行弹出页面，导航到栈中的第一个页面，弹出来的所有页面会被卸载删除















