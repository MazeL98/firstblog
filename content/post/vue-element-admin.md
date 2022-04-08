---
title: "第一次本地运行vue-element-admin解决npm install报错git dep preparation failed"
date: 2022-03-16T14:24:22+08:00
draft: false
---

项目地址：https://github.com/PanJiaChen/vue-element-admin

解决方案参考：https://ask.csdn.net/questions/7624615?spm=1100839930&username=weixin_52394634

执行 npm install ，在安装 tui-eiditor 编辑器时报错：git dep preparation failed

更换 cnpm / yarn 命令重新安装，仍然报错。

单独安装 tui-editor 最新版本仍然是相同的报错。

参考上面这位博主的解决方案我进行了如下操作：

1、重新用淘宝镜像的连接安装cnpm

```powershell
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

2、重新执行 

```
npm install
```

3、在2的过程中遇到卡顿，推测和git.exe 有关，下载安装 dev-sidecar 加速器，打开 npm 加速和 git.exe 代理后成功安装

4、执行 

```
npm install core-js@2
```

5、 尝试运行 npm run dev ，但遇到报错: "Cannot find modules xxxx", 需要单独安装这个模块 

```
npm install xxxx(报错的模块名) --save
```

这里差不多遇到了十几次，而且每次都只提示一个模块（晕，也没有别的方法，只能一个个安装。一般都能安装成功，如果还是不成功，试试以下办法：

（1）查看项目根文件夹中的package.json中显示的该模块需要的版本，精准安装该版本

（2）报错，大致意思是在该依赖文件夹下找不到package.json。例如我的一直提示 entities/maps/entities.json 下找不到package.json，无论怎么重新安装都没用，后来发现真正报错的是另一个模块markdown-to 下的entities.js文件，这个文件中引入entities.js的路径出现错误，少了一层lib文件夹。改正之后项目终于成功运行。

