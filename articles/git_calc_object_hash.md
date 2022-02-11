---
title: 'Gitのオブジェクトハッシュの計算方法'
emoji: "⛳"
type: "tech"
topics: ["git"]
published: true
---

## Git のオブジェクト

最初に少しだけGitのデータ(オブジェクト)の保存方法について触れます。
オブジェクトは `.git/objects/` に保存されます。
この中にリポジトリのファイルを効率的に保存します。
バージョンごとに同じファイルを重複して保存することはありません。
ファイルだけではありません。４種類のオブジェクトを保存します。

1. blob: Binary Large OBjecctの略で、つまりはファイルです。しかし、ファイル名は含みません。
2. tree: １階層分のディレクトリの全ファイル名とファイル内容のハッシュと権限です。
3. commit: `git commit` で作られます。コミット内容とか作成者、作成日時などが含まれます。コミットハッシュやコミットSHAと呼ばれるものです。
4. tag: `git tag` で作られます。本質的には `commit` とあまり変わりません。

なぜ、重複せずに保存できるのか？
重要なのは、ハッシュ値により同じファイルかどうかを判断しているからです。
`blob` はファイル名を含みません。これは、ファイル名が違っても同じ内容ならば１つのオブジェクトしか作らないことを意味します。
`tree` も同様です。同じ全くディレクトリならば、重複して作られません。

この記事では、Gitは触れるけどもう少し詳しく知りたい人向けに、 `blob` と `tree` のハッシュ値を求める方法を説明します。

### 検証用のディレクトリの準備
まずは、適当に `git repository` を作って、`.git/objects`を見てみましょう。
最初は何も入っていないはずです。
```bash
$ cd /tmp
$ mkdir git_calc_hash
$ cd git_calc_hash
$ git init
$ find .git/objects -type f
```

## blob のハッシュ

`git add` してみると、一つファイルが追加されました。
今回のオブジェクトは `blob` です。 `git cat-file blob` で中身を表示できます。
引数のハッシュは4桁以上なら、省略しても構いません。

```bash
$ echo "hello world" > hello.txt
$ git add hello.txt
$ find .git/objects -type f
.git/objects/3b/18e512dba79e4c8300dd08aeb37f8e728b8dad
$ git cat-file blob 3b18
```

さて、この `3b18e512dba79e4c8300dd08aeb37f8e728b8dad` を導いてみます。
フォーマットは、`blob <ファイルのバイト数>\0<ファイルの内容>` です。
\0 はバイナリのゼロです。

```bash
$ echo "blob `wc -c ./hello.txt | awk '{print $1}'`\0`cat ./hello.txt`" | openssl sha1
3b18e512dba79e4c8300dd08aeb37f8e728b8dad
```

先ほどの値と同じになりました。

## tree のハッシュ

今度はツリーです。
まずは同様にツリーのオブジェクトも作ってみましょう。

```bash
$ git write-tree
68aba62e560c0ebc3396e8ae9335232cd93a3f60
$ find .git/objects -type f
.git/objects/68/aba62e560c0ebc3396e8ae9335232cd93a3f60
.git/objects/3b/18e512dba79e4c8300dd08aeb37f8e728b8dad
```


68ab...が追加されました。中身を見てみましょう。

```bash
$ git cat-file tree 68ab
100644 hello.txt;ۧr
```

`100644` はパーミッションで、`hello.txt`はファイル名ですね。
最後の`;r` はバイナリのようです。`hexdump`で見てみましょう

```
$ git cat-file tree 68ab | hexdump -C
00000000  31 30 30 36 34 34 20 68  65 6c 6c 6f 2e 74 78 74  |100644 hello.txt|
00000010  00 3b 18 e5 12 db a7 9e  4c 83 00 dd 08 ae b3 7f  |.;......L.......|
00000020  8e 72 8b 8d ad                                    |.r...|
00000025
```

"\0"を挟んで、`3b 18 ...` なので、先ほど計算した、hello.txtのハッシュのようです。
フォーマットは、`<パーミッション> <ファイル名>\0<ファイルのハッシュ>` ですね。
では、計算してみましょう。

```bash
$ oid=$((echo -n "blob `wc -c ./hello.txt | awk '{print $1}'`\0" && cat ./hello.txt) | openssl sha1)
$ content="`stat -f "%p" ./hello.txt` hello.txt\0`echo -n $oid | xxd -r -p -`"
$ (echo -en "tree `echo -n $content | wc -c | awk '{print $1}'`\0" && echo -n $content) | openssl sha1
68aba62e560c0ebc3396e8ae9335232cd93a3f60
```

計算できました。


最後になりますが、 今回手動で行ったハッシュの計算は `git hash-object` で確認することもできます。
シェルがすごい複雑になってしまったので、もし、もっと良い書き方をご存じの方がいれば教えていただけると幸いです。
