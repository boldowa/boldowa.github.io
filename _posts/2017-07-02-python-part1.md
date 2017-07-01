---
layout: post
title: "スクリプト言語 Python(1)"
category: "Python"
tags: [Python, HelloWorld]
comments: true
---
{% include JB/setup %}

**この地上に落とされた最速さいつよ言語 - Python -**  ～ 多分これが一番早いと思います ～

というわけで、Pythonをつまんでみます。

Pythonはスクリプト言語ですが、Numpy等の強力なライブラリを備えており、スクリプトの組み方によってはスクリプト言語とはおもえないパフォーマンスを発揮するプログラムが作れるようです。

というわけで、れっつとらい！！

## インストール

私はArch Linuxを使っているので、pacmanを使ってインストールします。

```
# pacman -S python3
```

## はろーわーるど

### Hello World.py

```python
#
# Hello World
#

print("Hello, World!")
```

### 実行

```
$ python Hello\ World.py
Hello, World!
```

シンプル。

上から順に実行されます。

