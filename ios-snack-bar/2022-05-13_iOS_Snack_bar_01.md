# [iOS Snack bar \#1](https://ios-snack-bar.connpass.com/event/246443/)
## スライド
- [プロダクトグロースと技術のベースアップを両立させるRettyのアプリ開発スタイル / Achieve Product Growth and Tech Update](https://speakerdeck.com/imaizume/achieve-product-growth-and-tech-update)
- [機能横断型チームにおける技術改善](https://speakerdeck.com/takeshiakutsu/ji-neng-heng-duan-xing-timuniokeruji-shu-gai-shan)

## メモ
- Dangerというのがある。導入事例を割と見る。
   - [Danger\-Swift 導入のススメ / Path to import Danger\-Swift](https://speakerdeck.com/lovee/path-to-import-danger-swift)
- iOSDCでもあったSPMを使った構成の話。
    - [大規模なマルチモジュール開発をSwiftPackageに移行して運用してみた](https://tech.timee.co.jp/entry/2021/08/24/160205)
    - [pointfreeco/isowords](https://github.com/pointfreeco/isowords)
- SPMにより環境構築が容易に -> コミッターが増えて開発が活発になった。
    - CocoaPodsのインストール時に`permission denied`辛いよね…みたいなことがなくなる。(`sudo`はセキュリティ的にやめて〜)
- 定期的に必要な作業はMakefileに処理をまとめている
- SwiftUIはUIKitに比べてコードレビューがしやすく、チーム内で知見の高め合いがしやすい。
- 毎日リリースする環境ならGitHub Flowのmaster/featureだけのシンプルな運用じゃないと手間すぎるってことかな。
    - [GitHub Flowとは](https://qiita.com/tatane616/items/aec00cdc1b659761cf88)
- ライブラリの自動アップデート・自動でPRを作ってくれる。脆弱性がある場合は強調されるとか。
	- [RenovateによるiOSライブラリーの自動更新](https://speakerdeck.com/ikesyo/renovateniyoruiosraiburarifalsezi-dong-geng-xin)
	- [RenovateをiOSアプリ開発に導入してみた](https://tech.dely.jp/entry/2020/12/04/102515)
