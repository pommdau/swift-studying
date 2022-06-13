## [Faster and simpler notarization for Mac apps](https://developer.apple.com/videos/play/wwdc2021/10261/)

## 関連
- https://twitter.com/tobi462/status/1529610841372639232
    -　わかりやすいまとめ
- [Apple、App Store外で公開するMacアプリのマルウェアチェックを行う公証サービスをより速くシンプルに行える「notarytool」コマンドを提供。](https://applech2.com/archives/20210611-apple-provide-notarytool-command.html)
altool (deprecated) -> notarytoolに移行しよう

## まとめ

- altool (deprecated) -> notarytoolに移行しよう
- 4倍早い
- 煩雑だった公証完了の待ち処理も、waitオプションを使うだけでOK.
- web-hookによるコールバックで、CIワークフローに組み込む際に便利
