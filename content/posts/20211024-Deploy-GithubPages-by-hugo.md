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

draft默认为true，不会被生成到网页中，编辑完成内容后将其修改为false以参与网页构建；或使用-D参数使草稿临时参与构建。

### 构建网页

```bash
hugo server -D
hugo
```

以上命令用于在本地生成，-D表示构建草稿。使用浏览器访问<http://localhost:1313>进行预览。

```bash
hugo
```

以上命令用于正式构建网页，默认构建在/public目录中。

## 将 hugo 与 github 建立连接

Github Pages中静态文件的存放位置有以下三种：（仓库中settings）

- main 分支
- main 分支下docs目录
- gh-pages 分支(前提是这个分支存在)

![Github Pages 静态页面设置](/20211024-Deploy-GithubPages-by-hugo/githu-pages-branch.png)

为实现hugo静态页面的发布，可以在config.toml中添加以下配置：

```toml
publishDir = 'docs'
```

此后运行hugo命令将会使生成的网页文件保存在/docs目录下。将整个代码仓库推送到GitHub的main分支上，并在settings中设置站点source为main /docs。访问`https://<username>.github.io`即可看到成果。
使用main分支的docs文件夹的好处是推一次代码就可以将源文档和构建的页面一起发布到GitHub中。如果希望对源文档和构建页面分别进行版本管理，则可以单独新建分支gh-pages（未测试）：参考<https://zhuanlan.zhihu.com/p/37752930>
无需修改hugo的publishdir，直接将/public子目录添加到.gitignore文件中，使main分支忽略其更新；之后新建分支gh-pages。

```bash
# 忽略public子目录
echo "public" >> .gitignore
# 初始化gh-pages branch
git checkout --orphan gh-pages
git reset --hard
git commit --allow-empty -m "Initializing gh-pages branch"
git push origin gh-pages
git checkout master
```

为了提高每次发布的效率，可以将下述命令存在脚本中，每次只需要运行该脚本即可将gh-pages branch中的文章发布到Github的repo中：

```bash
#!/bin/sh

if [[ $(git status -s) ]]
then
    echo "The working directory is dirty. Please commit any pending changes."
    exit 1;
fi

echo "Deleting old publication"
rm -rf public
mkdir public
rm -rf .git/worktrees/public/

echo "Checking out gh-pages branch into public"
git worktree add -B gh-pages public origin/gh-pages

echo "Removing existing files"
rm -rf public/*

echo "Generating site"
hugo

echo "Updating gh-pages branch"
cd public && git add --all && git commit -m "Publishing to gh-pages (publish.sh)"

echo "Push to origin"
git push origin gh-pages
```

最后将main分支中的源文档和gh-pages分支h中的网页文档分别push到Github仓库中，进入settings将Source选定gh-pages即可。

## 添加个人域名

待补充...
