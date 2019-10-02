---
title: Hello World
---
hexo文档： [documentation](https://hexo.io/docs/)

## Quick Start

### 新建一篇文章

``` bash
$ hexo new "My New Post"
```

### 运行服务器

``` bash
$ hexo server
```

### 生成静态文件

``` bash
$ hexo generate / hexo g
```

### 发布到远端

``` bash
$ hexo deploy / hexo d
```

## Hexo博客 同步管理 & 迁移

> 本文的博客搭在github pages上

### 仓库的构建：

1. 在原电脑上操作，给你的博客仓库创建hexo分支，并设为默认分支。

2. 执行`git clone git@github.com:username/username.github.io.git`把仓库 clone 到本地。

3. 进入刚才 clone 到本地的仓库，删掉除了 .git 文件夹以外的所有内容。

4. 命令行 cd 到 clone 的仓库，git add .，git commit -m "--"，git push origin hexo，把刚才删除操作引起的本地仓库变化更新到远程，此时刷新下 github 端博客hexo分支，应该已经被清空了。

5. 将上述 .git 文件夹复制到本机本地博客根目录下（即含有 themes、source 等文件夹的那个目录）

6. 将博客目录下 themes 文件夹下每个主题文件夹里面的 .git .gitignore 删掉。

7. cd 到博客目录，git add .，git commit -m "--"，git push origin hexo，将博客目录下所有文件更新到 hexo 分支。至此原电脑上的操作结束。

### 迁移

1. 必要的环境安装，详见官方文档

2. 选好博客安装的目录， `git clone git@github.com:username/username.github.io.git`

3. cd 到博客目录，npm install、hexo g && hexo s，安装依赖，生成和启动博客服务。正常的话，浏览器打开 localhost:4000 可以看到博客了。至此新电脑操作完毕。

以后无论在哪台电脑上，更新以及提交博客，依次执行`git pull，git add .，git commit -m "message"，git push origin hexo，hexo clean && hexo g && hexo d` 即可。

