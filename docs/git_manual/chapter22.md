---
title: 搭建 Git 服务器
nav:
  title: Git 手册
  path: /git_manual
order: 23
---

对于某些视源代码如生命的商业公司来说，既不想公开源代码，又舍不得给 GitHub 交保护费，就只能自己**搭建一台 Git 服务器作为私有仓库**使用

## 准备

- 一台运行 Linux 的机器，强烈推荐用 `Ubuntu` 或 `Debian`（通过几条简单的 `apt` 命令就可以完成安装）
- 假设你已经有 `sudo` 权限的用户账号

## 安装 git

```bash
sudo apt-get install git
```

## 创建一个 git 用户，用来运行 git 服务

```bash
sudo adduser git
```

## 创建证书登录

收集所有需要登录的用户的公钥，就是他们自己的 `id_rsa.pub` 文件，把所有公钥导入到 `/home/.ssh/authorized_keys` 文件里，一行一个。

## 初始化 Git 仓库

先**选定一个目录作为 Git 仓库**，假定是 **`/srv/sample.git`** ，在 `/srv` 目录下输入命令：

```bash
sudo git init --bare sample.git
```

Git 就会创建一个裸仓库，而裸仓库没有工作区，因为服务器上的 Git 仓库纯粹是为了共享，所以不让用户直接登录到服务器上去改工作区，并且服务器上的 Git 仓库通常都以 `.git` 结尾。然后，把 `owner` 改为 `git`：

```bash
sudo chown -R git:git sample.git
```

## 禁用 shell 登录

出于安全考虑，第二步创建的 `git` 用户不允许登录 `shell`，这可以通过编辑 `/etc/passwd` 文件完成。找到类似下面的一行：

```bash
git:x:1001:1001:,,,:/home/git:/bin/bash
```

改为：

```bash
git:x:1001:1001:,,,:/home/git:/usr/bin/git-shell
```

这样，git 用户可以正常通过 `ssh` 使用 git，但无法登录 `shell` ，因为我们为 git 用户指定的 `git-shell` **每次一登录就自动退出**。

## 克隆远程仓库

现在，可以通过 `git clone` 命令克隆远程仓库了，在各自的电脑上运行：

```bash
git clone git@server:/srv/sample.git
Cloning into 'sample'...
warning: You appear to have cloned an empty repository.
```

## 管理公钥

如果团队很小，把每个人的公钥收集起来放到服务器的 `/home/.ssh/authorized_keys` 文件里就是可行的。
如果团队有几百号人，可以用 `Gitosis` 来管理公钥

## 管理权限

Git 支持钩子（hook），可以在服务器端编写一系列脚本来控制提交等操作，达到权限控制的目的。

> **`Gitolite`** 就是这个工具

## 小结

- 搭建 Git 服务器非常简单，通常 10 分钟即可完成
- 要方便管理公钥，用 `Gitosis`；
- 要像 `SVN` 那样变态地控制权限，用 `Gitolite`
