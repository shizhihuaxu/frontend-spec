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

```bash
commitizen init cz-conventional-changelog --save --save-exact

# 生成 change-log
conventional-changelog -p angular -i CHANGELOG.md -s -r 0

#或者在 package.json 中加入
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

### 1.7 任务管理

​		任务管理工具可以帮助 leader 了解任务进度，清晰的看到哪些需要做，哪些正在做，完成之后及时更改任务状态。

## 2  技术栈



## 3  浏览器兼容性

## 4  项目组织

## 5  编码规范

## 6  文档规范

## 7  测试规范

## 8  异常处理、监控和调试规范

## 9  前后端协作规范