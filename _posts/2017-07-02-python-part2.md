---
layout: post
title: "スクリプト言語 Python(2)"
category: "Python"
tags: [Python, Args]
comments: true
---
{% include JB/setup %}

今回は、プログラムの引数を取得してみます。

## あーぎゅめんと

### Argument.py

```python
#
# Argument
#

# argv 取得に必要なモジュールをインポート
import sys

print("Hello, {0}!".format(sys.argv[1]))
```

### 実行

```
$ python Argument.py Takeshi
Hello, Takeshi!
```

プログラム引数は、**sys**モジュールをインポートすることで使用可能になります。

モジュールとは、部品のことです。

みんなだいすきレゴブロックでいうなれば、プロペラ付きブロックみたいなものです。

プロペラ付きブロックを付けることで、「プロペラを回転させる」機能が追加されるように、sysモジュールをインポートすることで様々な機能を使用できます。

プログラム引数は、sys.argv に格納されており、これを参照することで取得可能です。
