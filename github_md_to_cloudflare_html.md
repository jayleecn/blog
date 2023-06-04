---
layout: default
title: 如何将Github的markdown文件，部署在Cloudflare上变成html
---

## 目标：
1. 用markdown写文档，托管在Github上做管理
2. 部署到Cloudflare上变成html，方便外部访问

## 过程：
按照ChatGPT4的前几次回答，部署都失败了。不过贴了Cloudflare的部署日志，多问了几次后，部署成功了。
把步骤记录下，适合像我这样的麻瓜阅读，减少后人踩坑。

下面是详细的步骤，在Cloudflare上构建时，选了用Jekyll把md转为html。

#### 步骤1：在Github上创建仓库

首先，你需要一个包含你的Markdown文档的Github仓库。你可以点击右上角的"+"符号，然后选择"New repository"来创建一个新的仓库。

#### 步骤2：创建Markdown文档

在你的Github仓库中，你可以创建或上传你的Markdown文档。Markdown文档通常以.md作为文件后缀。

#### 步骤3：创建Gemfile（无后缀）

在你的Github仓库根目录中，创建一个名为"**Gemfile**"的文件。Gemfile用来管理Ruby项目的依赖关系。将下面的内容复制粘贴到Gemfile中：

```ruby
source 'https://rubygems.org'
gem 'jekyll', '~> 4.2.0'
```

这里我们指定了Jekyll的版本为4.2.0，你也可以选择其它的版本。

#### 步骤4：配置Jekyll

接下来，我们需要配置Jekyll。

1. 在你的Github仓库根目录中，创建一个名为"\_config.yml"的文件，这是Jekyll的配置文件。你可以添加如下内容：

   ```yaml
   markdown: kramdown
   ```

   这会告诉Jekyll使用kramdown作为Markdown的转换器。

2. 创建一个\_layouts目录并在其中创建一个名为"default.html"的文件。此文件是你的站点的默认布局。你可以添加如下内容：

   ```html
   <!DOCTYPE html>
   <html>
     <head>
       <title>{{ page.title }}</title>
     </head>
     <body>
       {{ content }}
     </body>
   </html>
   ```

   此文件会将Markdown内容放入HTML的body标签中。

3. 对于你的每一个Markdown文件，你需要在文件的顶部添加一段YAML前导事项，如下所示：

   ```markdown
   ---
   layout: default
   title: Your Document's Title
   ---

   Your markdown content here...
   ```

   这会告诉Jekyll使用"default"布局，并设置文档的标题。

#### 步骤5：部署到Cloudflare Pages

现在你已经准备好部署你的站点到Cloudflare Pages了。

1. 登录到你的Cloudflare账户，然后选择"Pages"。

2. 点击"Create a project"，然后选择你刚刚创建的Github仓库。

3. 在下一步，你需要设置Cloudflare Pages的构建设置。
- Framework preset：选择 `Jekyll`
- Build command：输入 `bundle install && bundle exec jekyll build`
- 其他：都用默认值即可

4. 点击"Begin setup"，然后在下一页，点击"Save and Deploy"。

5. Cloudflare会开始构建和部署你的项目。构建完成后，你的站点将会被部署到一个*.pages.dev的子域名上。

请注意，每次你在Github上提交新的更改，Cloudflare Pages都会自动重新构建和部署你的站点。如果你需要一个自定义的域名，你可以在Cloudflare Pages的设置中添加一个。

至此，大功告成。

比如，本篇md文档，部署到Cloudflare后，可以通过：Cloudflare 来访问

#### You:
Gemfile 有后缀吗？

#### ChatGPT:
`Gemfile` 是没有后缀的。它是一个纯文本文件，用于列出 Ruby 应用程序的 gem 依赖关系。Ruby 的 gem 是类似于库或包的可复用组件，用于扩展 Ruby 应用程序的功能。`Gemfile` 通常位于项目的根目录中，并由 Bundler（一个 Ruby 依赖管理工具）使用。

在这个 `Gemfile` 中，你可以指定你的应用程序依赖哪些 gem，以及这些 gem 的版本。当你运行 `bundle install` 命令时，Bundler 会查看 `Gemfile`，并安装列出的所有 gem 及其依赖关系。

所以，你应该在你的项目根目录下创建一个名为 `Gemfile` 的文件，不需要任何后缀。在这个 `Gemfile` 中，你可以指定要使用的 Jekyll 的版本，如我之前所示的那样。
