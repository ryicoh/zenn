---
title: 'Vim で Typescript の型チェックする'
emoji: "⛳"
type: "tech"
topics: ["vim", "neovim", "TypeScript"]
publication_name: "vim_jp"
published: false
---


# TL;DR

Makefileに以下の追加して、`:make`するだけ

```:Makefile
typecheck:
	@npx -s tsc --noEmit | sed -e 's/(\([0-9]*\),\([0-9]*\))/:\1:\2/g'
```
