---
title: react-native学习系列--在已存在的项目中集成
date: 2016-08-19 17:38:50
tags: react-native
---
上一篇文章中我们直接使用react-native命令行工具创建一个新的react-native项目，接下来我们要在现有的Android项目中集成react-native。

<!-- more -->  

### app项目中加入JS
在Android项目的根目录下，执行下面命令：  

```
npm init
npm install --save react-native
curl -o .flowconfig https://raw.githubusercontent.com/facebook/react-native/master/.flowconfig
```
这时react-native的npm依赖就添加进来了，然后在自动给我们创建的package.json文件中，在scripts里面，加入一句：  

```
"start": "node node_modules/react-native/local-cli/cli.js start"
```
接下来将下面的代码复制到根目录下的index.android.js文件中。  

```
use strict';

import React from 'react';
import {
  AppRegistry,
  StyleSheet,
  Text,
  View
} from 'react-native';

class HelloWorld extends React.Component {
  render() {
    return (
      <View style={styles.container}>
        <Text style={styles.hello}>Hello, World</Text>
      </View>
    )
  }
}
var styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
  },
  hello: {
    fontSize: 20,
    textAlign: 'center',
    margin: 10,
  },
});

AppRegistry.registerComponent('HelloWorld', () => HelloWorld);

```

### Android 项目中的改动
在app模块中的build.gradle中增加react-native依赖：  

```
compile "com.facebook.react:react-native:+"  // From node_modules
```
在根项目中的build.gradle中加入maven仓库地址：  

```
allprojects {
    repositories {
        ...
        maven {
            // All of React Native (JS, Android binaries) is installed from npm
            url "$rootDir/node_modules/react-native/android"
        }
    }
    ...
}
```
最后不要忘了给应用添加网络访问权限。

### Activity怎么写？
在onCreate()中，创建一个ReactRootView，然后使用我们创建的ReactRootView作为setContentView（）的参数。  

```
public class MyReactActivity extends Activity implements DefaultHardwareBackBtnHandler {
    private ReactRootView mReactRootView;
    private ReactInstanceManager mReactInstanceManager;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        mReactRootView = new ReactRootView(this);
        mReactInstanceManager = ReactInstanceManager.builder()
                .setApplication(getApplication())
                .setBundleAssetName("index.android.bundle")
                .setJSMainModuleName("index.android")
                .addPackage(new MainReactPackage())
                .setUseDeveloperSupport(BuildConfig.DEBUG)
                .setInitialLifecycleState(LifecycleState.RESUMED)
                .build();
        mReactRootView.startReactApplication(mReactInstanceManager, "HelloWorld", null);

        setContentView(mReactRootView);
    }

    @Override
    public void invokeDefaultOnBackPressed() {
        super.onBackPressed();
    }
}
```
其中mReactRootView.startReactApplication(mReactInstanceManager, "HelloWorld", null)中得"HelloWorld"必须对应上面index.android.js中的"HelloWorld"。

接下来，我们需要在Activity的生命周期回调函数中控制ReactInstanceManager。  

```
@Override
protected void onPause() {
    super.onPause();

    if (mReactInstanceManager != null) {
        mReactInstanceManager.onHostPause();
    }
}

@Override
protected void onResume() {
    super.onResume();

    if (mReactInstanceManager != null) {
        mReactInstanceManager.onHostResume(this, this);
    }
}

@Override
protected void onDestroy() {
    super.onDestroy();

    if (mReactInstanceManager != null) {
        mReactInstanceManager.onHostDestroy();
    }
}
```

如多需要在react-native中监听手机的返回键，则需要重写Activity的onBackPressed()函数：  

```
@Override
 public void onBackPressed() {
    if (mReactInstanceManager != null) {
        mReactInstanceManager.onBackPressed();
    } else {
        super.onBackPressed();
    }
}
```

同时，我们需要通过菜单键调出react-native的调试带单，所以我们需呀监听手机的物理菜单键：  

```
@Override
public boolean onKeyUp(int keyCode, KeyEvent event) {
    if (keyCode == KeyEvent.KEYCODE_MENU && mReactInstanceManager != null) {
        mReactInstanceManager.showDevOptionsDialog();
        return true;
    }
    return super.onKeyUp(keyCode, event);
}
```
至此，代码部分已经完成，接下来在项目根目录执行下面命令启动服务：  

```
npm start
```

然后运行Android项目，此时，react-native已成功集成到我们的Android项目中了。
