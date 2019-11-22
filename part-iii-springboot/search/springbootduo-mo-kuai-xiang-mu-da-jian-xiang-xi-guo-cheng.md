1. 项目介绍:

本项目包含一个父工程 demo  和 四 个子模块（demo-base, demo-dao, demo-service, demo-web\)， demo-base 为其他三个模块的公共内容， 四个模块都依赖父模块， demo-dao 依赖 demo-base；   demo-service 依赖 demo-dao, 间接依赖 demo-base;   demo-web 依赖 demo-service, 间接依赖demo-base和demo-dao



