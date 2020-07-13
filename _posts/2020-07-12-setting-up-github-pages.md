---
layout: post
title:  "使用Github Pages搭建自己的站点"
date:   2020-07-12 22:13:12 +0800
categories: github pages jekyll
---

## 写在前面 
有时候想把工作或生活中的一些想法记录下来，需要一个写博客的地方，虽然国内有很多托管博客的平台，像CSDN、博客园、微信公众号等，但受限于平台审核，文章内容需要符合监管要 求才能发表。自己搭建服务又太耗费精力和财力，好在Github Pages免费提供站点托管服务，而且搭建非常简单，很容易创建自己的站点。

本文简单记录Github Pages的搭建过程，以供自己以后查阅。 

## Github Pages
[Github
Pages](ref-githubpages)是一个使用HTML，CSS和JavaScript文件托管的静态资源托管服务。你可以通过构建处理程序运行这些文件并发布站点。你可以在[这里](https://github.com/collections/github-pages-examples)查看更多关于Github
Pages的例子。

你可以使用`github.io`或自定义域名，关于自定义域名可以参考[这里](https://docs.github.com/en/articles/creating-a-github-pages-site)。

## Jekyll
[Jekyll](ref-Jekyll)是一个web静态资源生成工具，可以将普通文本转换成web站点静态资源和博客。其特点是：

- **简单** ：无数据库，无评审审核，无繁琐的升级安装，你只需要关注你的内容即可。
- **静态化**：支持[Markdown](ref-Markdown)，[Liquid](ref-Liquid)，HTML&CSS，生成的静态资源可以直接部署。
- **博客组件**：原生支持Permalinks，categories，pages，posts和自定义布局。

## 依赖环境 
`Github Pages`建议使用Jekyll生成静态资源，Jekyll是一个[Ruby Gem](ref-rubygem)，支持多种操作系统，本站点在`CentOS
6.5`操作系统上搭建。

- [Ruby](ref-ruby)

ruby需要2.5或以上版本，可以通过`ruby -v`查看版本号。

- [RubyGems](https://rubygems.org/pages/download)

需要安装ruby gem，可以通过`gem -v`确认。

- [GCC](ref-gcc)

```
$ gcc -v
...
Thread model: posix
gcc version 4.7.2 20121015 (Red Hat 4.7.2-5) (GCC)
```

- [Make](ref-make)：

```
$ make -v
GNU Make 3.81
Copyright (C) 2006  Free Software Foundation, Inc.
...
```

## 搭建步骤

> ### Part-1：创建github pages

**1. 创建代码仓库**

首先你需要注册一个[github](ref-github)账号，然后[创建](https://github.com/new)一个代码仓库，命名为：`username.github.io`，其中`username`是你在`github`中的用户名。

**2. 克隆代码仓库**

选择一个你想保存站点的目录，克隆代码仓库：

```bash
$ git clone https://github.com/username/username.github.io
```

**3. Hello World**

```bash
$ cd username.githumb.io
$ echo "Hello World" > index.md
```

**4. 提交代码**

```bash
$ git add --all
$ git commit -m "Initial commit"
$ git push -u origin master
```

**5. 访问站点**

打开浏览器，访问` https://username.github.io`，是不是可以看到`Hello World`了。

> ### Part-2：使用Jekyll生成静态资源

**1. 安装依赖环境**

安装上文提前到依赖环境。

**2. 安装jekyll和bundler gems**

```
$ gem install jekyll bundler
```

**3. 创建一个Jekyll站点**

```
$ jekyll new myblog
```

**4. 编译并启动本地站点服务**

```
$ cd myblog
$ bundle exec jekyll serve
```

**5. 访问本地站点**

打开浏览器，访问`http://localhost:4000`即可看到默认主页。

`jekyll serve`支持指定服务端口和host，如`bundle exec jekyll serve --port 8888 --host
127.0.0.1`，更多命令行参数请参考[配置可选项说明](https://jekyllrb.com/docs/configuration/options/)。

> ### Part-3：发布

`Part-2`是利用`Jekyll`在本地运行了站点服务，在本地测试没有问题后，需要将`Jekyll`生成的静态资源提交到github上。

**1. 添加静态资源**

```
$ cp -r myblog/* /path/to/username.github.io
```

需要注意的是，如果你运行过`bundle exec jekyll serve`，则会生成_site目录，github
pages会根据你提交的内容重新编译，生成新的_site目录，所以该目录并不用提交。

**2. 修改配置**

进入`/path/to/username.github.io`目录，打开`Gemfile`文件，你会看到下面这段话：

```
# If you want to use GitHub Pages, remove the "gem "jekyll"" above and
# uncomment the line below. To upgrade, run `bundle update github-pages`.
# gem "github-pages", group: :jekyll_plugins
```

如果需要让`github pages`托管，需要取消行`gem "github-pages"`注释，移除`gem "jekyll"`，同时需要使用当前`github
pages`所使用的版本号替换*VERSION*，关于版本号请参考[Dependency versions](ref-depversion)。

```
gem "github-pages", "~> VERSION", group: :jekyll_plugins
```

更新`Gemfile`并保存。

**3. 提交到github**

```
$ git add --all
$ git commit -m "add jekyll blog template"
$ git push -u origin master
```

**4. 查看发布内容** 

打开浏览器，访问`https://algopeek.github.io`，是不是看到内容更新了，enjoy~

到这里，已经搭建完成，如果你在搭建过程中也遇到一些问题，下面这些`trouble shooting`可能会帮到你。

## 遇到的问题

**1. cannot load such file -- openssl (LoadError)**

手动安装ruby时报错如下：

```
$ sudo yum install openssl-devel
cannot load such file -- openssl (LoadError)
```

解决办法：
```
# 进入ruby的安装包目录
$ cd /ruby-dir/ext/openssl 
$ ruby extconf.rb
$ make && make install
```

关于该问题更详细的讨论参考[这里](https://bundler.io/doc/troubleshooting.html)。


**2. cannot load such file -- zlib**

安装jekyll和bundler时报错如下：
```
$ gem install jekyll bundler
ERROR:  Loading command: install (LoadError)
    cannot load such file -- zlib
ERROR:  While executing gem ... (NoMethodError)
    undefined method 'invoke_with_build_args' for nil:NilClass
```

解决办法：
```
# 进入ruby源码文件夹 
# 安装ruby自身提供的zlib包 
$ cd ext/zlib
$ ruby ./extconf.rb
$ make
$ make install
```

关于该问题更详细讨论参考[这里](http://www.51testing.com/html/77/497177-3709664.html)。

**3. cc1plus: error: unrecognized command line option "-std=c++11"**

[StackOverflow](https://stackoverflow.com/questions/14674597/cc1plus-error-unrecognized-command-line-option-std-c11-with-g)给出了答案：

> In versions pre-G++ 4.7, you will have to use -std=c++0x, for more recent versions you can use -std=c++11.

原因是我操作系统自带的gcc太老了，需要升级，具体升级方案比较多，我主要参考[这里](https://superuser.com/questions/381160/how-to-install-gcc-4-7-x-4-8-x-on-centos)。

**4. git push: 403 Forbidden** 

推送代码时报错如下：

```
$ git push -u origin master
error: The requested URL returned error: 403 Forbidden while accessing
https://github.com/AlgoPeek/algopeek.github.io.git/info/refs
fatal: HTTP request failed
```

我处理的比较暴力，将`HTTPS`协议替换`SSH`协议，具体参考[这里](https://docs.github.com/en/github/using-git/changing-a-remotes-url)

--- 完---

[ref-githubpages]: https://docs.github.com/en/github/working-with-github-pages/about-github-pages
[ref-Markdown]: https://daringfireball.net/projects/markdown
[ref-Liquid]: https://github.com/Shopify/liquid/wiki
[ref-Jekyll]: https://jekyllrb.com
[ref-github]: https://github.com
[ref-rubgem]: https://jekyllrb.com/docs/ruby-101/#gems
[ref-ruby]: https://www.ruby-lang.org/en/downloads
[ref-gcc]: https://gcc.gnu.org/install
[ref-make]: https://www.gnu.org/software/make
[ref-depversion]: https://pages.github.com/versions
