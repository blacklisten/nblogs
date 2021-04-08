title: Mocha完成单元测试
date: 2021-03-25 15:26:06
tags:
- 单元测试
categories:
- Front
language: zh-CN
toc: true
providers:
    cdn: loli
    fontcdn: loli
    iconcdn: loli
cover: /gallery/covers/23.jpg
thumbnail: /gallery/covers/23.jpg
---

使用 mocha.js chai.js 完成对工具库的单元测试

<!-- more -->

## 问题记录

### tsconfig.json module 设置为 esnext 后，但愿测试报错，import 不可使用，在不修改为 commonjs 的情况下怎么解决这个问题呢？

网上找了很多方法，使用 babel 进行转换，基本上都是采用 babel-register 插件去做 [让Mocha支持es6语法](https://greenfavo.github.io/blog/docs/02.html)

但最后发现使用这个方法是不支持 typescript 的，于是继续寻找可使用方法在 [Trying to use mocha, ES6 modules, and ts-node with the --experimental-loader option](https://stackoverflow.com/questions/65376414/trying-to-use-mocha-es6-modules-and-ts-node-with-the-experimental-loader-opt) 的一条回答中找到了解决方案 
{% codeblock packageA package.json lang:json %}
  {
    "scripts": {
      "test": ""TS_NODE_COMPILER_OPTIONS='{\"module\":\"commonjs\"}' nyc --reporter=html mocha --require ts-node/register src/**/*.spec.ts""
    }
  }
{% endcodeblock %}
配置 mocha 测试环境下 TS_NODE_COMPILER_OPTIONS 的module 为 commonjs 即可，既不用更改 tsconfig.json 又可以直接使用 ts-node/register 进行单元测试

<br>

<article class="message message-immersive is-warning">
<div class="message-body">
<i class="fas fa-question-circle mr-2"></i>Something wrong with this article? 
Click <a href="https://github.com/blacklisten/nblogs/edit/site/source/_posts/2021/Mocha.md">here</a> 
to submit your revision.
</div>
</article>

<a style="background-color:black;color:white;text-decoration:none;padding:4px 6px;font-size:12px;line-height:1.2;display:inline-block;border-radius:3px" href="https://www.vecteezy.com/free-vector/vector-landscape" target="_blank" rel="noopener noreferrer" title="Vector Landscape Vectors by Vecteezy"><span style="display:inline-block;padding:2px 3px"><svg xmlns="http://www.w3.org/2000/svg" style="height:12px;width:auto;position:relative;vertical-align:middle;top:-1px;fill:white" viewBox="0 0 32 32"><path d="M20.8 18.1c0 2.7-2.2 4.8-4.8 4.8s-4.8-2.1-4.8-4.8c0-2.7 2.2-4.8 4.8-4.8 2.7.1 4.8 2.2 4.8 4.8zm11.2-7.4v14.9c0 2.3-1.9 4.3-4.3 4.3h-23.4c-2.4 0-4.3-1.9-4.3-4.3v-15c0-2.3 1.9-4.3 4.3-4.3h3.7l.8-2.3c.4-1.1 1.7-2 2.9-2h8.6c1.2 0 2.5.9 2.9 2l.8 2.4h3.7c2.4 0 4.3 1.9 4.3 4.3zm-8.6 7.5c0-4.1-3.3-7.5-7.5-7.5-4.1 0-7.5 3.4-7.5 7.5s3.3 7.5 7.5 7.5c4.2-.1 7.5-3.4 7.5-7.5z"></path></svg></span><span style="display:inline-block;padding:2px 3px">Vector Landscape Vectors by Vecteezy</span></a>
