---
layout: post
title: "uclcmd コマンドをインストールする"
category: Programming
tags: [UCL, Config]
---
{% include JB/setup %}

UCLファイルの検証として、[UCLCMD](https://github.com/allanjude/uclcmd)を使おうと思い、参考リンクの手順でインストールしようと思ったら、いらんところで詰まったので、対処したことを書いておきます。

## 副産物

- [boldowa/libucl: Universal configuration library parser](https://github.com/boldowa/libucl)
- [boldowa/uclcmd: Command line tool for working with UCL config files](https://github.com/boldowa/uclcmd)

ArchLinux上でしか確認してないので、折を見てBSD上で確認してみたいところ。

確認とれたらプルリク投げたいと思っています。

### libucl のCMakeLists.txt の修正

libuclには、CMakeLists.txtが付属しているものの、インストール機能を備えていません。

そのため、CMakeLists.txtを修正し、インストール機能を追加しました。


### uclcmd の修正

uclcmd には、そもそもCMakeListsが付属していません。

加えて、BSD用(?)になっているっぽく、付属のMakefileでmakeを実行しても、コンパイルエラーおよびリンクエラーが出る為、ビルドが通りません。

生のMakefileをもうしばらく書いていないので、とりあえずCMakeLists.txtを書いて対応することにしました。

また、uclcmd.hに、GCC用の定義を追加し、コンパイルが通るようにしました。


## 対処後のインストール手順

### 1. libuclのインストール

1. libucl リポジトリをcloneしてくる
2. てきとうな場所でlibuclをcmakeしてmakeする
3. `sudo make install`でlibuclをシステムインストールする

### 2. uclcmdのインストール

1. uclcmd リポジトリをcloneしてくる
2. てきとうな場所でuclcmdをcmakeしてmakeする
3. `sudo make install`でuclcmdをシステムインストールする



## その他

pcファイルとかはよくわからんので、zlibのやつを参考に作りました。


---


## 参考リンク

- [次世代設定ファイルフォーマット UCL \(Universal Configuration... \| CUBE SUGAR STORAGE](http://momijiame.tumblr.com/post/125659650081/%E6%AC%A1%E4%B8%96%E4%BB%A3%E8%A8%AD%E5%AE%9A%E3%83%95%E3%82%A1%E3%82%A4%E3%83%AB%E3%83%95%E3%82%A9%E3%83%BC%E3%83%9E%E3%83%83%E3%83%88-ucl-universal-configuration)

