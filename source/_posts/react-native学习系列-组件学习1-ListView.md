---
title: react-native学习系列--组件学习1(ListView)
date: 2016-08-26 10:20:34
tags:
---
接下来开始react-native组件的学习。组件(Components)是react-native的重要组成部分，对应于Android原生开发的控件。在原生开发中，列表控件ListView常常出现，在我们的应用中占有至关重要的地位。今天我们就来学习下，react-native中得ListView如何使用。

### 最简单的用法
下面的代码实现了一个最简单的ListView的使用：

```
var ListViewExample = React.createClass({
	getInitialState: function() {
  		var ds = new ListView.DataSource({rowHasChanged: (r1, r2) => r1 !== r2});
  		return {
   		 dataSource: ds.cloneWithRows(['row 1', 'row 2', 'row 3', 'row 4', 'row 5'
   		 , 'row 6', 'row 7', 'row 8']),
  		};
	},
	render: function() {
  		return (
    		<ListView
      		dataSource={this.state.dataSource}
     		renderRow={(rowData) => <Text>{rowData}</Text>}
    		/>
  		);
	},
});
```
效果图如下：  

![](/img/4/eight-row-list.png)  

在原生开发中，每一个ListView都需要有一个适配器Adapter，在Adapter中需要指定ListView的数据源和每一行的布局，这样ListView才能正确的显示。
在上面的代码中，ListView设置了两个属性：dataSource和renderRow。其中，dataSource即对应于原生开发中的数据源，renderRow即对应原生开发中的每一行的布局，这里采用了react得JSX语法，在javascript代码中嵌入xml代码。

### 点击事件
上面代码展示了ListView得基本用法，实现了用ListView展示数据源。但我们在使用过程中往往不单单只是要展示数据，有时我们也需要为ListView设置点击事件，在点击相应的Item的时候会有相应的操作。在原生开发中，设置点击事件可以通过调用ListView的onItemClickListener()方法监听ListView的点击事件，那么，在react-native中，我们怎样监听ListView的点击事件呢。

react-native中设置ListView的点击事件监听相对麻烦些(就我目前所了解到的方法来说)。首先，react-native中得ListView组件并没有发现类似setOnclik()的方法。不过有一个TouchableHighlight组件可以接收触摸事件。我们只需要在renderRow的最外层包一层TouchableHighlight就可以实现对点击事件的监听。比如下面这样：  

```
<ListView
	dataSource={this.state.dataSource}
    renderRow={(rowData) => <TouchableHighlight onPress={() => {}}><Text>{rowData}	</Text></TouchableHighlight>}
 />
```
不过上面这样写只是最简单的写法，但这样写的话每一个Item的响应动作都是一样的，没办法为每个Item指定对应的事件，并且点击事件直接在上面写的话也会比较难看。  
所以具体的做法应该是给ListView的renderRow属性直接赋值一个函数，函数在render函数之外实现，返回Item得布局文件并设置点击事件监听。

```
render: function() {
    return (
        <ListView
          dataSource={this.state.dataSource}
          renderRow={this._renderRow}
          />
    );
  },
```
_renderRow函数有四个参数(rowData: string, sectionID: number, rowID: number, highlightRow: (sectionID: number, rowID: number) => void),分别表示当前Item的行数据、组Id、行Id和每一个Item被选中时高亮的函数。代码如下： 

```
_renderRow: function(rowData: string, sectionID: number, rowID: number, highlightRow: (sectionID: number, rowID: number) => void) {
    var rowHash = Math.abs(hashCode(rowData));
    var imgSource = THUMB_URLS[rowHash % THUMB_URLS.length];
    return (
      <TouchableHighlight onPress={() => {
          this._pressRow(rowID);
          highlightRow(sectionID, rowID);
        }}>
        <View>
          <View style={styles.row}>
            <Image style={styles.thumb} source={imgSource} />
            <Text style={styles.text}>
              {rowData + ' - ' + LOREM_IPSUM.substr(0, rowHash % 301 + 10)}
            </Text>
          </View>
        </View>
      </TouchableHighlight>
    );
  },
```
从上面的代码中可以看到，在TouchableHighlight的onPress中，制定了this._pressRow(rowID)为每个Item点击时的处理函数，其中rowID即为每个Item的ID，所以根据rowID为不同的Item指定不同的点击事件。

```
_pressRow: function(rowID: number) {
    this._pressData[rowID] = !this._pressData[rowID];
    this.setState({dataSource: this.state.dataSource.cloneWithRows(
      this._genRows(this._pressData)
    )});
  },
```
上面的代码中，_pressData数组中保存了每个Item是否为按下状态，然后根据其状态决定每个Item按下时响应的事件。  
```
完整的代码在文章末尾
```

### 设置分割线
上面的代码最终出来的ListView是没有分割线的，而一般我们使用ListView都会设置一个分割线，一是比较美观，二是方便区分没一个Item。ListView有一个renderSeparator属性，其官方介绍如下：

```
renderSeparator function 
(sectionID, rowID, adjacentRowHighlighted) => renderable
If provided, a renderable component to be rendered as the separator below each row but not the last row if there is a section header below. Take a sectionID and rowID of the row above and whether its adjacent row is highlighted. 
```

很明显这是用来给ListView设置分割线的。具体用法如下：   

