title: Npm、Yarn详解
date: 2021-01-14 13:55:06
tags:
- MacOs M1
categories:
- MacOs
language: zh-CN
toc: true
providers:
    cdn: loli
    fontcdn: loli
    iconcdn: loli
cover: /gallery/covers/DD Vector Landscape Scene 65463.svg
thumbnail: /gallery/covers/DD Vector Landscape Scene 65463.svg
---

记录日常使用到的 npm 灬 yarn 常用命令

## 基本命令

**查看全局安装的包**

{% codeblock "bash zsh" lang:zsh %}
npm list -g --depth 0
yarn global list --depth 0
{% endcodeblock %}

**查看当前包命名是否被使用**

{% codeblock "bash zsh" lang:bash %}
npm view <packageName>
{% endcodeblock %}

被使用则会返回当前使用此名称包的信息，**404**代表未被使用

**查看包版本**

{% codeblock "bash zsh" lang:bash %}
npm view <packageName> version # 查看某个模块的最新版本
npm view <packageName> versions # 查看某个模块的所有历史版本
{% endcodeblock %}

<!-- more -->

## package.json详解

如果你使用过 `Nodejs` ，则可能会遇到 **package.json** 文件。它是一个 `JSON` 文件，位于项目根目录。你的 **package.json** 包含关于项目的重要信息。它包含关于项目的使人类可读元数据（如项目名称和说明）以及功能元数据（如程序包版本号和程序所需的依赖项列表）。

