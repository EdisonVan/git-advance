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