```
<ListView
    dataSource={this.state.dataSource}
    renderRow={this._renderRow}
    renderSeparator={this._renderSeparator}
/>
```
然后实现_renderSeparator，返回分割线的视图：  

```
_renderSpeparator: function(sectionID:number, rowID:number, adjacentRowHighlighted:bool){
	return (
		<View
			style={
				height: adjacentRowHighlighted ? 4 : 1,
				backgroundColor: adjacentRowHighlighted ? '#3B5998' : '#CCCCCC',
			}
			key={`${sectionId}-${rowID}`}
	);
}
```
这样，我们就完成了给ListVIew设置分割线了。

### 更多用法
ListView时Android中最强大的一个控件，其用法肯定不止上面那么简单，更多的功能和用法等待着我们在之后的使用过程中慢慢的发掘。

### 完整代码
完整的代码如下：

```
'use strict';

var React = require('react');
var ReactNative = require('react-native');
var {
  Image,
  ListView,
  TouchableHighlight,
  StyleSheet,
  RecyclerViewBackedScrollView,
  Text,
  View,
} = ReactNative;

// var UIExplorerPage = require('./UIExplorerPage');

var ListViewSimpleExample = React.createClass({
  statics: {
    title: '<ListView>',
    description: 'Performant, scrollable list of data.'
  },

  getInitialState: function() {
    var ds = new ListView.DataSource({rowHasChanged: (r1, r2) => r1 !== r2});
    return {
      dataSource: ds.cloneWithRows(this._genRows({})),
    };
  },

  _pressData: ({}: {[key: number]: boolean}),

  componentWillMount: function() {
    this._pressData = {};
  },

  render: function() {
    return (
        <ListView
          dataSource={this.state.dataSource}
          renderRow={this._renderRow}
          renderScrollComponent={props => <RecyclerViewBackedScrollView {...props} />}
          renderSeparator={this._renderSeparator}

        />
    );
  },

  _renderRow: function(rowData: string, sectionID: number, rowID: number, highlightRow: (sectionID: number, rowID: number) => void) {
    var rowHash = Math.abs(hashCode(rowData));
    var imgSource = THUMB_URLS[rowHash % THUMB_URLS.length];
    return (
      <TouchableHighlight onPress={() => {
          this._pressRow(rowID);
          highlightRow(sectionID, rowID);
        }}>
        <View>
          <View style={styles.row}>
            <Image style={styles.thumb} source={imgSource} />
            <Text style={styles.text}>
              {rowData + ' - ' + LOREM_IPSUM.substr(0, rowHash % 301 + 10)}
            </Text>
          </View>
        </View>
      </TouchableHighlight>
    );
  },

  _genRows: function(pressData: {[key: number]: boolean}): Array<string> {
    var dataBlob = [];
    for (var ii = 0; ii < 100; ii++) {
      var pressedText = pressData[ii] ? ' (pressed)' : '';
      dataBlob.push('Row ' + ii + pressedText);
    }
    return dataBlob;
  },

  _pressRow: function(rowID: number) {
    this._pressData[rowID] = !this._pressData[rowID];
    this.setState({dataSource: this.state.dataSource.cloneWithRows(
      this._genRows(this._pressData)
    )});
  },

  _renderSeparator: function(sectionID: number, rowID: number, adjacentRowHighlighted: bool) {
    return (
      <View
        key={`${sectionID}-${rowID}`}
        style={
          height: adjacentRowHighlighted ? 4 : 1,
          backgroundColor: adjacentRowHighlighted ? '#3B5998' : '#CCCCCC',
        }
      />
    );
  }
});

var THUMB_URLS = [
  require('./Thumbnails/like.png'),
  require('./Thumbnails/dislike.png'),
  require('./Thumbnails/call.png'),
  require('./Thumbnails/fist.png'),
  require('./Thumbnails/bandaged.png'),
  require('./Thumbnails/flowers.png'),
  require('./Thumbnails/heart.png'),
  require('./Thumbnails/liking.png'),
  require('./Thumbnails/party.png'),
  require('./Thumbnails/poke.png'),
  require('./Thumbnails/superlike.png'),
  require('./Thumbnails/victory.png'),
  ];
var LOREM_IPSUM = 'Lorem ipsum dolor sit amet, ius ad pertinax oportere accommodare, an vix civibus corrumpit referrentur. Te nam case ludus inciderint, te mea facilisi adipiscing. Sea id integre luptatum. In tota sale consequuntur nec. Erat ocurreret mei ei. Eu paulo sapientem vulputate est, vel an accusam intellegam interesset. Nam eu stet pericula reprimique, ea vim illud modus, putant invidunt reprehendunt ne qui.';

/* eslint no-bitwise: 0 */
var hashCode = function(str) {
  var hash = 15;
  for (var ii = str.length - 1; ii >= 0; ii--) {
    hash = ((hash << 5) - hash) + str.charCodeAt(ii);
  }
  return hash;
};

var styles = StyleSheet.create({
  row: {
    flexDirection: 'row',
    justifyContent: 'center',
    padding: 10,
    backgroundColor: '#F6F6F6',
  },
  thumb: {
    width: 64,
    height: 64,
  },
  text: {
    flex: 1,
  },
});

module.exports = ListViewSimpleExample;
```

效果图如下：

![](/img/4/final-effect.png)


















