---
layout: post
title:  "How to Create Website by Jekyll and Github Pages"
date:   2023-12-17 18:57:16 +0800
categories: jekyll update
---
## 视频教程

[bilibili](https://www.bilibili.com/video/BV1qs41157ZZ?p=12&vd_source=6a3f8a4ac0085023617b175841321a65)

[youtube](https://www.youtube.com/playlist?list=PLLAZ4kZ9dFpOPV5C5Ay0pHaa0RJFhcmcB)

## Ubutu 安装Jekyll

```bash
# 安装Ruby和相关依赖
sudo apt-get install ruby-full build-essential zlib1g-dev
# 设置环境变量
echo '# Install Ruby Gems to ~/gems' >> ~/.bashrc
echo 'export GEM_HOME="$HOME/gems"' >> ~/.bashrc
echo 'export PATH="$HOME/gems/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
# 安装Jekyll和bundler
gem install jekyll bundler
```

上述方法在Ubuntu20.04上只能安装Ruby 2.70，而gem install jekyll bundler需要Ruby3.0，因此使用 RVM 安装 Ruby

RVM 是一个命令行工具，可用于安装、管理和使用多个 Ruby 环境。

安装从源代码构建 Ruby 所需的依赖项：

```bash
sudo apt update
sudo apt install curl g++ gcc autoconf automake bison libc6-dev \        libffi-dev libgdbm-dev libncurses5-dev libsqlite3-dev libtool \        libyaml-dev make pkg-config sqlite3 zlib1g-dev libgmp-dev \        libreadline-dev libssl-dev
```

运行以下命令添加 GPG 密钥并安装 RVM：

```bash
curl -sSL https://rvm.io/mpapis.asc | gpg --import -
curl -sSL https://rvm.io/pkuczynski.asc | gpg --import -
curl -sSL https://get.rvm.io | bash -s stable
```

要开始使用 RVM，请使用 [`source`](https://linuxize.com/post/bash-source-command/) 加载脚本环境变量 命令：

```bash
source ~/.rvm/scripts/rvm
```

要获取可以使用此工具安装的所有 Ruby 版本的列表，请键入：

```bash
rvm list known
```

更新Ruby安装jekyll

```bash
rvm install 3.0
rvm use 3.0.0 -default
rvm -v
rvm gemset update
gem install jekyll
jekyll -v
#在bashrc添加下述语句，用于在新终端激活rvm，否则在会提示Command 'jekyll' not found
echo 'source ~/.rvm/scripts/rvm' >> ~/.bashrc
```



## 使用Jekyll命令创建一个默认网站

```bash
# 创建一个网站名称为self_blog的默认博客
jekyll new self_blog
```

会生成一个名为self_blog的文件夹，里面Jekyll生成的一个基础网站的是默认文件

```bash
# 使用Jekyll编译这些文件并生成网站
cd self_blog
bundle exec jekyll serve
```

此时jekyll会开始在本地建立这个网站的服务

终端中会生成一系列信息，其中`Serve address：`后面的就是网站的本地网址，在网页中打开就可以看到该网站

## Jekyll生存成的默认文件介绍

`_posts：`存储博客和文章

`_site：`存储这最终完成的网页信息，类似于C++编译好的可执行文件，只需要这个文件夹就可以在别的服务器上运行网页了

`_config.yml：`网页的配置文件，类似与设置

`Gmefile：`存储这个网页的依赖，类似于C++的CmakeList，包括主题等

`index.md：`网页中主页中的内容

`about.md：`网页中about页中的内容

## Jekyll进阶使用

### 头信息（Frontmater）

每个博客使用markdown书写，头信息是一篇博客的基本信息，在每个markdown文件的开头。

例如`_posts`文件夹中的markdown博客的开头中第前几行：

```markdown
---
# 博客的版面（相当于一个文章排版的格式）
layout: post
# 博客的名称
title:  "Welcome to Jekyll!"
# 博客的日期
date:   2023-12-17 15:47:16 +0800
# 和date一起决定了该博客所在网页存在的位置URL如http://127.0.0.1:4000/jekyll/update/1997/07/10/welcome-to-jekyll.html
# 在编译好的_site文件夹的上述路径可以找到存储这篇编译好的博客的HTML
categories: jekyll update
---
```

头信息可以使用YAML或者JSON语言写，两种语言都是用来定义键值对

除了上述头信息，也可以自定义一些头信息，例如：author: "Shihao Li"，然后再通过HTML的自定义变成将这个头信息先是在Jekyll布局中。

实际使用中，直接使用模板的头信息就可以

### 创建一个新的博客

一个博客可以使用Markdown和HTML书写，这里一Markdown为例。

- 创建一个新的博客需要在`_posts`文件夹下创建，其命名规则需满足：`nnnn-yy-rr-xxxxxx.md`其中`nnnn-yy-rr`为年月日，可以随便给年月日，因为可以在博客的头信息中设置显示的日期

- 添加头信息

  ```markdown
  ---
  # 如果没有会直接以一个markdown的排版编译
  layout: post
  # 如果没有会以文件名中的xxxxxx作为标题
  title:  "Welcome to Jekyll!"
  # 如果没有会以文件名中的nnnn-yy-rr作为标题
  date:   2023-12-17 15:47:16 +0800
  # 如果没有博客对应网页的存储的位置URL就会少一个分级，如http://127.0.0.1:4000/2023/12/17/test-blog.html
  categories: jekyll update
  ---
  ```

- 添加内容

- 也可以在`_posts`文件夹下创建其他文件夹来分级管理博客

### 使用草稿

- 在主目录下创建`_drafts`文件夹来存储草稿

- 此时`jekyll serve`编译的网页不会显示草稿文件夹中的博客

- 使用

  ```bash
  jekyll serve --draft
  ```

  可以在网页看到草稿的博客

- 当草稿的博客没有头信息时，它会被分配一个使用`jekyll serve --draft`命令时的日期

### 创建页面

- 在主目录下创建一个Markdown文件

- 输入头信息

  ```mark
  ---
  # 版面（网页）
  layout: page
  # 名称
  title:  "Test Page"
  # 定义了网页的URL，如http://127.0.0.1:4000/about/
  # 在编译好的_site文件夹的上述路径可以找到存储这篇编译好的博客的HTML
  # 如果和其他page的permalink相同，会使两个page现实的内容一样，因为文件被覆盖了
  permalink: /about/
  ---
  ```

- 写入网页内容

- 也可以通过在根目录下创建文件夹来分级管理网页

### 永久链接（permalink）

头信息中的`permalink`可以指定编译好的网页存储的指定路径，即指定页面或博客对应的URL。

如果在前面的blog的头信息中添加这句，则blog对应的存储位置或URL不会根据`date`和`categories`设置，而会直接设置为`permalink`

或者使用

```markdown
permalink: \:categories\:date\:year\:month\:title
```

来引用头信息中的其他变量来作为URL

### 头信息的默认值

可以在`_config.yml`文件中设置一些头信息变量的默认值，使得不用每篇博客都需要重新写头信息

在`_config.yml`文件中添加

```yaml
defaults:
	-
		# 默认值适用的范围
		scope:
			path: "" # 这些默认值适用于文件的路径，如果为""则是所有文件，如果是"file"则只对file文件夹下的文件其作用
			type: "post" # 这些默认值适用与哪类文件，如果为"post"则所有_post文件夹下的文章都使用上述默认值
		# 具体需要设置的默认值
		values:
			layout: "post" # 把layout设置为"post"
```

设置万需要重新运行

```bash 
jekell serve
```

### 主题

可以在`_config.yml`文件的`theme`设置主题，默认的主题是minima

- 可以在[https://rubygems.org](https://rubygems.org)中找到一些主题，比图向使用的主题的名称为find-theme

- 打开`Gemfile`设置依赖，添加

  ```gfm
  gem "find-theme"
  ```

- 打开终端，使用

  ```bash
  # 下载Gemfile中的依赖
  bundle install
  ```

- 在`_config.yml`文件的`theme`修改为find-theme

- 注意新的主题中的`layout`的可选项可能和之前的不一样，需要阅读新主题的文件进行修改。可以直接在其`_layouts`文件夹下查看

### 布局（layout）

可以在根目录下创建`_layouts`文件夹，然后在里面定义新的布局

- 在根目录下创建`_layouts`文件夹

- 创建新的布局的HTML文件，例如post.html

- 每个布局中可以通过layout:来继承其他的布局

  ```html
  ---
  layout: other_layout
  ---
  ```

- 详细的见视频

### HTML文件介绍

#### 常用变量

`<h1>xxx<h1>`:第一行的内容为xxx

`<h2>xxx<h2>`:第一行的内容为xxx

`<hr>`：横线

`{{ comtent }}`:markdown内容的占位符

`{{ layout.xxx }}`:当前版式（HTML文件）中的头信息中设置的xxx键对应的值

`{{ page.xxx }}`:使用该版式的页面或博客（Markdowm文件）中在文件头信息中设置的xxx键对应的值

`{{ site.xxx }}`:设置中（_config.yml文件）中在文件头信息中设置的xxx键对应的值

#### Include

在一个HTML文件中引入其他HTML文件的代码

- 在根目录下创建一个HTML文件，如header.html

- 然后在要引用header.html的HTML文件中输入

  ```html
  {% include header.html %}
  ```

### 遍历

```html
# 对于每个_posts文件夹中的文件遍历
{% for post in site.posts %}

{% endfor %}
```

### 条件语句

```html
# 判断文件中的tittle是否等于"test"
{% if page.tittle == "test" %}

{% else %}

{% endif %}
```

#### 数据文件

可以存储一些数据

- 在根目录下创建`_data`文件夹
- 创建数据文件，数据文件可以使用YAML，JSON，CSV文件
- 使用site.data.文件名访问

#### 静态文件

存储一些网页的资源

- 在根目录下创建一个文件夹存储这些资源，如`assets`
- 使用site.static_files来索引

## Github Pages

- 创建一个gihub仓库，比如仓库名为page
- 将`_config.yml`里的baseurl修改为"/仓库名"比如“/page”
- 将代码上传
- 网站的链接为https://github用户名.github.io/page/
