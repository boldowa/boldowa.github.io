---
layout: post
title: "スクリプト言語 Python(3)"
category: "Python"
tags: [Python, Web, Tornado]
comments: true
---
{% include JB/setup %}

こまかいことはすっとばして、PythonのWebフレームワークを使ってみます。

Django, Bottle, Flask等のフレームワークがありますが、今回は**Tornado**というフレームワークを使ってみます。

## インストール

てっとりばやくpipを使います。

その他の方法は[ググりましょう](https://www.google.co.jp/search?q=python+tornado+%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB)。

```
# pip install tornado
```

## Hello World

[このページ](https://sites.google.com/site/tornadowebja/documentation/overview)を参考、もといパクります。

### serv.py

```python
#
# serv
#

import tornado.ioloop
import tornado.web

class MainHandler(tornado.web.RequestHandler):
    def get(self):
        self.write("Hello, World!")

app = tornado.web.Application([
    (r"/", MainHandler),
])

if __name__ == "__main__":
    app.listen(8888)
    tornado.ioloop.IOLoop.instance().start()
```

### 実行

```
$ python serv.py

```

ブラウザでアクセスすれば、"Hello, World!"の文字列が返ってくることがわかります。

![Tornado Hello World]({{site.baseurl}}/images/python/tornado_hello_world.png)

