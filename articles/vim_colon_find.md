---
title: 'vim の :find を極める！'
emoji: "⛳"
type: "tech"
topics: ["vim", "find"]
published: false
---

# :find はまだ使える。。

今どき、vim で `:find` を使ってる人は少ないと思うのですが、全然、皆さんがよく使っている [Denite](https://github.com/Shougo/denite.nvim) や [fzf-preview](https://zenn.dev/yano/articles/vim_with_fzf_preview_is_best_experience) に負けないくらい（絶対ウソ）に使えるよというのを伝えたいです。

## 使われない理由

`:find` が使われない最大の理由は遅すぎることです。
