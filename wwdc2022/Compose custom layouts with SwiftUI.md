# [Compose custom layouts with SwiftUI](https://developer.apple.com/videos/play/wwdc2022/10056/?time=873)
## 所感
- GridはAutoLayoutで辛かった部分の解消にもなってて良い.
- Custom Layoutは割と裏で泥臭いことは必要、呼び出す側的には簡単で良さげ.
- ViewThatsFit: HStackで幅が足りないときにVStackにする、みたいなことができる.

## Twitterの声

<blockquote class="twitter-tweet" data-lang="ja" data-dnt="true" data-theme="dark"><p lang="ja" dir="ltr">Swift UIのCustom Layout、なかなか強力なものになっていて面白い。AnyLayoutすごいぞ <a href="https://twitter.com/hashtag/WWDC22?src=hash&amp;ref_src=twsrc%5Etfw">#WWDC22</a></p>&mdash; Date (@d_date) <a href="https://twitter.com/d_date/status/1534576455450886150?ref_src=twsrc%5Etfw">2022年6月8日</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

https://twitter.com/koher/status/1533962868084776960?s=20&t=MA6NrlT462pRZqji-9-NUA

## Grid
- 2次元配列のレイアウトを考える

![](https://i.imgur.com/I2tg1Bp.jpg)


![](https://i.imgur.com/rRqnror.jpg)
 
![](https://i.imgur.com/3vNRsmI.jpg)

![](https://i.imgur.com/QtmuLm5.jpg)

![](https://i.imgur.com/8s1hKiz.jpg)

![](https://i.imgur.com/1DseFoW.jpg)

- `.leading`

![](https://i.imgur.com/np2eRXE.jpg)

- 数字だけ.trailingに

![](https://i.imgur.com/rbRiiLE.jpg)

- 新しいModifier

![](https://i.imgur.com/iCKz32l.jpg)

![](https://i.imgur.com/9PaY98d.jpg)

- HStackではボタンのWidthを最大に合わせることができない

![](https://i.imgur.com/k6QQ6ta.jpg)

- Containerによって決定され、これでは広すぎる

![](https://i.imgur.com/WVtuYOp.jpg)

## Layout

![](https://i.imgur.com/sWY1wZw.jpg)

![](https://i.imgur.com/pSiLSVg.jpg)

![](https://i.imgur.com/6JapUAZ.jpg)

![](https://i.imgur.com/I9kiLGE.jpg)

![](https://i.imgur.com/gB9Bsma.jpg)

![](https://i.imgur.com/7vLPK7K.jpg)

![](https://i.imgur.com/pTisOCL.jpg)

![](https://i.imgur.com/aJWod0W.jpg)

![](https://i.imgur.com/hZtZ7Ev.jpg)

- refactoring

![](https://i.imgur.com/UAnGcRc.jpg)

![](https://i.imgur.com/NQxFgEK.jpg)

### placeSubviews

![](https://i.imgur.com/a1NoTJw.jpg)

![](https://i.imgur.com/OhxpXbm.jpg)

![](https://i.imgur.com/iCQI6aC.jpg)

- ごにょごにょしたけどこんな感じで隠蔽して使える

![](https://i.imgur.com/Hnf9u7P.jpg)

- スペースが足りない場合にVStackを採用するみたいな？

![](https://i.imgur.com/2w5m2FX.jpg)

![](https://i.imgur.com/iBoEIMW.jpg)
