---
layout: post
title: "libuclを使ってみよう(ファイル読み込み編)"
category: Programming
tags: [UCL, Config]
---
{% include JB/setup %}

UCLファイルのパーサ・エミッタの、C言語による実装として、libuclというライブラリがあります。

ここでは、そのライブラリの使い方の一例を紹介しようと思います。

この記事では、ファイルの読み込みを取り扱います。

## コード例

<script src="https://gist.github.com/boldowa/a5e17c9cb21e465eb5dba0b730fc12f9.js"></script>



パラメータ読み込みまでの簡易的な手順は、

1. parserオブジェクトの作成
2. parserへのファイル割り当て
3. 入力したuclのエラーチェック
4. parserオブジェクトから、ucl\_objectを取得
5. ucl\_objectから値を検索し、読み出す

となります。


### 解説

#### parserオブジェクトの作成

ソースコードの46行目がこれに該当します。

'''c
	parser = ucl_parser_new(UCL_PARSER_DEFAULT);
'''

**ucl_parser_new**関数は、引数にフラグパラメータをとり、これによりパーサの挙動を変化させることができます。


#### parserへのファイル割り当て

ソースコードの130行目がこれに該当します。

'''c
	if(!ucl_parser_add_file(parser, path))
'''

**ucl_parser_add_file**関数を使うことで、uclファイルをパーサに追加できます。

追加なので、複数回使うと、実行回数分だけパーサにuclファイルを追加(くっつける)ことができるとおもわれます。
(たぶん)


#### 入力したuclのエラーチェック

ソースコードの138行目がこれに該当します。

'''c
	uclerr = ucl_parser_get_error(parser);
'''

**ucl_parser_get_error**関数を使うことで、uclの構文チェックを行うことができます。

正常にパースできないようであれば、エラー内容を示す文字列が返ってきます。

おそらく、これは必須ではありませんが、やっておいたほうが無難であると思います。



#### parserオブジェクトから、ucl\_objectを取得

ソースコードの147行目がこれに該当します。

'''c
	uclobj = ucl_parser_get_object(parser);
'''

**ucl_parser_get_object**関数を使うことで、uclオブジェクトを取得できます。

このオブジェクトに対し、読み出し関数を使用して値を読み出していきます。


#### ucl\_objectから値を検索し、読み出す

ソースコードの64行目からの処理がこれに該当します。

```c
	tgtobj = ucl_object_lookup_path(rootobj, test_key);
	if(!tgtobj)
	{
		fprintf(stderr, "Error: %s: key \"%s\" not found.\n", path, test_key);
		ucl_object_unref(rootobj);
		ucl_parser_free(parser);
		return -1;
	}
	result = ucl_object_toint_safe(tgtobj,&test_value);
	if(result)
	{
			printf("%s: %s = %ld\n", path, test_key, test_value);
	}
	else
	{
		fprintf(stderr, "Error: %s: key \"%s\" isn't int value.\n", path, test_key);
	}
```

ポイントは、**ucl_object_lookup_path**関数と、**ucl_object_toint_safe**関数で、  
前者が値の検索、後者が値の取り出しを行っています。

検索関数・値の取り出し関数は他にも様々なものがあるので、**ucl.h**の中身を覗いてみてください。

複数のオブジェクトに対して処理を繰り返す みたいな処理は、たぶん**ucl_object_iterate**関数あたりをつかえばできそうです。


### コード例の使用例

**main.c**
```c
#include <stdio.h>
#include "load/load.h"

int main(int argc, char** argv)
{
	return ucl_fload("test.ucl");
}
```

こんな感じで。

以下のファイルを読み込むと、

**test.ucl**
```
{
	hogehoge {
		fugafuga : 30
	}
}
```

出力結果はこんな感じ。

```
test.ucl: hogehoge.fugafuga = 30
```


プロジェクトのサンプルを、[ここ](https://github.com/boldowa/ucltest)に置いておきます。




## おわりに

libuclの使い方のキホンは分かったので、これからどんどん使っていきたいと思います。





---

## 関連記事

- [UCL(Universal Config Language)ファイルについて](2018-05-27-ucl)

## 参考リンク

- [UCL All of the Things (MeetBSD California 2014 Lightning Talk)](https://www.slideshare.net/iXsystems/ucl-all-of-the-things-meetbsd-california-2014-lightning-talk)

