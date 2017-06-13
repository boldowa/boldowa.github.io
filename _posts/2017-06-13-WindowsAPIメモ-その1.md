---
layout: post
category : WindowsAPI
tags : [WindowsAPI, CreateProcess, C言語]
comments: true
---
{% include JB/setup %}

このシリーズは、WindowsAPIを使用してハマりそう/ハマった内容について記述していく備考録です。

(ライブラリ豊富な今、WindowsAPIを直接叩く機会があるのかどうかは不明ですが…)

不定期特集第1回目は、外部プロセスを起動する**CreateProcess** APIです。

## CreateProcess の基本情報

[MSDN](https://msdn.microsoft.com/ja-jp/library/cc429066.aspx)より

```c
BOOL CreateProcess(
  LPCTSTR lpApplicationName,                 // 実行可能モジュールの名前
  LPTSTR lpCommandLine,                      // コマンドラインの文字列
  LPSECURITY_ATTRIBUTES lpProcessAttributes, // セキュリティ記述子
  LPSECURITY_ATTRIBUTES lpThreadAttributes,  // セキュリティ記述子
  BOOL bInheritHandles,                      // ハンドルの継承オプション
  DWORD dwCreationFlags,                     // 作成のフラグ
  LPVOID lpEnvironment,                      // 新しい環境ブロック
  LPCTSTR lpCurrentDirectory,                // カレントディレクトリの名前
  LPSTARTUPINFO lpStartupInfo,               // スタートアップ情報
  LPPROCESS_INFORMATION lpProcessInformation // プロセス情報
);
```

### ポイント

- lpProcessAttributes は、**NULL**を指定すると規定のセキュリティ設定を使う(WindowsNT/2000以降)
- lpCurrentDirectory は、**NULL**を指定すると呼び出し側プロセスと同じディレクトリが使われる
- この関数は、プロセスの**生成**に成功または失敗するとすぐに制御を返す。呼び出したプロセスの終了を待つ場合は、別途[WaitForSingleObject](https://msdn.microsoft.com/ja-jp/library/cc429427.aspx)APIを使う必要がある。

そのままコールするとなると、実行の度に各変数の初期化処理を書く必要があり、面倒です。  
ラッパ関数を用意するのが良いと思います。


## ハマりポイント

### 第2引数の要素にスペースがある場合は気を付けよう

わりと基本的なことですが、例えば引数の１つになんかしらのパスを指定する場合は、スペースが含まれるケースをちゃんと考えないといけません。

たとえば、次のようなケースです。

```c
STARTUPINFO si;
PROCESS_INFORMATION pi;
const TCHAR * exePath = _T("Executable program.exe");
const TCHAR * anyPath = _T(" C:\\Documents and Settings\\hoge\\file.txt");

ZeroMemory(&pi, sizeof(pi));
ZeroMemory(&si, sizeof(si));
si.cb = sizeof(si);

CreateProcess(exePath, anyPath,
		NULL, NULL,
		TRUE, NORMAL_PRIORITY_CLASS,
		NULL, NULL, 
		&si, &pi);
```

このケースの場合、以下のように解釈されてしまいます。

- 第1引数(実行ファイル) : Executable program.exe
- 第2引数 : C:\Documents
- 第3引数 : and
- 第4引数 : Settings\file,txt

正しい解釈をさせたい場合は、パスをダブルクォーテーションで囲むのが一般的です。

anyPathの指定を以下のように訂正します。

```c
const TCHAR * anyPath = _T(" \"C:\\Documents and Settings\\hoge\\file.txt\"");
```

訂正後の引数解釈は、以下になります。

- 第1引数(実行ファイル) : Executable program.exe
- 第2引数 : C:\Documents and Settings\file.txt

この他に、全てのスペースの前にエスケープ文字を入れるという方法もありますが、面倒なのでここでは触れません。

### 第2引数の先頭には、スペースを挿入しよう

上の例で普通に実践していますが、第2引数でコマンドライン引数を指定する場合、引数の先頭にスペースを挿入することを推奨します。

つまり、第2引数の値として、以下のような設定は避けるべきです。

```c
const TCHAR * anyPath = _T("\"C:\\Documents and Settings\\hoge\\file.txt\"");
```

先頭にスペースが無い場合、第2引数の先頭要素の解釈が不定となります。

これはOSにより多少異なるようで、
Windows8.1 の場合、プロセスを普通に起動できる場合もあれば、起動できない場合もあります。(私の場合は**GetOpenFileName**APIをコールし、IDOK処理をした後は起動できなくなりました。)  
WindowsXP の場合、プロセスの起動は普通にできるものの、コマンドライン引数の1番目の指定が反映されないような動作になるようです。

コマンドライン引数を確実に処理してもたいたい場合は、先頭にスペースを入れましょう。

ただし、これには例外もあり、第1引数(実行ファイルパス)をNULLにして、第2引数に実行ファイルパスを含める場合は、先頭のスペースは無くても問題ないようです。  
ただし、実行ファイル名にスペースが含まれる場合は、ダブルクォーテーションで囲む必要があります。

