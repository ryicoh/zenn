---
title: 'vscodeでVimのソースコードをデバッグする'
emoji: "⛳"
type: "tech"
topics: ["vim", "vscode"]
publication_name: "vim_jp"
published: true
---

Vimのソースコードをデバッグするのに、GUIのデバッガーが欲しくなったので、
大変心苦しくはありますが、vscodeの力を借りることにしました。
vscodeは普段全く使いませんし、C/C++は学校で習って以来なので、この記事の信頼性はかなり低いです。

![](https://storage.googleapis.com/zenn-user-upload/9550c496d2af-20221208.png)

# 環境

* M1 Macbook Air (arm64)
* clang 14.0.6
* lldb 14.0.6
* vscode 1.73.1

armだとgdbは多分使えないです。代わりにlldbを使います。

# やりたかった事

`:find` が `wildignore` を設定していても遅いので、それを直したかったです。
`vim-jp`の偉い方々によれば、先にファイル名を展開してるかららしいです。

# vimのビルド・テスト

まずはダウンロード

```bash
$ git clone https://github.com/vim/vim.git
$ cd vim
```

[src](https://github.com/vim/vim/tree/master/src) にソースコードが大量にあって、[src/testdir](https://github.com/vim/vim/tree/master/src/testdir) にテストファイルがあります。

ビルドはシンプルに

```
$ make
```

テストは、`src/testdir`に行って、make <テストファイル名>

```
$ cd src/testdir
$ make test_findfile
```

これだけでOKです。

# デバッガーの設定

まずは、[CodeLLDB](https://marketplace.visualstudio.com/items?itemName=vadimcn.vscode-lldb)をインストールします。
続いて先ほどのテスト実行した時のスクリプトを見ながら、vscodeのlaunch.jsonに追記します。

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Launch LLDB",
            "type": "lldb",
            "request": "launch",
            "program": "${workspaceFolder}/src/vim",
            "args": [
                "-f",
                "-u",
                "unix.vim",
                "-U",
                "NONE",
                "--noplugin",
                "--not-a-term",
                "-S",
                "runtest.vim",
                "test_findfile.vim",
                "--cmd",
                "au SwapExists * let v:swapchoice = \"e\"",
            ],
            "env": {
                "VIMRUNTIME": "${workspaceFolder}/runtime"
            },
            "cwd": "${workspaceFolder}/src/testdir",
        },
    ]
}
```

これでブレークポイントを設置しても引っかからないので、デバッグビルドします。
`src/Makefile` からそれっぽい行をコメントアウトします。

```Makefile:Makefile
CFLAGS += -g -O0 -DDEBUG -Wall -Wshadow -Wmissing-prototypes
```

ビルドし直します。

```
make clean
make
```

できました。
さて、皆さんも、vscodeのVim本体のソースコード読んで行きましょう。

いつか、Vimでデバッグもやってみたいです。
