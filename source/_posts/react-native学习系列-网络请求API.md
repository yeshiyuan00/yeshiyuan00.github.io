---
title: react-native学习系列-网络请求API
date: 2016-09-02 14:41:10
tags: react-native
---
在日常的App开发中，我们经常需要和服务器端进行通信，从服务器端取得数据，然后才显示到我们的App上面。原生Android开发可以使用HttpUrlConnection这个类来与服务器连接，也可以使用一些其他的网络请求框架，比如:Okhttp、retrofit、Volly等等。
react-native也给我们提供了类似的api，比如fetch api、WebSocket，也有其他的网络请求框架XMLHttpRequest。今天我们主要就来学习fetch api的用法。

### 使用 Fetch
Fetch API 是React-native提供的API，所以不需要添加其他依赖，即可以直接使用。

#### 发送网络请求
  
```
fetch('https://mywebsite.com/mydata.json')
```

上面是一个最简单的网络请求，直接使用fetch，参数即为要访问的URL。fetch有可选的第二个参数，可用来定制我们的网络请求Request。你可以在里面添加请求头部、请求方法、请求实体等等。如下：  

```
fetch('https://mywebsite.com/endpoint/', {
  method: 'POST',
  headers: {
    'Accept': 'application/json',
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    firstParam: 'yourValue',
    secondParam: 'yourOtherValue',
  })
})

```

#### 处理响应结果
上面我们发送了一个网络请求，在大部分情况下，我们发送请求后，需要接受服务器给我们的响应结果并处理。fetch方法返回的是一个[Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)对象，所以我们可以非常简单的实现异步处理响应结果。 如下： 

```
getMoviesFromApiAsync() {
    return fetch('http://facebook.github.io/react-native/movies.json')
      .then((response) => response.json())
      .then((responseJson) => {
        return responseJson.movies;
      })
      .catch((error) => {
        console.error(error);
      });
  }
```
上面代码中response即为服务器返回给我们的结果，我们将其转换为json对象，然后取出键值为movies的内容。如果在网络请求的过程中发生异常，则通过catch可以捕获异常。  
当然也可以使用 ES2017 的新语法async/await:  

```
async getMoviesFromApi() {
    try {
      let response = await fetch('http://facebook.github.io/react-native/movies.json');
      let responseJson = await response.json();
      return responseJson.movies;
    } catch(error) {
      console.error(error);
    }
  }
```

我们将查询到得结果用Text显示出来，如下：
  
```
class ShowNetInfoVIew extends Component{
	constructor(props) {
		super(props);
  		this.state = {movies: []};
	}

	async getMoviesFromApi() {
    	try {
      		let response = await fetch('http://facebook.github.io/react-native/movies.json');
      		let responseJson = await response.json();
      		console.log('====>')
      		this.setState({movies: responseJson.movies});
    	} catch(error) {
      		console.error(error);
    	}
	}

	componentDidMount(){
		this.getMoviesFromApi();
	}
	render() {
		const {movies} = this.state;
		return (
			<Text>from server {movies.map(item => <Text>{`title: ${item.title}`}</Text>)} </Text>
		)
	}
}
```
效果图如下：  

![](/img/7/show-movies.png)


### 使用XMLHttpRequest
XMLHttpRequest相当于原生开发者的第三方封装的网络请求框架，不过XMLHttpRequest是内建在react-native中得，我们可以直接使用而不需要添加其他的依赖。
一个简单的XMLHttpRequest的使用范例如下：  

```
var request = new XMLHttpRequest();
request.onreadystatechange = (e) => {
  if (request.readyState !== 4) {
    return;
  }

  if (request.status === 200) {
    console.log('success', request.responseText);
  } else {
    console.warn('error');
  }
};

request.open('GET', 'https://mywebsite.com/endpoint/');
request.send();
```
收钱我们创建一个XMLHttpRequest的实例，然后设置相应状态的监听，根据不同的响应代码而做不同的操作，接下来调用open()设置请求方法和请求URL，最后调用send()方法发送请求。

### 总结
上面讲到的都是些比较简单和常用的方法，还有许多情况(比如加载网络图片、上传和下载文件等)在之后用到的时候进一步深入学习理解。  

















