---
title: 前言
nav:
  title: Git 手册
  path: /git_manual
order: 2
---

## 特殊命令

- 按 ESC 按键后，键入`:e！`恢复上次编辑保存的文件状态
- e.g.上次输入的是 print 并且按：w 保存了。这时再次编辑这个文件，即使你把原来的保存的 print 字符串删除了，那么只需要按这个：`e！`就会恢复这个 print 字符串。(一旦你按了这个命令，当前输入的文本将会消失不见)

### 重命名文件

```
git mv  旧文件名 新文件名
-ren/mv 旧文件名 新文件名
-git rm 旧文件名
-git add 新文件名
```

```bash
git mv home.j this.js
```

## 安装 Git

- Git 官网下载最新版本 git
- Git 管理的文件有三种状态：
  - 已修改（modified）
  - 已缓存（staged）
  - 已提交（committed）

## Git 学习资料

**Git**

- [Git 训练网址](https://learngitbranching.js.org/)
- [Git 的官方网站](http://git-scm.com)
- [Git-Book 官方文档](https://git-scm.com/book/en/v2)
- [git-简明指南 罗杰·杜德勒](http://rogerdudler.github.io/git-guide/index.zh.html)
- [git 详细学习](https://git-scm.com/book/zh/v2)
- [Learn Git Branching 通过一系列刺激的关卡挑战，逐步深入的学习 Git 的强大功能](https://learngitbranching.js.org/)
- [猴子都能懂的 GIT 入门](https://backlogtool.com/git-tutorial/cn/)
- [代码提交规范](doc/SubmitSpecification.md)
- [GitLab 搭建与总结](doc/GitLab.md)
- [Learn Git Branching 通过一系列刺激的关卡挑战，逐步深入的学习 Git 的强大功能](https://learngitbranching.js.org/)