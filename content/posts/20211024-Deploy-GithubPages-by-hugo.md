---
title: "使用 Hugo 搭建 Github Pages"
tags: [ "Github Pages", "Hugo"]
categories: [ "Github Pages" ]
date: 2021-10-24T20:15:26+08:00
draft: false
---

## Github Pages

待补充...

## Hugo

官网连接： [Hugo_Quick-Start](https://gohugo.io/getting-started/quick-start/)

### 下载 Hugo

Github <https://github.com/gohugoio/hugo/releases>

Windows中无需安装，解压到喜欢的位置后将hugo.exe所在路径添加到环境变量中（可选）即可。
在git bash中敲入一下命令确认可以执行；不建议使用Powershell，因为后续使用echo等命令时会造成乱码。

```bash
hugo version
```

### 创建 Hugo 站点

```bash
hugo new site quickstart
```

找到一个你喜欢在本地存放代码的文件夹，执行这段代码，然后存放代码仓库的文件夹“quickstart”将会被创建，内部已经生成了Hugo所必须的一些代码。此时可以切换到代码仓库：

```bash
cd quickstart
```

### 站点配置

使用git初始化代码仓库：

```bash
git init
```

从[Hugo主题网站上](https://themes.gohugo.io/)找一个喜欢的主题，使用git同步到本地代码仓库的themes文件夹。下面挑了两个比较喜欢的：

```bash
git submodule add https://github.com/dillonzq/LoveIt themes/LoveIt
git submodule add https://github.com/google/docsy themes/docsy
```

将主题信息写入hugo配置文件config.toml

在config.toml添加一行

```toml
theme = "LoveIt"
```

同时可以配置一下其他信息，某些主题会有特定的规范。
以[LoveIt主题](https://hugoloveit.com/zh-cn/theme-documentation-basics/#basic-configuration)为例

```toml
baseURL = "http://example.org/"

defaultContentLanguage = "zh-cn" # 默认语言 [en, zh-cn, fr, ...] 
languageCode = "zh-CN" # 网站语言, 仅在这里 CN 大写
hasCJKLanguage = true # 是否包括中日韩文字
title = "我的全新 Hugo 网站" # 网站标题

# 更改使用 Hugo 构建网站时使用的默认主题
theme = "LoveIt"

[params]
  # LoveIt 主题版本
  version = "0.2.X"

[menu]
  [[menu.main]]
    identifier = "posts"
    # 你可以在名称 (允许 HTML 格式) 之前添加其他信息, 例如图标
    pre = ""
    # 你可以在名称 (允许 HTML 格式) 之后添加其他信息, 例如图标
    post = ""
    name = "文章"
    url = "/posts/"
    # 当你将鼠标悬停在此菜单链接上时, 将显示的标题
    title = ""
    weight = 1
  [[menu.main]]
    identifier = "tags"
    pre = ""
    post = ""
    name = "标签"
    url = "/tags/"
    title = ""
    weight = 2
  [[menu.main]]
    identifier = "categories"
    pre = ""
    post = ""
    name = "分类"
    url = "/categories/"
    title = ""
    weight = 3

# Hugo 解析文档的配置
[markup]
  # 语法高亮设置 (https://gohugo.io/content-management/syntax-highlighting)
  [markup.highlight]
    # false 是必要的设置 (https://github.com/dillonzq/LoveIt/issues/158)
    noClasses = false
```

添加代码行号
上述配置并不能将网页内代码行号显示出来，因此在[markup.highlight]下添加配置[（具体参见官方文档）](https://gohugo.io/content-management/syntax-highlighting/)：

```toml
linenos = true
```

其他未测试配置 来自<https://huangzhongde.cn/post/2020-02-22-hugo-code-linenumber/>

```toml
pygmentsUseClasses = true
[markup]
  [markup.highlight]
    codeFences = true
    guessSyntax = true
    hl_Lines = ""
    lineNoStart = 1
    lineNos = true
    lineNumbersInTable = false
    noClasses = true
    style = "github"
    tabWidth = 4
```

防复制行号（未测试、可能使用的主题不需要配置）

```css
.highlight .ln {
    width: 20px;
    display: block;
    float: left;
    text-align: right;
    user-select: none; /* 复制是不能被选中，其他的是格式上的调整 */
    padding-right: 8px;
    color: #ccc;
}
```

### 创建文章

```bash
hugo new <content内文件夹路径>/<文章文件名.md>
hugo new posts/my-first-post.md
```

以上命令在代码仓库中content\posts文件夹中创建文件"my-first-post.md"

打开"my-first-post.md"，文件开头"---"之间已经存在预先生成的信息：

```md
---
title: "my-first-post"
date: 2021-10-21T20:09:21+08:00
draft: true
---
```

待补充...