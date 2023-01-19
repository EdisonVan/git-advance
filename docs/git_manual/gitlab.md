---
title: GitLab
nav:
  title: Git 手册
  path: /git_manual
order: 26
---

## GitLab（运维来做）
- 有企业定制需求
- 迭代很快、发展飞速
- GitLab代码开源（Ruby），公司开源基于开源版本做二次开发

## 核心功能

- 集成CI功能
- CI/CD功能与Jenkins冲突，但是市场占比和Jenkins(java编写)已相差不大
  - CI/CD功能允许代码快速发布
- [提供DevOps全生命周期的解决方案⚠️](https://about.gitlab.com/devops-tools/)
  - [各阶段概述](https://about.gitlab.com/stages-devops-lifecycle/)
    - 产品管理
    - 仓库创建
    - 打包发布
      - docker容器注册
    - 线上产品配置
      - 环境、权限配置
    - 监控
      - 自动化监控
    - 安全管控
- [自动化部署](https://about.gitlab.com/stages-devops-lifecycle/auto-devops)

### 项目管理
- [issue功能](https://gitlab.com/gitlab-org/gitlab/-/issues)
  - issue看板可以拖动
  - issue可用于项目管理，提bug等

### code review
- 对分支实行保护，团队之间不能直接合并到分支上
- [Merge Requests阶段](https://gitlab.com/gitlab-org/gitlab/-/merge_requests)
    - 增加CodeReview,代码标砖限制
    - 自动化测试
- Review方式
    - 自动化机器人CodeReview
    - 人工CodeReview
- Settings/Repository中进行相应push设置、设置保护分支

### 保证集成质量
- 前提：Settings->CI/CD->Runners(跑CI/CD的代理)
- CI/CD会对每一次commit进行完备检查
  - Build->Prepare->Test->Post-test->post-cleanup
- 包含：lint检查、test（单元测试检查）
- [配置gitlab-ci.yml文件让CI/CD进行工作，参考GitLab-ee项目的CI/CD](https://gitlab.com/gitlab-org/gitlab/-/blob/master/.gitlab-ci.yml)
- [检查具体的Pipelines是否包含相应CI/CD配置](https://gitlab.com/gitlab-org/gitlab/-/pipelines/223209012)
- 部署过程利用了docker技术

### 把应用部署到AWS（公有云）
部署：根目录deploy文件夹
AWS搭建：看AWS文档
获取最新代码

## 简单实用教程

### GitLab.com

gitlab是开源项目，官网也提供了社区版安装包 
- 1.有自己服务器的话可以私有化部署一个，安装教程参考[官网](https://about.gitlab.com/installation/)
  - 个人开发者来说要求的服务器配置有点高，1核1G的服务器也能勉强跑起来
- 2.另一种选择就是使用 GitLab.com，这是官方提供的免费平台，功能和社区版一样，有人在维护、更新，新功能会比较多，缺点就是偶尔抽风。[地址](https://gitlab.com/users/sign_in )
  - 注册流程比较简单，填一下 username 和 email 就差不多了，这也将是git空间的标记，名字不要取得太随意就行了

### 创建项目

登录后点击右上角的加号(New project)
- Project path：如果你属于group的话可以选group名字，这样项目就会放在对应group下，一般团队项目比较好用
- Project name：你的项目名字
- Import project from：可以从多个github、bitbucket等主流托管平台导入项目。
- Project description：项目描述，可选
- Visibility Level：项目可见级别
- Private：私有项目，需要授权才能访问，适合个人、团队开发。
- Internal：内部项目，注意只要登录账号就能访问，适合开源贡献代码。
- Public：公开项目，不用登录就能访问，适合分享项目。

点击 create，创建项目，进入空项目，会出现初始化步骤，可以用 ssh 和 https 方式来上传代码，推荐ssh，比较安全。

### 配置ssh(可选)

如果本地没有ssh key，用 ssh-keygen 初始化一个，可以参考之前写的 github 教程 git 初始化那部分

有ssh key后添加到后台，点击右侧头像，下拉菜单里选settings，在顶部的tab里点击 SSH Keys，或者[直接访问](https://gitlab.com/profile/keys)
- Key就是 .ssh/id_rsa.pub 文件内容，title填自己知道的就行，尽量语义化点

Add key，完成

### 上传项目

回到我们创建的空项目页面，在项目名称下面选择传输协议，ssh或者https，下面教程里的url会跟着变

之后按照下面的教程来做就行了，最后 `push
git push -u origin master`

完成，这时项目页面应该就有东西了，后面就可以用正常的git命令来维护代码仓库了

### 配置CI(持续集成)

如果只是要一个git代码托管的话上面几步已经足够了，现在开始介绍gitlab提供的持续集成功能，这对于需要打包、发布的人来说非常方便。

#### Pipelines

一个 pipeline 就是一次持续集成任务，一般由一次 push 触发，在网页上对项目的修改、merge也会触发 pipline。pipline 由Runner执行，Runner有两种：
- Specific Runners：私有runner，部署和执行在自己服务器上，优点是安全、速度快，缺点是需要提供服务器，[部署教程](https://docs.gitlab.com/runner/install/linux-repository.html)
- Shared Runners：共享runner，官方提供的runner，优点是免费，缺点是会偶尔抽风、或者速度慢

#### Jobs

pipline 由多个 job 组成，一个 job 会发给一个 runner 来执行，所以各个 job 之间的数据不是共享的，除非使用 cache。所以尽量把一些有依赖的步骤放到一个 job 里，或者把一些通用步骤放到 before_script 里，这个后面会提到

#### stages

stage是对job的分组，同一个stage里的job是并行的，两个stage之间是串行的

#### gitlab-ci.yml

要想配置上面说的这些，需要在项目根目录新建 .gitlab-ci.yml 文件，文件格式为 yaml，[教程](https://docs.gitlab.com/ee/ci/yaml/README.html)
- 举个例子，这是我www工程配置文件的简化版，使用golang编译：`image: golang:latest`
```
before_script:
- ln -s /builds/wuyuans/www /go/src/www
- cd /go/src/www
- mkdir bin

stages:
- build
- deploy

build_web:
stage: build
script:
- go build -v -o bin/web www/web
except:
- release

build_service:
stage: build
script:
- go build -v -o bin/service www/service
except:
- release

deploy_web:
stage: deploy
script:
- go build -v -o bin/web www/web
- scp bin/web root@${HOST_1}:/bin/
environment:
name: www/web
url: http://$CI_ENVIRONMENT_SLUG.wuyuans.com
when: manual
only:
- release
```

**image**

编译使用的 docker 镜像，如果是 golang 的话可以用 golang:latest，使用最新版 golang，其他可以[在 docker hub查](https://hub.docker.com/_/golang/)

**before_script**

每个 job 执行前都会执行 before_script 里的步骤，主要是做一些环节初始化，比如我这里把工程目录链到了 GOPATH 下，这样方便使用 go 命令。也可以在这里做一些 go get 工作

**stages**

我分了两个stage，build和deploy。build里有build_web、build_service，deploy里的是deploy_web，名字可以随便，主要是job里的stage字段需要和stages里定义的对应上。

**build_web、build_service**

script里的是执行的命令，做go build的工作，except表示这个job不能在release分支执行。

**deploy_web**

script 和前面一样。environment 用来标记发布的名字，可以用 environment 来管理发布版本、回滚等。when 表示执行时间，默认是always 每次都会执行，manual 表示需要在后台手动执行，这样在不需要所有 deploy job 都执行的时候手动 deploy 项目。only表示只在 release 分支执行。

**Environments**

在.gitlab-ci.yml里配置了environment后，job执行完后会在项目页面里的Pipelines->Environments下看到这次job，他会按照配置里的name来合并，每次job都可以重做，也就是可以用来做项目的重发和回滚，右边有相对于的按钮，很方便。

#### 总结
- gitlab 有很多功能非常实用，比如上面讲到的Pipelines、Environments等，还有像Graph(以前叫network)可以显示所有分支的树状结构，这对于在多个分支里来回切换、分不清在哪个分支提交的人来说很直观
- 而且 gitlab 对于权限控制提供了很多的选项，很适合团队合作
- 缺点：因为是官方托管的平台，日常维护、偶尔抽风什么的，还有被墙的风险什么的
  - 如果是个人用户应该关系不大
  - 如果是团队的话还是自建 gitlab 社区版，功能应该差不多，毕竟安全和稳定对团队来说是比较重要的

### 实用技巧

vscode 拉 gitLab 项目的方法：
- 查看->命令面板->搜索 Git ->Git:克隆中输入对应 http 地址->选择对应存储位置即可。

如何删除GitHub或者GitLab上的文件夹
- 情景：把本地仓库的一个文件夹 A 推送到了远程 GIT 服务器上，此时想删除远程仓库的文件夹 A ,但是本地又不想删除
- 问题：远程操作只能操作单个文件，无法删除文件夹

解决方案：
- 1.以删除 .setting 文件夹为案例
```git
git rm -r --cached  .setting  #--cached不会把本地的.setting删除
git commit -m 'delete .setting dir'
git push -u origin master
```

- 2.当误提交的文件夹比较多，直接修改 .gitignore 文件,将不需要的文件过滤掉，然后执行命令
```git
git rm -r --cached .
git add .
git commit
git push  -u origin master
```