---
title: Travis-CI 自动化部署 Github Pages
date: 2019-05-07 00:27:32
tags: TravisCI
---

这个科技在第一次搭 `Github Pages` 的时候看见过，但是当时觉得太麻烦了，就没有管这个。已经半年没有写过博客，没想到新的一篇博客时在这样的情况下写出来的。

因为 `ZJU` 普通计科需要必修 “面向信息技术的沟通技巧”，第一节课需要每组抽一个产品下一次课前做一个推荐视频，当堂展示。我们组正好抽到了 `Travis-CI` 这个东西。抽过来一脸蒙蔽，组里没一个人用过。于是我接了做几个 `Travis-CI` 的例子用来展示的锅，趁着学这个东西，顺手把自己的博客和 `Travis-CI` 集成一下，本身也很方便。

<!-- more -->

主要参考了这个 [blog](https://zespia.tw/blog/2015/01/21/continuous-deployment-to-github-with-travis/) 里的内容。

## 0x00

首先需要生成一对公密钥，`TravisCI` 和 `Github Pages` 对接的时候，显然不可能把自己的密钥直接 `push` 到 `repo` 里，然后在 `TravisCI` 的虚拟机里做 `deploy`，所以 `Github Pages` 的 `repo` 支持单独的添加一对专门用于 `deploy` 的公密钥。

生成公密钥，注意不要把自己原来的覆盖掉：
```bash
ssh-keygen -t rsa -C "dydxh1998@gmail.com"
```

然后把公钥贴到 `username.github.io` 这个 `repo` 的 `setting` 里 `Deploy Keys` 那一栏，注意要勾选写权限，`ReadOnly` 的话在 `TravisCI` 里 `hexo deploy` 的时候会报错。

## 0x01

然后安装 `Travis-CI` 自己的命令行工具，主要是用于生成加密后的密钥，可以安全的传输到 `Travis-CI` 的虚拟机里。

```bash
gem install travis
travis login --auto
travis encrypt-file ~/.ssh/deploy/id_rsa --add
```

注意是把密钥喂进去，最后一步会生成 `id_rsa.enc` 是加密后的密钥，在这里 `Travis-CI` 先用登录你的帐号，然后给每个帐号生成一个用来加密的 `Key` 和 `IV`，这两个加密的东西都会在自动部署的时候，出现在 `Travis-CI` 的虚拟机的环境变量里，做到了很好的保密性。

然后最后一步还会在 `.travis.yml` 里加一句用来解密密钥的 `openssl` 命令，比较沙雕的是它给路径里多加了几个反斜杠 `\`, 不删掉的话，在 `Travis-CI` 执行这句的时候会报错找不到文件。

## 0x02

然后需要准备用来 `deploy` 的 `ssh_config`

```bash
Host github.com
  User git
  StrictHostKeyChecking no
  IdentityFile ~/.ssh/id_rsa
  IdentitiesOnly yes
```

## 0x03

再写一下 `.travis.yml`

```yml
language: node_js
node_js:
  - '11'
before_install:
  - openssl aes-256-cbc -K $encrypted_a8afbadcb2d6_key -iv $encrypted_a8afbadcb2d6_iv
    -in .travis/id_rsa.enc -out ~/.ssh/id_rsa -d
  - chmod 600 ~/.ssh/id_rsa
  - eval $(ssh-agent)
  - ssh-add ~/.ssh/id_rsa
  - cp .travis/ssh_config ~/.ssh/config
  - git config --global user.name "dydxh"
  - git config --global user.email "dydxh1998@gmail.com"
  - npm install hexo -g
script:
  - hexo generate
  - hexo deploy
branches:
  only:
    - source
```

注意这里我把 `ssh_config` 和 `id_rsa.enc` 放到了 `.travis` 文件夹里，然后我的原稿在 `source` 分支里。

另外一点，`Travis-CI` 默认的 `NodeJS` 版本好像比较迷，写这个 `blog` 的时候是 `v0.10.48`，但是 `hexo` 比较新的版本已经不支持版本低于 `6` 的 `NodeJS` 了，我本机上是 `v11.14.0` 的，填上去能正常跑。


## 0x04

第一次接触持续集成的东西，简单体验了一下，感觉很强大，打算这学期几个大程都着手用一下，做做测试什么的挺好的。
