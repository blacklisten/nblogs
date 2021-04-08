title: Macos 使用问题记录以及一些简单的技巧
date: 2021-01-11 13:55:06
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
cover: /gallery/covers/wallhaven-7232p9.jpg
thumbnail: /gallery/covers/wallhaven-7232p9.jpg
---

因为我之前并没有使用过 Mac 系统，也从未使用过 Linux 系统(上学的时候玩过)，然后自己有买了最新款的 Macos M1 芯片，故此记录使用过程中的各种问题...

<!-- more -->

## M1 使用问题纪要

### vscode 问题

~~直接从 vscode 官网安装了...发现不是很适配，最终通过 twitter 找到了适配版本[arm 版本的 vscode](https://twitter.com/code/status/1338886895867224070)~~

~~arm 版本的 vscode 没办法进行配置同步，虽然一直在同步中，但是同步不过来，插件、设置都需要重新弄了，唉...😔~~

在 2021-02 的时候 vscode 终于宣布在正式版本中支持 arm 处理器了🌈 [链接](https://code.visualstudio.com/updates/v1_54)

### 如何解决 curl :(7) Failed to connect to raw.githubusercontent.com port 443: Connection refused

[解决方法](https://github.com/hawtim/blog/issues/10) **switchhosts 是真的好用**

### homebrew 问题...

这个真的是有些恶心了...网上找了各种帖子，各种尝试，都没有成功，最终通过 youtube 上大佬发布的[视频](https://www.youtube.com/watch?v=nv2ylxro7rM)成功解决。
使用大佬[托管在 gist 上的 shell 脚本](https://gist.github.com/nrubin29/bea5aa83e8dfa91370fe83b62dad6dfa)成功安装了 homebrew😆

### nvm 的问题

就目前来讲...nvm 似乎还没完全适配 M1，安装之后 server install 各种慢，真的是有些受不了...[nvm install node fails to install on macOS Big Sur M1 Chip](https://github.com/nvm-sh/nvm/issues/2350)。卸载 nvm 后，直接从官方下载 [nodejs](https://nodejs.org/zh-cn/) ，然后用 **Rosetta** 跑起来完美，server install 都比之前快了不止一个档次。

## 访问 github 443

错误：<p style="color: red">Failed to connect to github.com port 443: Operation timed out</p>

解决方案：

- 去 [ipaddress.com](https://ipaddress.com) 上查找 github.com 对应的 IP 
- 修改 `hosts` 文件
  ```bash
    vim /etc/hosts
  ```
  追加上 github.com 对应的 IP
- 刷新 DNS，在终端输入（需要权限）
  ```bash
    sudo killall -HUP mDNSResponder;say DNS cache has been flushed
  ```

## oh my zsh

**安装 oh my zsh**

使用curl安装

{% codeblock "bash zsh" %}
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
{% endcodeblock %}

**配置主题**

oh-my-zsh 提供了很多主题，可以通过命令查看当前使用的主题：

{% codeblock "bash zsh" %}
ls ~/.oh-my-zsh/themes
{% endcodeblock %}

可以通过编辑 .zshrc 文件来修改你的主题

{% codeblock "bash zsh" %}
vim ~/.zshrc
{% endcodeblock %}

{% codeblock "bash zsh" %}
source ~/.zshrc
{% endcodeblock %}

**zsh-autosuggestions 自动补全插件**

[zsh-autosuggestions INSTALL](https://github.com/zsh-users/zsh-autosuggestions/blob/master/INSTALL.md#oh-my-zsh)

`重启终端即可`

## 一些快捷键

- 「⌃ + ⌘ + ␣」（control + command + space） 打开 emoji 表情

## 终端的一些基础命令

- mv ~/xx ~/yy 将 xx（文件、目录）移动到 yy 下面 这对我从vscode-insider 版本换会正式版本的vscode起了很大作用(不用重复安装插件了，直接移动过去🤣)
- cp -R 源文件 目标文件 **-R是对目录进行递归操作**

<br>

<article class="message message-immersive is-warning">
<div class="message-body">
<i class="fas fa-question-circle mr-2"></i>Something wrong with this article? 
Click <a href="https://github.com/blacklisten/nblogs/edit/site/source/_posts/2021/Macos.md">here</a> 
to submit your revision.
</div>
</article>

<a style="background-color:black;color:white;text-decoration:none;padding:4px 6px;font-size:12px;line-height:1.2;display:inline-block;border-radius:3px" href="https://wallhaven.cc" target="_blank" rel="noopener noreferrer" title="Vector Landscape Vectors by Vecteezy"><span style="display:inline-block;padding:2px 3px"><svg xmlns="http://www.w3.org/2000/svg" style="height:12px;width:auto;position:relative;vertical-align:middle;top:-1px;fill:white" viewBox="0 0 32 32"><path d="M20.8 18.1c0 2.7-2.2 4.8-4.8 4.8s-4.8-2.1-4.8-4.8c0-2.7 2.2-4.8 4.8-4.8 2.7.1 4.8 2.2 4.8 4.8zm11.2-7.4v14.9c0 2.3-1.9 4.3-4.3 4.3h-23.4c-2.4 0-4.3-1.9-4.3-4.3v-15c0-2.3 1.9-4.3 4.3-4.3h3.7l.8-2.3c.4-1.1 1.7-2 2.9-2h8.6c1.2 0 2.5.9 2.9 2l.8 2.4h3.7c2.4 0 4.3 1.9 4.3 4.3zm-8.6 7.5c0-4.1-3.3-7.5-7.5-7.5-4.1 0-7.5 3.4-7.5 7.5s3.3 7.5 7.5 7.5c4.2-.1 7.5-3.4 7.5-7.5z"></path></svg></span><span style="display:inline-block;padding:2px 3px">Vector Landscape Vectors by Vecteezy</span></a>
