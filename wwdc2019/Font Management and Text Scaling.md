# [Font Management and Text Scaling](https://developer.apple.com/videos/play/wwdc2019/227/)
# まとめ
- [WWDC19：iOS 13、iPadOS用サードパーティ製フォントはApp Store経由のインストーラー形式で提供 \#WWDC19](http://www.macotakara.jp/blog/news/entry-37641.html)

# 関連リンク
- [iOS13のカスタムフォントを試してみる](https://llcc.hatenablog.com/entry/2020/10/31/151755)
- [非表示Appの配信](https://developer.apple.com/jp/support/unlisted-app-distribution/)
- [\[Swift\] iOS13とフォントのお話](https://qiita.com/hcrane/items/a0a7a77f9d709e9692b6#1-%E3%83%95%E3%82%A9%E3%83%B3%E3%83%88%E3%82%92%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB%E3%81%99%E3%82%8B)

# Memos
- 新しいシステムフォント3つのリリース

![](https://i.imgur.com/HV1u4Y0.jpg)

![](https://i.imgur.com/o73ECB5.jpg)

![](https://i.imgur.com/gnoQCbJ.jpg)

- フォントの利用方法

![](https://i.imgur.com/oMNqs6O.jpg)

- フォント名では指定せずに抽象的に指定する

![](https://i.imgur.com/aAiz0cD.jpg)

![](https://i.imgur.com/iGzOQco.jpg)

- suitcase, psフォントはインストールできないよ

![](https://i.imgur.com/oiB0ual.jpg)

- FontのEntitlementsの設定

![](https://i.imgur.com/zyJPLF8.jpg)

![](https://i.imgur.com/Emnc5Q5.jpg)

- ブラウザ機能は必要
- システムからフォントは削除されるかもしれないから、変更を検知できるように。
- オンデマンドリソースを使用すること
    - = 第三者のサーバからは提供できない
    - ユーザが必要なフォントだけダウンロードさせること

![](https://i.imgur.com/Cmc8mnu.jpg)

- デモアプリ

![](https://i.imgur.com/9emZBuF.jpg)

- フォントのインストールにはユーザへの通知が必須

![](https://i.imgur.com/1iBRu5K.jpg)

- システムからインストールされたフォントを確認できる

![](https://i.imgur.com/FAW6Tbh.jpg)

![](https://i.imgur.com/iuW0yvX.jpg)

- こんな感じで検知できるようにね。

![](https://i.imgur.com/PdwBVaS.jpg)

- コードの紹介

![](https://i.imgur.com/IRpQ8hQ.jpg)

![](https://i.imgur.com/ScFucd9.jpg)

- フォントの変更を検知する

![](https://i.imgur.com/y9rYi9w.jpg)

![](https://i.imgur.com/E8IBDYF.jpg)

- UIの更新はNotificationで行っている

![](https://i.imgur.com/mMtYc4h.jpg)

- フォント要求側の話
- 多分前の話にでたこれの話

![](https://i.imgur.com/J8b6bJL.jpg)

![](https://i.imgur.com/LlCrsM6.jpg)

- 不足しているフォントの情報を取得する

![](https://i.imgur.com/YnenLDC.jpg)

![](https://i.imgur.com/25evGjW.jpg)

- 注意点

![](https://i.imgur.com/cnl4x9Y.jpg)


## カスタムフォントの使い方

![](https://i.imgur.com/450bwro.jpg)

![](https://i.imgur.com/i9v9XfO.jpg)

![](https://i.imgur.com/Phzq88l.jpg)

![](https://i.imgur.com/RLPY413.jpg)

![](https://i.imgur.com/2pEfzlf.jpg)

- フォント選択画面の実装(省略)

## Text Scaling
- OS間で見た目上の大きさが違う問題

![](https://i.imgur.com/0LkkEra.jpg)

![](https://i.imgur.com/KVzcnyD.jpg)


![](https://i.imgur.com/UeWNdCH.jpg)

- コピペ間の文字の大きさの問題は解決される

![](https://i.imgur.com/LYVHNXF.jpg)

![](https://i.imgur.com/ll8Wmzy.jpg)

![](https://i.imgur.com/fInYdR6.jpg)

![](https://i.imgur.com/o21JZK3.jpg)

![](https://i.imgur.com/nvoxQsw.jpg)

![](https://i.imgur.com/HOkLB9S.jpg)



