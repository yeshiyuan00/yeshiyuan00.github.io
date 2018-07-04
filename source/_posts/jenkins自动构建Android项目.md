---
title: jenkins自动构建Android项目
date: 2016-08-04 10:33:37
tags: jenkins
---

  最近在学习如何使用jenkins来自动构建Android项目，为了防止之后忘记，特地将整个过程记录下来，方便以后快速回忆。

<!-- more -->

## 安装jenkins
  我的系统是MAC OS，在这里使用brew来安装。
  
  ```  bash
  $ brew install jenkins  
  ```
  
  linux系统可使用yum安装，或者安装brew工具.
  
  安装完成后，终端输入jenkins即可开启jenkins：
  
  ``` bash
  $ jenkins
  ```
    
  成功启动jenkins后在浏览器输入localhost:8080 即可访问jenkins(jenkins的默认端口时8080)。第一次打开如下图所示：  
  
  ![](/img/jenkins-getting-start.png)
  
  将/Users/yeshiyuan/.jenkins/secrets/initialAdminPassword里面的密码填入输入框即可继续。然后根据提示进行后面的操作。主要是安装一些插件和创建一个管理员账户。完成后，界面如下：
  
  ![](/img/jenkins-started.png)
  
