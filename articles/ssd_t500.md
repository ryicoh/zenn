---
title: 'Crucial SSD T500 のファームウェアを更新したら書き込みが安定した'
emoji: "💾"
type: "tech"
topics: ["ssd", "crucial"]
published: true
---

Crucial T500 は、書き込みが安定しないという問題があると以前から知っていたが、
筆者が2024/11/2に購入して、本問題が発生したのでその対応を記す。

端的に言うと、ファームウェアのバージョンをP8CR004に更新したら、書き込みが安定した。
シリアルナンバーは、202410から始まっていたので、製造時期はそのあたりと想定されるが、
バージョンはP8CR003のままだった。

https://www.crucial.jp/support/ssd-support/t500-support

Storage Executive からファームウェアを更新できる。
Kioxia の ユーティリティはWindowsしか対応していないが、
Crucial の場合はマイクロンのページからLinux版やArm版なんかもダウンロードできる。

https://jp.micron.com/sales-support/downloads/software-drivers/storage-executive-software#sidetabs-1513cf722e-item-f8944450d4-tab


* Before

![](https://storage.googleapis.com/zenn-user-upload/1807ddfa4c16-20241102.png)

* After

![](https://storage.googleapis.com/zenn-user-upload/a338952813f5-20241102.png)

