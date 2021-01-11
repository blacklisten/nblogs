title: Macos M1芯片尝鲜
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

Macos M1芯片，记录使用过程中的各种问题...

<!-- more -->

## vscode问题

直接从vscode官网安装了...发现不是很适配，最终通过twitter找到了适配版本[arm版本的vscode](https://twitter.com/code/status/1338886895867224070)

arm版本的vscode没办法进行配置同步，虽然一直在同步中，但是同步不过来，插件、设置都需要重新弄了，唉...😔

## 如何解决curl: (7) Failed to connect to raw.githubusercontent.com port 443: Connection refused

[解决方法](https://github.com/hawtim/blog/issues/10) **switchhosts是真的好用**:+1:


## homebrew问题...

这个真的是有些恶心了... 网上找了各种帖子，各种尝试，都没有成功，最终通过youtube上大佬发布的[视频](https://www.youtube.com/watch?v=nv2ylxro7rM)成功解决。
使用大佬[托管在gist上的shell脚本](https://gist.github.com/nrubin29/bea5aa83e8dfa91370fe83b62dad6dfa)成功安装了homebrew😆
