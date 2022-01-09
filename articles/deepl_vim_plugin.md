---
title: 'VimでDeepL翻訳のプラグインを使う (無料版対応)'
emoji: "⛳"
type: "tech"
topics: ["vim", "neovim", "deepl"]
published: true
---

# deepl.vim

DeepLはGoogle翻訳よりも精度が高いとされる翻訳サービスです。
2021年7月よりDeepL API Freeという無料プランが追加されました。
1ヶ月あたり500,000文字まで使うことができます。

deepl.vimは、Vim、NeoVimの両方でDeepL翻訳を利用できるプラグインです。

https://github.com/ryicoh/deepl.vim

## デモ動画

![デモ動画](https://user-images.githubusercontent.com/37844673/148679023-6814c111-6ada-4279-99ea-cd29afc6a1f3.mov)

## インストール方法と使い方

### DeepLアカウント作成

まずは、DeepLのアカウントを作成します。作成はもちろん無料ですがクレジットカードが必要になります。

[deepl.com](https://www.deepl.com)

登録が終了したら次は`AuthKey`を取得します。

`AuthKey`はアカウントページの一番下から、`Authentication Key for DeepL API` の部分にあります。

[アカウントページ](https://www.deepl.com/pro-account/summary)

### curlのインストール

内部的にcurlを利用していますので、インストールしてください。

MacOSならbrewから入れられます。

```bash
brew install curl
```


### deepl.vimのインストール

[vim-plug](https://github.com/junegunn/vim-plug)を使う場合はvimrcに以下の１行を追加します。
設定には、deeplのFree用URLと先ほど取得した`AuthKey`とキーマップを設定します。


```vim:.vimrc
Plug "ryicoh/deepl.vim"

" フリー版のエンドポイントを指定
let g:deepl#endpoint = "https://api-free.deepl.com/v2/translate"

" 先ほどコピペしたAuthKeyを指定
let g:deepl#auth_key = "00000000-0000-0000-0000-000000000000:fx"

" キーマップを指定。t => CTRL+j で日本語変換、t => CTRL+e で英語変換するように設定します。
vmap t<C-j> <Cmd>call deepl#v("JA")<CR>
vmap t<C-e> <Cmd>call deepl#v("EN")<CR>
```

翻訳したい英語をvで選択した後に、t を押して CTRL+j を押すと、選択部分が日本語に変換されます。
キーマップの`t`は、元々は`f`同様に横方向の1文字検索に使うのですが、
`t`のあとがCTRLを押しながらの場合は特に何もしないため、このマッピングにしています。
翻訳を意味する`translation`の`t`と`japanese`の`j`か`english`の`e`と覚えてください。

### DeepL プロ版

プロ版を利用する場合は、エンドポイントを以下に設定してください。
プロ版の値段2022年1月現在で630円+従課金です。
1,000,000文字あたり2500円で、少しお高いような気も。。？

```vim:.vimrc
let g:deepl#endpoint = "https://api.deepl.com/v2/translate"
```
