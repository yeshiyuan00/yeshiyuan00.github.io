---
title: react-native学习系列--组件学习2(其它常用控件)
date: 2016-08-26 15:49:33
tags:
---
在原生开发中，除了ListView以外，最常用的控件就是TextView、ImageView、Button和EditText这些了，下面我们来看看在react-native中时如何使用这些组件的。

### Text
Text是react-native的一个基本组件，对应于原生Android中的TextView控件，主要用于显示基本的文本信息。除了显示基本的文本信息以外，还可以嵌套显示，设置样式以及处理点击事件。
下面是一个Text组件的基本用法：

```  

import React, { Component } from 'react';
import { AppRegistry, Text, StyleSheet } from 'react-native';

class TextInANest extends Component {
  constructor(props) {
    super(props);
    this.state = {
      titleText: "Bird's Nest",
      bodyText: 'This is not really a bird nest.'
    };
  }

  render() {
    return (
      <Text style={styles.baseText}>
        <Text style={styles.titleText} onPress={this.onPressTitle}>
          {this.state.titleText}<br /><br />
        </Text>
        <Text numberOfLines={5}>
          {this.state.bodyText}
        </Text>
      </Text>
    );
  }
}

const styles = StyleSheet.create({
  baseText: {
    fontFamily: 'Cochin',
  },
  titleText: {
    fontSize: 20,
    fontWeight: 'bold',
  },
});

// App registration and rendering
AppRegistry.registerComponent('TextInANest', () => TextInANest);  

```
效果图如下：  

![](/img/5/text-demo.png)

冲上面的代码中可以看到，Text的样式是可以嵌套的，里层的Text会使用外层Text的样式，如果外层样式和里层样式冲突，则里层的Text使用里层样式。通过设置不同的样式可以对文本进行各种个性化的定制。titleText通过设置onPress={this.onPresstitle}设置了点击标题时的响应事件，此时，Text即相当于一个按钮Button。  

### Image
Image是react-native中加载显示图片的控件，可以加载网络图片，本地资源图片,打包的APP中的图片资源以及手机文件中的图片(相册等).下面介绍加载各种不同类型图片的方法：

#### 加载网络图片
加载网络图片的代码如下： 
 
```  

<Image 
	source={{uri: 'https://facebook.github.io/react/img/logo_og.png'}}
    style={{width: 200, height: 200}} 
/>   

```

通过上面的方法即可加载网络图片资源，需要注意的是必须给Image设置大小样式，因为react-native中的Image是没有类似Android原生开发中的wrap_content的属性，如果不设置图片大小，系统无法计算图片的大小，最终导致图片无法显示。

#### 加载本地图片
本地图片是指存放在react项目目录下得图片资源，加载本地图片代码如下：

```  

<Image
	style={styles.stretch}
    source={require('./img/beauty.jpg')}>
    	<Text>Inside Words</Text>
</Image>

```
上面代码中，在Image的内部嵌套了一个Text，其作用是在Image图片上面显示文字。
效果图如下：  
 
![](/img/5/image-demo1.png)

#### 加载使用APP中的图片

我们很多时候并不单纯的使用react-native进行开发，而是搭配原生进行混合开发。所以有些时候，我们需要使用app中得图片(对应Android项目中得drawable文件夹下面的图片)。下面的代码即可实现加载使用APP中得图片：  

```   

<Image 
	source={{uri:'ic_launcher'}} 
	style={{width: 40, height: 40}} 
/>  

```
上面代码的Image加载显示了drawable目录下的ic_launcher图片。

#### 设置图片样式
通过设置Image的style可以设置图片的显示样式，Image样式主要有以下属性：
 
- FlexBox  支持弹性盒子风格   
- Transforms  支持属性动画                
- resizeMode  设置缩放模式
- backgroundColor 背景颜色
- borderColor     边框颜色              
- borderWidth 边框宽度
- borderRadius  边框圆角
- overflow 设置图片尺寸超过容器可以设置显示或者隐藏('visible','hidden')
- tintColor  颜色设置         
- opacity 设置不透明度0.0(透明)-1.0(完全不透明)  


### TextInput 

TextInput即文本输入框，对应Android原生开发中的EditText。文本输入框最常的用法就是通过键盘输入到TextInput中，然后提交输入框中得数据。其中一种做法就是增加一个按钮，然后再点击按钮的时候提交TextInput中得数据。除此之外，TextInput还可以订阅onChangeText事件来监听TextInput的内容变化。下面是一个示例代码：

```  
import React, { Component } from 'react';
import { AppRegistry, TextInput } from 'react-native';

class UselessTextInput extends Component {
  constructor(props) {
    super(props);
    this.state = { text: 'Useless Placeholder' };
  }

  render() {
    return (
      <View>
      	<TextInput
          style={{height: 40, borderColor: 'gray', borderWidth: 1}}
          onChangeText={(text) => this.setState({text})}
          value={this.state.text}
        />
        <Text>{this.state.text}</Text>
        </View>
    );
  }
}

// App registration and rendering
AppRegistry.registerComponent('AwesomeProject', () => UselessTextInput);  
```
效果图如下：  

![](/img/5/textinput-demo.png)


### 更多组件
除了以上组件以外，react-native还提供了其他大量的组件可供使用，更多组件在之后的项目运用中逐步学习。