{% raw %}<article class="message is-info"><div class="message-body">{% endraw %}
每个字段的含义可查看[官方文档](https://docs.npmjs.com/cli/v7/configuring-npm/package-json)
{% raw %}</div></article>{% endraw %}

**package.json** 示例如下所示：

{% codeblock package.json lang:json %}
{
  "name": "my-project",
  "version": "1.5.0",
  "description": "Express server project using compression",
  "main": "src/index.js",
  "scripts": {
    "start": "node index.js",
    "dev": "nodemon",
    "lint": "eslint **/*.js"
  },
  "dependencies": {
    "express": "^4.16.4",
    "compression": "~1.7.4"
  },
  "devDependencies": {
    "eslint": "^5.16.0",
    "nodemon": "^1.18.11"
  },
  "repository": {
    "type": "git",
    "url": "https://github.com/osiolabs/example.git"
  },
  "author": "Jon Church",
  "contributors": [{
    "name": "Amber Matz",
    "email": "example@example.com",
    "url": "https://www.osiolabs.com/#team"
  }],
  "keywords": ["server", "osiolabs", "express", "compression"]
}
{% endcodeblock %}

### package.json 的用途是什么？

项目的 **package.json** 是配置和描述如何与程序交互和运行的中心。 **npm CLI**（和 **yarn**）用它来识别你的项目并了解如何处理项目的依赖关系。**package.json** 文件使 **npm** 可以启动你的项目、运行脚本、安装依赖项、发布到 **NPM** 注册表以及许多其他有用的任务。 **npm CLI** 也是管理 **package.json** 的最佳方法，因为它有助于在项目的整个生命周期内生成和更新 **package.json** 文件。

**package.json** 会在项目的生命周期中扮演多个角色，其中某些角色仅适用于发布到 **NPM** 的软件包。即使你没有把项目发布到 **NPM** 注册表中，或者没有将其公开发布给其他人，那么 **package.json** 对于开发流程仍然至关重要。

你的项目还必须包含 **package.json**，然后才能从 **NPM** 安装软件包。这可能是你在项目中需要它的主要原因之一。

### package.json 中的常见字段

- **name**
  **`name` 字段定义包的名称**。发布到 NPM 注册表时，这是软件包将在其中显示的名称。它不能超过 **214** 个字符，只能是小写字母，并且必须是URL安全的（允许连字符和下划线，但 URL 中不允许使用空格或其他字符）。

  **如果将软件包发布到 NPM，则 `name` 属性是必需的**，并且必须是唯一的。如果尝试用 NPM 注册表上当前已经使用的名称发布程序包，则会收到错误消息。如果你的软件包并不是要发布到 NPM 上，则 `name` 不必是唯一的。
- **version**
  `version` 字段对于任何已发布的软件包都非常重要，并且在发布之前是必填的。这是 package.json 描述的软件的当前版本。

  通常情况下我们并不需要使用 [Semver](https://docs.npmjs.com/about-semantic-versioning) 来管控版本，通常在将新版本发布到 NPM 之前，根据 `SemVer`，版本号会增加。[了解有关语义版本控制（semantic versioning）的更多信息](https://heynode.com/tutorial/how-use-semantic-versioning-npm)。

  - 补丁版本 ~1.0.4
  - 次要版本 ^1.0.4
  - 正式版本 1.0.4
  
  |  代码状态   | 阶段  | 规则 | 示例版本 |
  |  ----  | ----  | ----  | ----  |
  | 初版 | 新产品 |	从1.0.0开始	| 1.0.0 |
  | 向后兼容的错误修复	| 补丁发布	| 递增第三位数	| 1.0.1 |
  | 向后兼容的新功能	| 轻微释放	| 递增中间数字并将最后一位重置为零	| 1.1.0 |
  | 更改会破坏向后兼容性	| 主要发行	| 递增第一位并将中间和最后一位重置为零	| 2.0.0 |
- **license**
  这是非常重要但经常被忽略的属性。`license` 字段使我们可以定义适用于 package.json 所描述代码的许可证。同样，在将项目发布到 NPM 注册表时，这非常重要，因为许可证可能会限制某些开发人员或组织对软件的使用。拥有清晰的许可证有助于明确定义该软件可以使用的术语。

  `license` 字段的值通常是许可证的标识符代码——例如 **MIT** 或 **ISC** 之类的字符串，它们代表[MIT](https://opensource.org/licenses/MIT) 许可证和 [ISC](https://opensource.org/licenses/ISC) 许可证。如果你不想提供许可证，或者明确不想授予使用私有或未发布的软件包的权限，则可以将 `UNLICENSED` 作为许可证。如果你不确定要使用哪个许可证， [Choose a License](https://choosealicense.com/) 是对你有用的资源。
- **author 和 contributors**
  `author` 和 `contributors` 字段的功能类似。它们都是 people 字段，可以是"Name" 格式的字符串，也可以是具有 **name，email，url** 字段的对象。email 和 url 都是可选的。

  *author 只供一个人使用，contributors 则可以由多个人组成。*
- **description**
  NPM 注册表将 `description` 字段用于发布的软件包，以在搜索结果中和 [npmjs.com](https://www.npmjs.com/) 网站上描述该软件包。

  当用户搜索 NPM 注册表时，该字符串用于帮助了解软件包。这应该是软件包的简短摘要。

  即使你没有将其发布到 NPM 注册表中，它也可以用作项目的简单文档。
- **keywords**
  `keywords` 字段是一个字符串数组，其作用与描述相似。 NPM 注册表会为该字段建立索引，能够在有人搜索软件包时帮助找到它们。数组中的每个值都是与你的程序包关联的一个关键字。

  如果你不发布到 NPM 注册表，则这个字段用处不大，可以忽略它。
- **main**
  `main` 字段是 package.json 的功能属性。它定义了项目的入口点，通常是用于启动项目的文件。

  如果你的包（例如其名称为 foo-lib）是由用户安装的，则当用户执行 require('foo-lib') 时，这是 require 返回的 `main` 字段中所列出的文件的 module.exports 属性。
- **repository**
  你可以通过提供 `repository` 字段来记录项目代码所在的资源库。该字段是一个对象，用于定义源代码所在的 url 及其使用的版本控制系统的类型。对于开源项目，可能是以 Git 作为版本控制系统的 GitHub 或 Bitbucket 。

  *需要注意的是 URL 字段的本意是指向可从中访问版本控制的位置，而不仅仅是指向已发布的代码库。*
- **dependencies**
  这是 package.json 中最重要的字段之一，它列出了项目使用的所有依赖项（项目所依赖的外部代码）。使用 npm CLI 安装软件包时，它将下载到你的 node_modules/ 文件夹中，并将一个条目添加到你的依赖项属性中，注意软件包的名称和已安装的版本。
- **devDependencies**
  与 dependencies 字段类似，但是这里列出的包仅在开发期间需要，而在生产中不需要。
- **private**
  一般公司的非开源项目，都会设置 private 属性的值为 true，这是因为 npm 拒绝发布私有模块，通过设置该字段可以防止私有模块被无意间发布出去
- **files**
  发布哪些文件到 npm 上

### package.json 中不常见字段

- **指定模块适用系统（os）**
  假如我们开发了一个模块，只能跑在 darwin 系统下，我们需要保证 windows 用户不会安装到该模块，从而避免发生不必要的错误。
  这时候，使用 `os` 属性则可以帮助我们实现以上的需求，该属性可以指定模块适用系统的系统，或者指定不能安装的系统黑名单（当在系统黑名单中的系统中安装模块则会报错）
  {% codeblock package.json lang:json %}
  "os" : [ "darwin", "linux" ] # 适用系统
  "os" : [ "!win32" ] # 黑名单
  {% endcodeblock %}
  {% raw %}<article class="message is-info"><div class="message-body">{% endraw %}
  Tips：在 node 环境下可以使用 `process.platform` 来判断操作系统。
  {% raw %}</div></article>{% endraw %}
- **指定模块适用 cpu 架构（cpu）**
  上面的 os 字段类似，我们可以用 `cpu` 字段更精准的限制用户安装环境：
  {% codeblock package.json lang:json %}
  "cpu" : [ "x64", "ia32" ] # 适用 cpu
  "cpu" : [ "!arm", "!mips" ] # 黑名单
  {% endcodeblock %}
  {% raw %}<article class="message is-info"><div class="message-body">{% endraw %}
  Tips：在 node 环境下可以使用 `process.arch` 来判断 cpu 架构。
  {% raw %}</div></article>{% endraw %}
- **指定项目 node 版本（engines）**
  有时候，新拉一个项目的时候，由于和其他开发使用的 node 版本不同，导致会出现很多奇奇怪怪的问题（如某些依赖安装报错、依赖安装完项目跑步起来等）。

  为了实现项目开箱即用的伟大理想，这时候可以使用 package.json 的 `engines` 字段来指定项目 node 版本

  {% codeblock package.json lang:json %}
  "engines": {
    "node": ">= 8.16.0"
  }
  {% endcodeblock %}

  **该字段也可以指定适用的 npm 版本**

  {% codeblock package.json lang:json %}
  "engines": {
    "npm": ">= 6.9.0"
  }
  {% endcodeblock %}

  {% raw %}<article class="message is-warning"><div class="message-body">{% endraw %}
  需要注意的是，`engines` 属性仅起到一个说明的作用，当用户版本不符合指定值时也不影响依赖的安装。
  {% raw %}</div></article>{% endraw %}
- **自定义命令（bin）**
  用过 vue-cli，create-react-app等脚手架的朋友们，不知道你们有没有好奇过，为什么安装这些脚手架后，就可以使用类似 vue create/create-react-app之类的命令，其实这和 package.json 中的 `bin` 字段有关。

  `bin` 字段用来指定各个内部命令对应的可执行文件的位置。当package.json 提供了 `bin` 字段后，即相当于做了一个命令名和本地文件名的映射。
  当用户安装带有 `bin` 字段的包时

  如果是全局安装，npm 将会使用符号链接把这些文件链接到/usr/local/node_modules/.bin/；
  如果是本地安装，会链接到./node_modules/.bin/。

  举个 🌰，如果要使用 my-app-cli 作为命令时，可以配置以下 `bin` 字段
  {% codeblock package.json lang:json %}
  "bin": {
    "my-app-cli": "./bin/cli.js"
  }
  {% endcodeblock %}

  上面代码指定，my-app-cli 命令对应的可执行文件为 `bin` 子目录下的 cli.js，因此在安装了 `my-app-cli` 包的项目中，就可以很方便地利用 npm执行脚本
  {% codeblock package.json lang:json %}
  "scripts": {
    start: 'node node_modules/.bin/my-app-cli'
  }
  {% endcodeblock %}

  咦，怎么看起来和 vue create/create-react-app之类的命令不太像？原因：

  当需要 node 环境时就需要加上 node 前缀
  如果加上 node 前缀，就需要指定 my-app-cli 的路径 -> node_modules/.bin，否则 node `my-app-cli` 会去查找当前路径下的 **my-app-cli.js**，这样肯定是不对。

  若要实现像 vue create/create-react-app之类的命令一样简便的方式，**则可以在上文提到的 bin 子目录下可执行文件cli.js 中的第一行写入以下命令**
  {% codeblock cli.js lang:js %}
  #!/usr/bin/env node
  {% endcodeblock %}
  这行命令的作用是告诉系统用 node 解析，这样命令就可以简写成 `my-app-cli` 了。
- **peerDependencies**
  packageA packageB 同时依赖 vue 时，使用 `peerDependencies` 就可以很好的管理依赖，如果我们使用 `dependencies` 依赖树就会是这个样子
  ```
  .
  ├── package
  │   └── node_modules
  │       ├── vue
  │       ├── packageA
  │       │   └── nodule_modules
  │       │       └── vue
  │       └── packageB
  │       │   └── nodule_modules
  │       │       └── vue
  ```
  而 `peerDependencies` 就可以避免类似的核心依赖库被重复下载的问题。

  如果在 packageA 和 packageB 的 package.json 中使用 `peerDependencies` 来声明核心依赖库，例如：
  {% codeblock packageA package.json lang:json %}
  {
    "peerDependencies": {
      "vue": "3.0.1"
    }
  }
  {% endcodeblock %}

  {% codeblock packageB package.json lang:json %}
  {
    "peerDependencies": {
      "vue": "3.0.1"
    }
  }
  {% endcodeblock %}

  {% codeblock package package.json lang:json %}
  {
    "dependencies": {
      "vue": "3.0.1"
    }
  }
  {% endcodeblock %}
  此时在主系统中执行 `$ npm install` 生成的依赖图就是这样的：

  ```
  .
  ├── package
  │   └── node_modules
  │       ├── vue
  │       ├── packageA
  │       └── packageB
  ```

## 其它资料
[用 husky 和 lint-staged 构建超溜的代码检查工作流](https://segmentfault.com/a/1190000009546913)

<br>

<article class="message message-immersive is-warning">
<div class="message-body">
<i class="fas fa-question-circle mr-2"></i>Something wrong with this article? 
Click <a href="https://github.com/blacklisten/nblogs/edit/site/source/_posts/2021/Npm-Yarn-Help.md">here</a> 
to submit your revision.
</div>
</article>

<a style="background-color:black;color:white;text-decoration:none;padding:4px 6px;font-size:12px;line-height:1.2;display:inline-block;border-radius:3px" href="https://www.vecteezy.com/free-vector/vector-landscape" target="_blank" rel="noopener noreferrer" title="Vector Landscape Vectors by Vecteezy"><span style="display:inline-block;padding:2px 3px"><svg xmlns="http://www.w3.org/2000/svg" style="height:12px;width:auto;position:relative;vertical-align:middle;top:-1px;fill:white" viewBox="0 0 32 32"><path d="M20.8 18.1c0 2.7-2.2 4.8-4.8 4.8s-4.8-2.1-4.8-4.8c0-2.7 2.2-4.8 4.8-4.8 2.7.1 4.8 2.2 4.8 4.8zm11.2-7.4v14.9c0 2.3-1.9 4.3-4.3 4.3h-23.4c-2.4 0-4.3-1.9-4.3-4.3v-15c0-2.3 1.9-4.3 4.3-4.3h3.7l.8-2.3c.4-1.1 1.7-2 2.9-2h8.6c1.2 0 2.5.9 2.9 2l.8 2.4h3.7c2.4 0 4.3 1.9 4.3 4.3zm-8.6 7.5c0-4.1-3.3-7.5-7.5-7.5-4.1 0-7.5 3.4-7.5 7.5s3.3 7.5 7.5 7.5c4.2-.1 7.5-3.4 7.5-7.5z"></path></svg></span><span style="display:inline-block;padding:2px 3px">Vector Landscape Vectors by Vecteezy</span></a>