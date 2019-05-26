---
layout: post
title:  "github pages 博客初长成"
date:   2019-05-26 10:36:00
categories: jekyll default
tags: featured
image:
---
好早之前想弄过博客，用过wordpress，也想过自己写，可是基本上都是到一半感觉耗费太多精力，然后就放弃了，真是应了从入门到放弃。
周末在家无聊，研究了一下Github Pages功能，搜索到了可以和jekyll配合，然后尝试了一下感觉还可以，只是目前使用的这个[dirkfabisch/mediator](https://github.com/dirkfabisch/mediator)模版加载得好像有点慢。

然后尝试着写下第一篇blog，下面尝试各种语法高亮支持

{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

{% highlight js %}

<footer class="site-footer">
 <a class="subscribe" href="{{ "/feed.xml" | prepend: site.baseurl }}"> <span class="tooltip"> <i class="fa fa-rss"></i> Subscribe!</span></a>
  <div class="inner">a
   <section class="copyright">All content copyright <a href="mailto:{{ site.email}}">{{ site.name }}</a> &copy; {{ site.time | date: '%Y' }} &bull; All rights reserved.</section>
   <section class="poweredby">Made with <a href="http://jekyllrb.com"> Jekyll</a></section>
  </div>
</footer>
{% endhighlight %}


[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
