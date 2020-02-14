# ICE环境搭建

官方文档：https://ice.alibaba-inc.com/docs/guide/about

 

1、**主要是需要安装：node js和def，外加一个tnpm（就是npm的淘宝镜像）** 

2、**语雀教程：**

https://yuque.antfin-inc.com/paimaitest/zzkhgg/yhoww2

**我遇到的问题**（已经提单到语雀，已解决）：

https://yuque.antfin-inc.com/def-feedback/topics/332#reply-478536

3、**mac怎么安装node.js？**

jian'shu简书教程：https://www.jianshu.com/p/3b30c4c846d1

### Ice使用

ICE：

```
$ def init ice 初始化工程
$ def add page 添加页面
$ def add module 添加项目内的模块
$ def add @ali/ice-add-item 添加ice业务组件 这样即可在项目中通过import IceAddItem from '@ali/ice-add-item'; 引用

def dev 启动项目预览 访问：[http://127.0.0.1:3333](http://127.0.0.1:3333/)即可
def build 当开发工作完成后，需要将源代码编译。执行build 后，将在src 所在目录下生成build 目录，用于存放编译后的代码。
```



新建分支

```
$ git checkout -b daily/0.1.0 # 创建分支 
$ git push origin daily/0.1.0 # 提交分支

def publish 将文件发布到CDN
```

发布成功后资源路径的格式为{域名}+{gitlab 组名}+{仓库名}+{分支号x.y.x}+{资源文件路径}
• 日常环境发布后得到的cdn 资源路径是： 
//g-assets.daily.taobao.net/ice-assets/ice-demo/0.1.0/pages/home/index.js
• 线上环境发布后得到的cdn 资源路径是： 
//g.alicdn.com/ice-assets/ice-demo/0.1.0/pages/home/index.js
 
生命周期 http://www.jianshu.com/p/4784216b8194
es6 http://es6.ruanyifeng.com/
props与state的区别https://www.cnblogs.com/ysbpysbp/p/6115900.html
api查询 https://developer.mozilla.org/zh-CN/
router http://www.ruanyifeng.com/blog/2016/05/react_router.html?utm_source=tool.lu