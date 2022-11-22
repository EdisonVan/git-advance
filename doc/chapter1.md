- [Git 训练网址](https://learngitbranching.js.org/)
- [Git 的官方网站](http://git-scm.com)

# 特殊命令

- 按 ESC 按键后，键入`:e！`恢复上次编辑保存的文件状态
- e.g.上次输入的是 print 并且按：w 保存了。这时再次编辑这个文件，即使你把原来的保存的 print 字符串删除了，那么只需要按这个：`e！`就会恢复这个 print 字符串。(一旦你按了这个命令，当前输入的文本将会消失不见)

## 重命名文件

```
git mv  旧文件名 新文件名
-ren/mv 旧文件名 新文件名
-git rm 旧文件名
-git add 新文件名
```

```bash
git mv home.j this.js
```

# 安装 Git

- Git 官网下载最新版本 git
- Git 管理的文件有三种状态：
  - 已修改（modified）
  - 已缓存（staged）
  - 已提交（committed）
