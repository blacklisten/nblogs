title: 浅谈单点登录
date: 2021-02-25 14:26:06
tags:
- 服务端
categories:
- Learning
language: zh-CN
toc: true
providers:
    cdn: loli
    fontcdn: loli
    iconcdn: loli
cover: /gallery/covers/guimiezhiren3.jpg
thumbnail: /gallery/covers/guimiezhiren3.jpg
---

单点登录：用户只需要登录一次，就可以访问多个系统，避免频繁授权
单系统登陆功能主要是用 `Session` 保存用户信息来实现，但我们清楚的是：多系统可能有多个 **Tomcat**，而 `Session` 是依赖当前的 **Tomcat**，所以 A 的 `Session` 和 B 的 `Session` 是不共享的。

<!-- more -->

## Session 不共享解决方案

解决系统之间 `Session` 不共享问题有以下几个方案：

1. Tomcat集群 `Session` 全局复制（集群内每个 **tomcat** 的 `session` 完全同步）【会影响集群的性能，不建议】
2. 根据请求的 **IP** 进行 **Hash** 映射到对应的服务器上（这就相当于请求的 **IP** 会一直访问同一个服务器）【如果服务器宕机，会丢失一大部分 `Session` 的数据，不建议】
3. 把 `Session` 数据存放到 `Redis` 中（使用 `Redis` 模拟 `Session`）【建议】

结论：

1. SSO系统生成一个 **Token**，并将用户信息存在 `Redis` 中，并设置过期时间
2. 其它系统请求 SSO 系统进行登录，得到 SSO 返回的 **Token**，写到 `Cookie` 中
3. 每次请求时，`Cookie` 都会带上，拦截器得到 **Token**，判断是否已经登录

到这里，其实我们会发现就两个变化

1. 将登录功能抽取未一个系统（SSO），其它系统请求 SSO 进行登录
2. 本来将用户信息存储到 `Session`，现在将用户信息存储到 `Redis`

但我们会发现，如果我们请求的是 [https://www.google.com](https://www.google.com) 时，浏览器会自动把 google.com 的 `Cookie` 带过去给 google 服务器，而不会把 [https://www.baidu.com](https://www.baidu.com) 的 `Cookie` 带过去给 google 服务器。

这意味着，由于域名不同，用户登录系统 A 后，系统 A 返回给浏览器的 `Cookie`，用户再请求系统 B 的时候不会讲系统 A 的 `Cookie` 带过去。

## Cookie 存在跨域解决方案

针对 `Cookie` 存在跨域问题，有几种解决方案：

1. 服务端讲 `Cookie` 写到客户端后，客户端对 `Coolie` 进行解析，讲 **Token** 解析出来，此后请求都把这个 **Token** 带上即可
2. 多个域名共享 `Cookie`，在写到客户端的时候设置 `Coolie` 的 **domain**
3. 讲 **Token** 保存在 `SessionStrong` 中（不依赖 `Cookie` 就没有跨域问题了）

## 参考

[什么是单点登录(SSO)](https://juejin.cn/post/6844903845424971783)
[阿里P8架构师谈：单点登录的原理、来源、实现、以及技术方案比较](https://youzhixueyuan.com/the-principle-implementation-and-technical-scheme-of-single-sign-on.html)
[JSON Web Token 入门教程](http://www.ruanyifeng.com/blog/2018/07/json_web_token-tutorial.html)

<br>

<article class="message message-immersive is-warning">
<div class="message-body">
<i class="fas fa-question-circle mr-2"></i>Something wrong with this article? 
Click <a href="https://github.com/blacklisten/nblogs/edit/site/source/_posts/2021/Single-Sign-On.md">here</a> 
to submit your revision.
</div>
</article>

<a style="background-color:black;color:white;text-decoration:none;padding:4px 6px;font-size:12px;line-height:1.2;display:inline-block;border-radius:3px" href="https://www.vecteezy.com/free-vector/vector-landscape" target="_blank" rel="noopener noreferrer" title="Vector Landscape Vectors by Vecteezy"><span style="display:inline-block;padding:2px 3px"><svg xmlns="http://www.w3.org/2000/svg" style="height:12px;width:auto;position:relative;vertical-align:middle;top:-1px;fill:white" viewBox="0 0 32 32"><path d="M20.8 18.1c0 2.7-2.2 4.8-4.8 4.8s-4.8-2.1-4.8-4.8c0-2.7 2.2-4.8 4.8-4.8 2.7.1 4.8 2.2 4.8 4.8zm11.2-7.4v14.9c0 2.3-1.9 4.3-4.3 4.3h-23.4c-2.4 0-4.3-1.9-4.3-4.3v-15c0-2.3 1.9-4.3 4.3-4.3h3.7l.8-2.3c.4-1.1 1.7-2 2.9-2h8.6c1.2 0 2.5.9 2.9 2l.8 2.4h3.7c2.4 0 4.3 1.9 4.3 4.3zm-8.6 7.5c0-4.1-3.3-7.5-7.5-7.5-4.1 0-7.5 3.4-7.5 7.5s3.3 7.5 7.5 7.5c4.2-.1 7.5-3.4 7.5-7.5z"></path></svg></span><span style="display:inline-block;padding:2px 3px">Vector Landscape Vectors by Vecteezy</span></a>