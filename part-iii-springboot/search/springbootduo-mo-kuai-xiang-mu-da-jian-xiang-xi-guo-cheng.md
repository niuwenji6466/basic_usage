1. 项目介绍:

本项目包含一个父工程 demo  和 四 个子模块（demo-base, demo-dao, demo-service, demo-web\)， demo-base 为其他三个模块的公共内容， 四个模块都依赖父模块， demo-dao 依赖 demo-base；   demo-service 依赖 demo-dao, 间接依赖 demo-base;   demo-web 依赖 demo-service, 间接依赖demo-base和demo-dao

![](/assets/20181013204132335.png)

-

2.搭建思路：先创建一个 Spring Initializr工程 demo 作为 父工程， 然后在父工程再建四个子 Module \(demo-base, demo-demo-dao, demo-service\), 其实他们就是四个普通的Spring Initializr工程。

3.开始搭建：

首先，创建一个Spring Initializr项目 和 子Module



