# VUE安装

* 安装vue-cli

```
npm install -g @vue/cli
# OR
yarn global add @vue/cli
```

* 查看是否安装成功

```
vue --version
```

* 查看vue.js   [https://cn.vuejs.org/](https://cn.vuejs.org/)

* Vue.js 开发标准工具 [https://cli.vuejs.org/zh/](https://cli.vuejs.org/zh/ "开发标准工具")

* View UI 组件库 [https://www.iviewui.com/](https://www.iviewui.com/ )

* 有的时候由于npm不是最新版本，老版本就会出现错误，解决办法

```
     npm update -g
```

* 老版本的vue-cli 还有可能报其他错误，需要更新

```
npm update vue-cli
```

* vue -V 查看vue版本,查看vue-cli 版本 vue -v

  ```
    vue -V , vue -h \[查看帮助\]
  ```

* 当安装 vue-cli 失败的时候-4080，清除npm缓存

* 备注：vsCode 有两个控制台cmd\powerShell

# vue创建新项目\(npm安装-&gt;初始化项目\)

## 第一步：npm安装

1. 首先：先从nodejs.org中下载nodejs

2. 打开控制命令行程序（管理员权限CMD）,检查是否正常 node -v ,npm -v（如果npm不是最新版本执行npm install -g npm ）

3. 使用淘宝npm镜像, 道国内直接使用npm 的官方镜像是非常慢的，这里推荐使用淘宝 NPM 镜像 .

   $ npm install -g cnpm --registry=[https://registry.npm.taobao.org](https://registry.npm.taobao.org)

   这样就可以使用cnpm命令安装模块了

## 第二步：项目初始化

1. 第一步：安装vue-cli ， cnpm install vue-cli -g //全局安装 vue-cli ， 查看vue-cli是否成功，不能检查vue-cli,需要检查vue （vue list ）

2. 第二步： 选定路径，新建vue项目 ， 在桌面上新建了sun文件夹 ， cd目录路径 ， vue init webpack ”项目名称“ 。

   控制台（cmd）输入如下：

* ?project name \[键盘输入项目名\]

* ?project description \[键盘输入项目描述\]

* ?Author \[键盘输入作者\]

* ?Vue build \[\]

* ?Install vue-router? Yes

* ?Use ESLint to lint your code ? No

* ?Set up unit tests ? No

* ?Setup e2e tests with Nightwatch? No

* ?Should we run npm 'npm install ' for you after the project has been create ? npm

   3.现在已经创建好了，那就让项目先安装下依赖再运行一下，会出现下面的页面，操作指令是：

cnpm install , cnpm run dev 注意 这里要在sell下进行安装和运行

  4.现在已经创建好了，那就让项目先安装下依赖再运行一下，会出现下面的页面，操作指令是：

cnpm install , cnpm run dev 注意 这里要在sell下进行安装和运行







