# 搭建基于Hakyll的博客
***

## 环境
* 阿里云　Ubuntu 16.04

## 借助stack安装hakyll
各种版本的操作系统可以借鉴官网的说明，
```
curl -sSL https://get.haskellstack.org/ | sh　　  // 安装stack
stack setup                                     // 下载haskell的编译器到本地，放在~/.stack下面，独立于系统层面的haskell
stack install hakyll
```

## 借助github Page放置个人博客的主页
```
git clone git@github.com:Abraham9511/lincolnzjx.github.io.git
git checkout --orphan hakyll
git submodule add git@github.com:Abraham9511/lincolnzjx.github.io.git _site
git commit -m "Create hakyll branch"
git push -u origin hakyll
```

## Haskyll配置
```
hakyll-init .
stack init
stack build
```

## 站点的编译生成
```
stack exec site build
stack exec site watch
stack exec site clean
```

## 集成测试环境
```
git add --all
git commit -m "Configure Hakyll"
git push origin hakyll
```

### 配置文件
```
machine:
  ghc:
    version: 7.10.1

dependencies:
  cache_directories:
    - "~/.stack"
    - ".stack-work"
  pre:
    - curl -L https://github.com/commercialhaskell/stack/releases/download/v1.1.2/stack-1.1.2-linux-x86_64.tar.gz | tar zx -C /tmp
    - sudo mv /tmp/stack-1.1.2-linux-x86_64/stack /usr/bin
  override:
    - stack setup
    - stack build

test:
  override:
    - echo "skip test command"
  post:
    - git submodule init
    - git submodule update
    - cd _site/ && git checkout master
    - stack exec site build

deployment:

  production:
    branch: hakyll
    commands:
      - git config --global user.email circleci@circleci
      - git config --global user.name CircleCI
      - cd _site/ && git status
      - cd _site/ && git add --all
      - cd _site/ && git commit -m "Update (`date '+%F %T %Z'`) [ci skip]"
      - cd _site/ && git push origin master
```

## 进度
* 2017-6-12 按照教程完成，还需要对网站的样式，文章进行补充

### 参考资料
1. [使用 hakyll 作为新博客](https://birdgg.me/posts/2016-09-11-hakyll.html)
2. [haskellstack](https://docs.haskellstack.org/en/stable/README/)