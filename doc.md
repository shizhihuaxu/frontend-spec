[TOC]

## 1  工作流规范

### 1.1  版本规范

#### 1.1.1  开源项目

​		版本号的迭代应该遵循某一规则，使用户可以了解版本变更的影响范围。github 的[语义化版本表示](https://semver.org/lang/zh-CN/)就是关于版本定义的一个方法原则，例如 x.y.z：

​				主版本号：x 为 0 表示软件还在初始开发阶段。x 的增加表示做了不兼容的 API 修改。x 的每次更新，y、z 都必须归 0。

​				次版本号：y ，做了向下兼容的功能性新增。

​				修订号：z，做了向下兼容的 bug 修正。y 的每次更新，z 必须归 0。

​		版本号的修饰词：

​				alpha:  内部版本

​				beta:  测试版

​				rc:  即将作为正式版发布

​				lts:  长期维护

#### 1.1.2  商业软件

​		商业软件的版本号，商业软件主要以功能迭代和修复 bug 为主，需要结合产品本身的特点做版本的迭代规范。

​				主版本号：UI风格大改动和业务逻辑有大的改动或者整体架构有大改动甚至推翻重做时候更改。

​				次版本号：需求变动时更改。

​				修订号：修复产品中的 bug、优化时的更改。

​		可以一个次版本号对应多个需求，但是尽量不要把需求定义在修订号中，否则会对开发流程产生很大的干扰。

​		界定哪些是优化，哪些是需求变动，应该站在开发的角度上而不是用户的角度上。

### 1.2  版本控制系统规范

#### 1.2.1  分支管理规范

​		master 分支：发布稳定版。受保护分支，不可直接提交代码。

​		develop 分支：开发稳定版，此分支所有新功能研发和 bug 修复的基础。当系统功能测试通过后，向 master 分支提交 pr 

​		feature 分支：从 develop 分支切出来进行新功能的开发。单元测试通过后向 develop 分支提交 pr。

​		bugfix 分支：用于从 develop 分支切出来修复 bug 。单元测试通过后向 develop 分支提交 pr。

#### 1.2.2  commit message 规范

​		在项目中使用 commitizen，并使用 commit lint 做提交信息的格式校验。生成 changelog 使用 conventional-changelog-cli 。

- 安装依赖

  ```bash
  npm install -g commitizen conventional-changelog-cli
  npm install -D @commitlint/cli @commitlint/config-conventional 
  ```

- 然后使用 cz-conventional-changelog 初始化项目，需要node >=10, npm >=6。

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

### 1.3  开发规范

- JavaScript 代码风格：[airbnb](https://github.com/airbnb/javascript) 
- CSS 代码风格，命名规范

### 1.4  构建规范

​		vue 使用 vue-cli 3，react 使用 create-react-app，或者有一份配置好的可用的项目模板。

### 1.5  发布工作流规范

​		软件产品对外发布的一整套流程，将流程规范后，可实现自动化。一个典型的发布工作流程如下：

- 代码变更
- 单元测试
- 将代码提交到远程版本库
- 程序通过持续集成测试
- 更改 package.json 中的 version
- 生成 changelog
- 提交 package.json 和 CHANGELOG.md 文件
- 打 tag ，推送 tag 到远程版本库

### 1.6  持续集成规范  - 待完善

- 保证本地构建环境与线上构建环境的一致，如 Node 版本、npm 版本
- 保证构建失败后有原来可使用的版本的缓存
- 触发的条件。什么时候自动构建。
- 确定持续集成中要做哪些事情。

### 1.7  任务管理

​		任务管理工具可以帮助 leader 了解任务进度，清晰的看到哪些需要做，哪些正在做，完成之后及时更改任务状态。

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

- 项目的目录组织形式
- css 命名方式，公共代码定义哪些内容
- js 的常用工具函数库
- 迎接新技术，考虑学习成本、收益和风险



## 3  浏览器兼容性

​		应该根据公司的项目类型或者目标用户确定软件要兼容的浏览器版本.

### 3.1  确定兼容策略

​		**渐进增强**还是**优雅降级**？

​		渐进增强一开始针对低版本的浏览器构建页面，满足最基本的功能，再针对高级浏览器进行效果，交互，追加各种功能以达到更好用户体验，以实现最基础功能为基本，向上兼容。

​		优雅降级一开始针对一个高版本的浏览器构建页面，先完善所有的功能，然后针对各个不同的浏览器进行测试，修复，保证低级浏览器也有基本功能可以使用。

### 3.2  浏览器分级

​		[YUI 的浏览器分级策略](https://github.com/cssmagic/blog/issues/47)，将浏览器划分为多个等级，不同等级代表了不同的支持程度。

## 4  项目组织

- 项目命名：小写字母 + 连字符，字母开头。
- 项目目录组织风格。
- 常用的 js ，css 文件的名称。
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

## 7  测试规范

## 8  异常处理、监控和调试规范

## 9  前后端协作规范