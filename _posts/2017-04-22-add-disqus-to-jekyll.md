---
layout: post
comments: true
title:  "添加 Disqus 评价系统到 Jekyll"
categories: Jekyll
tags: jekyll disqus
author: booox
date: 2017/04/22 09:10:24
---

* content
{:toc}

 如何让 Jekyll 允许用户评论？




Jekyll 是纯文本的博客，其本身是没办法让用户对博文进行评论的。幸好有第三方的评论系统提供了相关的支持。Disqus 可以算是评论界的老大了。国内本来有「多说」，但可惜的是，官方已经宣布马上就关闭了。用 Disqus 有一点不方便的是，可能需要 FQ。

## 1. 注册 Disqus

到 [Disqus.com](https://disqus.com/) 上注册好账户，设置好站点，就可以拿到相关的代码去用了。

## 2. 设置 Jekyll

### 2.1 创建 disqus.html

创建 *_includes/disqus.html* 文件，添加如下代码：

```
{% if page.comments %}
<div id="disqus_thread"></div>
<script>
var disqus_config = function () {
this.page.url = "https://booox.github.io{{ page.url }}"; // <--- 修改成你的博客地址
this.page.identifier = "{{ page.id }}";
};
(function() { // DON'T EDIT BELOW THIS LINE
var d = document, s = d.createElement('script');

s.src = '// booox.disqus.com/embed.js'; // <--- 修改成你的 disqus 站点缩写名

s.setAttribute('data-timestamp', +new Date());
(d.head || d.body).appendChild(s);
})();
</script>
<noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript" rel="nofollow">comments powered by Disqus.</a></noscript>
{% endif %}

```

### 2.2 修改 post 模板

修改 *_layouts/post.html* ，添加如下代码到你想放置评论的位置：

`{% include disqus.html %}`

### 2.3 在博文页面启用评论

在需要启用评论的博文页面的 [YAML front matter](http://jekyllrb.com/docs/frontmatter/) 位置添加如下代码来启用评论：

`comments: true`

下面是一个示例：

```
---
layout: post
title:  "添加 Disqus 评价系统到 Jekyll"
categories: Jekyll
tags: jekyll, disqus
comments: true
date: 2017/04/22 09:10:24
---
```

### Links

* [Adding Disqus to a Jekyll Blog](http://sgeos.github.io/jekyll/disqus/2016/02/14/adding-disqus-to-a-jekyll-blog.html)
* [Disqus](https://disqus.com/)
