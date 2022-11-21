---
title: 'プラグインなしでVimを使うテクニック'
emoji: "⛳"
type: "tech"
topics: ["vim"]
published: false
---

プラグインなしでVimを使うテクニックについて説明します。

# TL;DR

*  ファイラーは `netrw`
*  ファイル検索は `:find`
*  文字列検索は `:vimgrep`
*  エラー表示は `:make`

正直、がっつり開発するならLSPのプラグインは必須です。
LSPのプラグインを入れる理由は、主に定義ジャンプと参照ジャンプです。
neovimなら一応プラグインなしでLSPを扱えますし、`ctags`を使うという方法もありますが今回はその辺りは説明しません。
逆にLSP以外の機能については、プラグインなしでもなんとかなると筆者は考えています。

# ファイラー

## プラグインならこの辺り

* https://github.com/preservim/nerdtree
* https://github.com/Shougo/defx.nvim
* https://github.com/weirongxu/coc-explorer
* https://github.com/lambdalisue/fern.vim

## 代わりにnetrwを使う

`netrw` はVimに標準で含まれているプラグインになります。
`:Explore` か、`:E` でファイラを開けます。左側に開きたい場合は、`:Vexplore`しましょう。

`<CR>` でディレクトリを開閉かファイルを開けます。
`t` で新しいタブで開き、`o` や `v` で上下左右に分割して開きます。
ツリーにするには以下の設定をしときましょう。
```
let g:netrw_liststyle = 3
```

`%` でファイル作れて`D`で消せます。`d`でフォルダ作れるけど消すのは空じゃないとできません。
`%<Tab>` で今のディレクトリを補完できますので、`:rm! -rf %<Tab>/xxx` で補完して消すのがおすすめです。
ファイル開いてから、そのファイルで `:E` すれば、そのファイルのフォルダで開けます。
逆にルートから開き直したい場合は、`:E .` としましょう。

他のEから始まるコマンドと競合する場合は以下の設定をしておきましょう。
```
command! -nargs=? E Explore <f-args>
```

# ファイル・文字列検索

## プラグインならこの辺り

* [junegunn/fzf.vim](https://github.com/junegunn/fzf.vim)
* [Shougo/ddu.vim](https://github.com/Shougo/ddu.vim)
* [nvim-telescope/telescope.nvim](https://github.com/nvim-telescope/telescope.nvim)

## ファイルの曖昧検索には :find を使う

`:fin` で使えます。
phpファイル を検索したい場合は、`:fin *.php<Tab>` です。`<C-d>` で`popup menu`を開けます。
`<C-n>`と`<C-p>` で上下移動できます。項目を選ぶ場合は`<C-y>`、popup menuを消す場合は`<C-e>`、
`<C-l>`でできる限り補完してくれます。
再帰的に検索したい場合は、`path`を設定する必要があります。
基本は`set path+=**`ですが、`node_modules`のような大量のファイルがあるとめちゃくちゃパフォーマンスが悪くなりますのでその場合は、`set path+=src/**,lib/**` など適宜変える必要あります。

## フォルダ内の文字列検索には :vimgrep を使う

`:vim` で使えます。
src フォルダ内を検索したい場合は、`:vim hello src/** | cw` とすると、quickfix window が開いて、そのままクリックするだけで開けます。
`:cn` で次のマッチ、`:cnf`で次のファイルのマッチ、`:cp` で前のマッチに飛びます。


# 診断・ビルドのエラー表示とジャンプ

## プラグインならこの辺り
この辺りは基本、LSPの機能になります。

* [dense-analysis/ale](https://github.com/dense-analysis/ale)
* [nvimのlsp](https://neovim.io/doc/user/lsp.html)
* [prabirshrestha/vim-lsp](https://github.com/prabirshrestha/vim-lsp)

## 代わりに:makeを使う

Makefile にコマンドを設定して、`:mak lint` で `quickfix window` に出します。

```
lint:
	eslint --ext=js,ts -f unix --fix .
build:
	go build ./...
```

または、`makeprg` を書き換えることもできます。
コマンド実行時に返される文字列のフォーマットは`errorformat`で設定できます。
