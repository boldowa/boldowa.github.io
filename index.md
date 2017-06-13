---
layout: page
title: Junk Warehouse
tagline: とっぷぺーじ
---
{% include JB/setup %}

<div style="float: right; text-align: right;">
  <a href="rss.xml" title="あーるえすえす ふぃーど">RSS Feed</a> | <a href="rss.xml" title="えーてぃーおーえむ ふぃーど">Atom Feed</a>
</div>

ローテクPGの役に立たないあぷりけぃしょんや、なぐりがきのぺーじです。

## せいさくぶつ

### [SuperC-SPCdrv](http://github.com/boldowa/SuperC-SPCdrv)

SPCドライバ / 専用MMLコンパイラ

### [Various-Script700](http://github.com/boldowa/Various-Script700)

実験的に作ったscript700のライブラリ


## うぇぶろぐ

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>

## そぉしゃる あかうんと

- [ついったぁ](http://twitter.com/{{site.author.twitter}})
- [ぎっとはぶ](https://github.com/{{site.author.github}})


## むかしのぺーじ

### [備考録なんです？](http://d.hatena.jp/boldowa/)

はてなのダイアリー

FLMML使えたからはてダ選んだけど、全然使ってませんでした。

