---
title: 'Vim で TypeScript の型チェックする'
emoji: "⛳"
type: "tech"
topics: ["vim", "neovim", "TypeScript"]
publication_name: "vim_jp"
published: true
---

やりたいことは、プロジェクト全体の型チェックを行い、エラーのリスト(`Quickfix`)を取得することです。

# TL;DR

Makefileに以下の追加して、`:make`します。

```:Makefile
typecheck:
	@npx -s tsc --noEmit | sed -e 's/(\([0-9]*\),\([0-9]*\))/:\1:\2/g'
```

## 解説

TypeScriptのLanguage Server(`tsserver`)は開いてるファイルしかエラー（`Diagnostics`）を出してくれません。
なのでプロジェクト全体でエラーが出ていないかをチェックするには、`tsc`コマンドを実行する必要があります。

vim/nvimには、`:make`というMakefileを実行してくれる機能があります。
そして、そこで見つけたエラーは`Quickfix`というところに貯めてくれて、エラーリストを行き来できたりします。
コミット前とかに`:make`しとくと、CIで落ちる前に気づけて便利です。

sedしてるのは、`tsc`コマンドからの出力フォーマットが`vim`の`errorformat`に対応していないためです。行番号の部分を書き換えています。
最近、Next.jsがディレクトリ名に`()`を入れ始めて、バグったのでこの正規表現を書き換えたためこの記事を書きました。
