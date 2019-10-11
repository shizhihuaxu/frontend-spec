[TOC]



## 1.  windows 安装 gitflow 教程

​		这里介绍的是在 windows 上与 Git for Windows 集成的 gitflow 安装方法，你可以查看其它方式或操作系统的 [安装教程](https://github.com/nvie/gitflow/wiki/Installation)。

- step 1：在 [util-linux package](http://gnuwin32.sourceforge.net/packages/util-linux-ng.htm) 上找到 Binaries  和 Dependencies 的压缩包下载。

- step 2：在 Binaries 解压后 bin 文件夹中 getopt.exe 复制到 git 安装目录中的 Git\bin 文件夹下。

- step 3：在 Dependencies 解压后 bin 文件夹中 libiconv2.dll 、libintl3.dll 两个文件也复制到 git 安装目录中的 Git\bin 文件夹下。

- step 4：打开 git bash ，运行：

  ```bash
  git clone --recursive git://github.com/nvie/gitflow.git
  ```

- step 5：以管理员权限打开 cmd，切到 clone下来的 gitflow 目录，运行：

  ```
  cd contrib 
  msysgit-install.cmd "D:\git\Git"  // git 的安装地址
  ```

- step 6：检验是否安装成功。打开 git bash ，执行 git flow 如果出现 usage 提示，表示安装成功。

## 2.  gitflow 使用方法

[相关文章](https://www.git-tower.com/learn/git/ebook/cn/command-line/advanced-topics/git-flow)

- 初始化

  ```bash
  cd your-project
  git flow init
  ```

接下来会允许配置一些分支名称，可以直接使用默认值

- 创建一个新的特性分支

  ```
  git flow feature start aaa  //  分支名最终为 feature/aaa
  ```

  

- 新功能开发完以后

  ```
  git flow feature finish 'desc'
  ```

  


- 新的发布

  ```
  git flow release start bbb  // 分支名最终为 release/bbb
  git flow release finish 'bbb' -m 'my tag msg'
  ```

  



finish release 和 finish hotfix 都需要写 tag 信息