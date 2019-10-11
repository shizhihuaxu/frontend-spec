[TOC]



## 1  工作流规范

### 1.1  版本规范

#### 1.1.1  开源项目

​		版本号的迭代应该遵循某一规则，使用户可以了解版本变更的影响范围。github 的[语义化版本表示](https://semver.org/lang/zh-CN/)就是关于版本定义的一个方法原则，例如 x.y.z：

​				主版本号：x 为 0 表示软件还在初始开发阶段。x 的增加表示做了不兼容的 API 修改。x 的每次更新，y、z 都必须归 0。以 0.1.0 作为你的初始化开发版本。

​				次版本号：y ，做了向下兼容的功能性新增。y 的每次更新，z 必须归 0。

​				修订号：z，做了向下兼容的 发布版 bug 修正。

​		版本号的修饰词：

​				alpha:  内部版本

​				beta:  测试版

​				rc:  即将作为正式版发布

​				lts:  长期维护

#### 1.1.2  商业软件

​		商业软件的版本号，商业软件主要以功能迭代和修复 bug 为主，需要结合产品本身的特点做版本的迭代规范。

​				主版本号：UI风格大改动和业务逻辑有大的改动或者整体架构有大改动甚至推翻重做时候更改。

​				次版本号：需求变动时更改。

​				修订号：修复产品中的 bug、优化时的更改，非开发时的 bug 。

​		可以一个次版本号对应多个需求，但是尽量不要把需求定义在修订号中，否则会对开发流程产生很大的干扰。

​		界定哪些是优化，哪些是需求变动，应该站在开发的角度上而不是用户的角度上。

### 1.2  版本控制系统规范

#### 1.2.1  分支管理规范

​		项目中应避免随意创建分支，合并代码应保持提交线的脉络清晰，可以使用[Github Flow](http://scottchacon.com/2011/08/31/github-flow.html)、 [Git Flow](https://nvie.com/posts/a-successful-git-branching-model/) 、[GitLabFlow](https://docs.gitlab.com/ee/workflow/gitlab_flow.html)或自定义分支管理模型。

​		如果使用 Git Flow 可以安装 [gitflow](https://github.com/nvie/gitflow/wiki/Windows) 工具，这样使用响应命令会自动从相应分支切出分支，避免出错。

|   分支名   |         作用         | 分支来源 |   分支合并目标   |    生命周期    |
| :--------: | :------------------: | :------: | :--------------: | :------------: |
|   master   |  主分支，打最终 tag  |          |                  |    一直存在    |
|  develop   |      开发稳定版      |  master  |      master      |    一直存在    |
| feature-*  |      新需求研发      | develop  |     develop      | 新功能开发结束 |
| realease-* | 标记产品中的版本信息 | develop  | develop & master |    版本发布    |
|  hotfix-*  | 线上发布版 bug 修复  |  master  | develop & master |  bug 修复结束  |

​		master 分支：主分支、受保护分支，不可直接提交代码，发布稳定版，在此分支上打 tag。

​		develop 分支：开发稳定版，此分支所有新功能研发基础。当系统功能测试通过后，向 master 分支提交 pr 。

​		feature 分支：从 develop 分支切出来进行新功能的开发。单元测试通过后合并到 develop 分支。结束了一次完整的发布流程后，才可创建新的特性分支。

​		release 分支：当 develop 分支的新状态稳定后，从 develop 分支切出来，用于发布分支，适用于产品发布、产品迭代，修改产品版本信息后合并到 develop 分支 ，并且将此分支合并到 master 分支上，然后在 master 分支上打一个 tag。

​		hotfix 分支：

​				当生产环境中的软件遇到了异常情况或者发现了严重到必须立即修复的软件缺陷的时候，就需要从master分支上指定的 tag 版本派生 hotfix 分支来组织代码的紧急修复工作。修复后同时合并到 master 和 develop 分支，然后再创建 release 分支，更改版本号中的修订号，重新走发布流程。

​				当 release 分支还存在，应该直接合并到 release 分支上，而不是 develop 分支上，然后走 release 分支流程。

​		辅助分支 feature、bugfix、hotfix 、release 分支分别在本次需求研发结束、本次 bug 修复结束、线上 bug 修复结束、版本发布结束后删除。

#### 1.2.2  commit message 规范

​		我们使用 [Angular 团队的规范](https://link.zhihu.com/?target=https%3A//github.com/angular/angular.js/blob/master/DEVELOPERS.md%23-git-commit-guidelines)，借助 commit message 格式化工具 [commitizen](https://github.com/commitizen/cz-cli)，并使用 commit lint 做提交信息的格式校验。生成 changelog 使用 conventional-changelog-cli 。

- 安装依赖

  ```bash
  npm install -g commitizen conventional-changelog-cli
  npm install -D @commitlint/cli @commitlint/config-conventional 
  ```

- 然后选择一个 Adapter 作为提交规范，这里选择 cz-conventional-changelog，一套适用于commitizen的Angular团队规范初始化项目，需要node >= 10, npm >= 6。

  ```javascript
  commitizen init cz-conventional-changelog --save --save-exact
  
  // 生成 change-log
  conventional-changelog -p angular -i CHANGELOG.md -s -r 0
  
  // 或者在 package.json 中加入
  "scripts": {
  	 "version": "conventional-changelog -p angular -i CHANGELOG.md -s -r 0 
  }
  ```

- 在提交 commit 前使用 husky 添加钩子，做格式校验。

  ```javascript
  npm install -D husky
  
  // .commitlintrc.js
  module.exports = { extends: ['@commitlint/config-conventional'] }
  
  // package.json
  
  "husky": {
      "hooks": {
          "pre-commit": "npm run test",  // 在提交代码前跑一下单元测试
          "commit-msg": "commitlint -E HUSKY_GIT_PARAMS"
      }
  }
  ```

- 提交时使用 git cz 代替 git commit。

### 1.3  项目构建规范

- 优先使用框架的脚手架构建项目。vue 使用 vue-cli 3，react 使用 create-react-app，或者使用配置比较成熟的项目模板。

### 1.4  开发流程规范

​		一个项目的整个生命周期中开发流程大概是这样的：

- 初始化 git 仓库，从 master 切出一个 develop 分支。

  ```bash
  git init
  git checkout -b develop master
  ```

- 从 develop 分支切出 feature-* 分支开始搭建项目结构集成一些工具。（feature 应该只存在于开发人员的代码库中，不应该在远程服务器上）

  ```bash
  git checkout -b feature-* develop
  ```

- 当每天完成一些代码，跑单元测试

  

git flow 服务器只存在 master 和 develop 分支，那么新功能协同开发和日常代码提交怎么办，每次可以提交就向 develop 合并一次吗？使用 git flow 要 fork 到自己的

如果使用 git folw，在哪个阶段跑单元测试，哪个阶段跑集成测试，哪个阶段 code review ？

单元测试是 develop 向master 提交时跑还是每日feature 向 develop 提交时跑，这样每次开发代码就要同时写单元测试的代码，

release 分支可以限定权限，比如只能由负责人或组长来

### 1.5  持续集成

- 

  

- 保证本地构建环境与线上构建环境的一致，如 Node 版本、npm 版本

- 保证构建失败后有原来可使用的版本的缓存，回滚，一旦当前版本发生问题，就要回滚到上一个版本的构建结果。最简单的做法就是修改一下符号链接，指向上一个版本的目录。

- 触发的条件。什么时候自动构建。

- 确定持续集成中要做哪些事情。

### 1.6  持续部署

部署到类生产环境进行更多的测试

### 1.7  发布流程

​		软件产品对外发布的一整套流程，将流程规范后，可实现自动化。一个典型的发布工作流程如下：

- 从 develop 分支切出 release 分支
- 更改 package.json 中的 version
- 生成 changelog
- 提交 package.json 和 CHANGELOG.md 文件
- 将release 分支合并到 master 分支，在 master 分支打 tag ，同时将 release 分支合并到 develop 分支

### 1.7  任务管理

​		任务管理工具可以帮助 leader 了解任务进度，清晰的看到哪些需要做，哪些正在做，完成之后及时更改任务状态。

​		任务管理工具中的开发、bug 修复条目应该与分支存在联系。

## 2  技术选型

​		在团队中应该选择一个比较稳定且成员比较熟练的框架，熟悉它相关的生态系统、配套工具、最佳实战、性能调优的方式。

### 2.1  框架及生态系统

|     框架      |         Vue         |      React       |
| :-----------: | :-----------------: | :--------------: |
| node 版本管理 |         nvm         |       nvm        |
|    包管理     |         npm         |       npm        |
|    脚手架     |      vue-cli 3      | create-react-app |
|   状态管理    |        vuex         |   mobox/redux    |
|     路由      |     vue-router      |   react-router   |
|    UI 框架    |  iview /elementUI   |    Ant Design    |
| CSS 预处理器  |      scss/less      |    scss/less     |
|  TypeScript   |     vue 3 /待定     |       成熟       |
| 服务器端渲染  |       Nuxt.js       |     Next.js      |
|    移动端     |                     |   React Native   |
|   桌面应用    |      Electron       |                  |
|   单元测试    | karma 、mocha、chai |  jest 、enzyme   |
| 代码格式校验  |       eslint        |      eslint      |

### 2.2  其它

- js 的常用工具函数库
- 迎接新技术，考虑学习成本、收益和风险

## 3  浏览器兼容性

​		应该根据公司的项目类型或者目标用户确定软件要兼容的浏览器版本。

### 3.1  确定兼容策略

​		**渐进增强**还是**优雅降级**？

​		渐进增强一开始针对低版本的浏览器构建页面，满足最基本的功能，再针对高级浏览器进行效果，交互，追加各种功能以达到更好用户体验，以实现最基础功能为基本，向上兼容。

​		优雅降级一开始针对一个高版本的浏览器构建页面，先完善所有的功能，然后针对各个不同的浏览器进行测试，修复，保证低级浏览器也有基本功能可以使用。

### 3.2  浏览器分级

​		[YUI 的浏览器分级策略](https://github.com/cssmagic/blog/issues/47)，将浏览器划分为多个等级，不同等级代表了不同的支持程度。

## 4  项目组织

- 项目命名：小写字母 + 连字符，字母开头。
- 项目目录组织风格。
- 常用的 js ，css 文件的名称。公共代码定义哪些内容。
- 项目中必须包含的文件：项目说明（READEME.md）、版本变更记录（CHANGELOG.md）、项目许可（LICENSE）
- 文件命名：小写字母 + 连字符，字母开头。

## 5  编码规范

### 5.1  JavaScript

​		Lint 工具：ESLint

​		规范：[Airbnb 代码风格](https://github.com/airbnb/javascript)，[JavaScript Standard Style](https://standardjs.com/) 

​		类型检查：TypeScript

### 5.2 CSS 

​		Lint 工具：

​		规范：Airbnb CSS / Sass Style Guide

​		方法论：BEM，CSS Modules、Styled Component、OOCSS

### 5.3  Vue / React

​		[vue 风格指南](https://cn.vuejs.org/v2/style-guide/)

​		[React 风格指南](https://github.com/airbnb/javascript/tree/master/react)

### 5.4  编辑器与代码格式化工具

​		推荐使用 vscode 作为编辑器；在 vscode 中使用 Prettier。

​		Vue ：Vetur

​		React：

### 5.5  Code Review

- 模块耦合度
- 代码重复
- 代码健壮性：是否存在安全性问题、错误是否被处理、是否存在潜在的性能问题和异常
- 代码的性能
- 一些 code review 的工具

## 6  文档规范

哪些内容需要被记录，应该以何种形式（注释、图表、md 文件）、文档应该包含哪些内容（文档模板）

jsdoc

tsdoc

接口文档

测试用例文档

~~测试结果报告~~

## 7  测试规范

单元测试

e2e 测试

浏览器兼容性测试

性能测试

安全性测试

测哪些模块

测试结果的指标，应该符合什么条件

相关工具



## 8  前后端协作规范

### 8.1  协作流程

需求分析

前后端开发讨论

设计接口

并行开发

联调，接口测试

真实环境联调

### 8.2  接口规范

风格：RESTful 、JSONRPC、GrapgQL

接口设计应该考虑的内容：

​	明确数据类型

​	异常情况怎么返回信息

​	空值怎么返回，全部为 nul 还是空的相应类型，例如空数组、空对象、空字符串等

### 8.3  接口文档规范

接口的文档应该包含：

​	版本号

​	服务的入口，例如基本路径 https: //a.b.c

​	简单的使用示例

​	安全和认证：哪些接口需要认证，怎么认证

​	具体的接口定义：

​			接口功能描述

​			请求方法和url

​			请求参数描述（数据类型，是否可选等）

​			响应参数描述（数据类型，空值以何种形式返回）

​			可能的情况：异常、错误处理、错误代码、错误描述

​			请求示例



​    