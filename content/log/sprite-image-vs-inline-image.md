---
date: 2013-07-31
title: CSS Sprite画像はDataURI画像にすべきか？
categories: 
    - performance
excerpt: "最近、スプライト画像はDataURIにすべきですか？という質問が多くて、調べてみました。てか、前のもそんな話題があったような。"
---

最近、スプライト画像はDataURIにすべきですか？という質問が多くて、調べてみました。てか、前のもそんな話題があったような。DataURIってなんぞって方は下記を見てほしい。

+ [データURIスキーム | MOL](http://t32k.me/mol/log/data-uri-scheme/)

> CSSファイルがパースされなければレンダリングが始まらないのでCSSファイルの肥大化は絶対に避けなければならない。画像の1KBとCSSファイルの1KBを同じように考えてはいけない。 ― [ぼくのかんがえたさいきょうのしーえしゅえしゅ ](http://t32k.me/mol/log/the-perfect-css-i-thought/)

あ、ホントそうだっけーなーと思いつつ、どこぞの資料見たんだっけなーと探してたらあった。

<iframe src="//www.youtube.com/embed/YV1nKLWoARQ" frameborder="0" width="560" height="315"></iframe>

[Optimizing the Critical Rendering Path for Instant Mobile Websites - Velocity SC 2013](https://docs.google.com/presentation/d/1IRHyU7_crIiCjl0Gvue0WY3eY_eYvFQvSfwQouW9368/present#slide=id.gc57996a9_0168)

このセッションはすごく分かりやすいのでオススメです（該当の箇所は12分位から）。というかIlya Grigorik ++

![](/static/blog/2013/07/11.png)

セッション中の資料には、ご覧のとおり、HTMLがパースされてDOMが完成したところで、画面には何も表示されない。感覚的には、スタイルのついてない『Hello world!』くらい表示されてもいいじゃんか！と思うんだけど、表示されない。

![](/static/blog/2013/07/21.png)

次に、CSSがパースされてCSSOM（CSSのDOMみたいなもの？Style Rulesとかとも言ったりする）が構築されるが、まだ画面は空白のままだ。

![](/static/blog/2013/07/31.png)

DOMとCSSOMがガッチャンコしてRender Treeが構築され、そこにレイアウト情報が加わって初めて描画となるんだね。この辺りの更に詳しい情報はHTML5 Rocksの以下の記事がすばらしいので読んでほしい（すぐ眠たくなるけど）。

+ [ブラウザのしくみ: 最新ウェブブラウザの内部構造 - HTML5 Rocks](http://www.html5rocks.com/ja/tutorials/internals/howbrowserswork/)

ここで重要なのはHTMLと（読み込んでいる）スタイルシート（CSS）が無い限り、描画はされないということだ。つまり、CSSの読み込みに手間取ればCSSレンダリングをブロックするということが考えられる。

レンダリングをブロックするのはJSファイルだけかと思ってたけど、スタイルシート（CSS）も気をつけなければならないということが分かる。

そこで冒頭にも述べたように、CSSファイルを出来る限り軽くし、読み込みを速くすることでレンダリングのブロックを回避するという考えになってくると思うが、実際どんなもんなのよ？と思ったので、テストファイル作ってみた。

+ [Normal CSS Sprites](https://dl.dropboxusercontent.com/u/356242/test/httprequests/normal_sprite.html)
+ [Inline image CSS Sprites](https://dl.dropboxusercontent.com/u/356242/test/httprequests/inline_sprite.html)


30個くらいのアイコン画像をスプライト化して読み込んでいるのと、それをさらにDataURIにしているもの比較だ。それらを[WebPagetest](http://www.webpagetest.org/)にかけてみた。

![](/static/blog/2013/07/filmstrip.png)

#### ビジュアル比較テスト結果

+ [WebPagetest - Visual Comparison](http://www.webpagetest.org/video/compare.php?tests=130730_2V_G48,130730_7X_G49)

#### 各テスト結果

+ [WebPagetest Test Details - Dulles : Normal.../normal_sprite.html](http://www.webpagetest.org/result/130730_2V_G48/1/details/)
+ [WebPagetest Test Details - Dulles : DataURI...inline_sprite.html](http://www.webpagetest.org/result/130730_7X_G49/3/details/)

どうでしょうか？

Fully Loadedがノーマルで**1.187s**で、DataURIが**0.994s**で、ビジュアル比較においても、表示完了までの時間がDataURIを使用した方が速い。まぁHTTPリクエストが2つと3つじゃ、リクエストの少ないほうが速いのは当たり前なんですけど、ここではレンダリング過程を見てほしい。

ノーマルは0.7秒あたりから画像なしだけどレンダリングが始まっているのに対して、その時点ではDataURIは真っ白な画面のままだ。[Start Render](https://sites.google.com/a/webpagetest.org/docs/using-webpagetest/quick-start-quide#TOC-Start-Render:)を見ても、ノーマルが、**0.697s**に対して、DataURIは**0.920s**で、普通の画像のほうがレンダリングが速く始まっている。

これは先程の、Ilya Grigorik氏のプレゼン内容を理解していれば当然の現象と理解できるだろう。DataURI化した文字列を含んだCSSは**59KB**サイズもあるのに対して、画像パスで読み込んだCSSは**3KB**と軽量だ。この読み込みの差が反映された結果になっている。

比較のテストはシンプルな実装だが、現実問題のサイトはもっと複雑だ。どのようなレンダリング過程になるのかはサイトそれぞれだし、DataURIにして読み込み時間を最小限するというのもひとつの手段かと思う。

[![](/static/blog/2013/07/vp.png)](https://sites.google.com/a/webpagetest.org/docs/using-webpagetest/metrics/speed-index#TOC-Measuring-Visual-Progress)

ただ、我々は機械ではないので、数値を機械的に判断するのではなく、**体感速度**という観点からも考えなければならない。ノーマルのテストのようなブログレッシブレンダリングは、ユーザーに即座のフィードバックを返している点で、ユーザーに安心感を与えている。

このような事例として、[検索サイトのBingはプログレッシブレンダリングをすることでユーザーの満足度を0.7％向上させた](http://t32k.me/mol/log/long-life-web-performance-optimization/) （サイトリニューアル並に上昇したようです）という例もある。

であるので、CSSにレイアウト情報をしっかり記述したり、画像が読み込まれていない時点でのスタイルのケア（background-imageだけでなく似たような色のbackground-colorも指定しておくなど）もしておけば、後は画像が当て込まれていくだけなので体感速度は向上するといったことも考えられるだろう。

セッションでは、AFT(Above-the-fold time)、つまりファーストビューで見える範囲だけのCSSをHTMLにインラインで記述し、残りCSSファイルは遅延読み込みするといったテクニックが披露されているが、実際の運用ベースで使うとなると厳しいだろう。

そもそも的な話で、

<blockquote>
+ Base64 encoding incurs transfer size overhead of 1.37 times the original data, with another 814 bytes of header data. Server gzip compression reduces this overhead to around 3% so the penalty is relatively small.
+ The content must be decoded back into it’s original form by the browser. This operation consumes CPU &amp; battery on mobile devices.
+ If data URIs are included in HTML or uncached CSS, the content of the data URI will be sent to the browser from the HTTP server with each request.
+ Regardless of whether the data URI content exists in a cached CSS or HTML file, the browser must decode the image each time a page renders: the decoding cost is paid repeatedly every time a page is viewed.
</blockquote>

[DataURIの画像は、通常の画像に比べて6倍遅いとかゆう記事](http://www.mobify.com/blog/data-uris-are-slow-on-mobile/)もありますし、ファイルサイズ自体も元より増加するし、毎回デコードしなければならなかったり、

<blockquote>
Inline images judiciously
	+ Inlining increases parse time
	+ External images don't block the parser
	+ Can defer resource discovery and execution
	+ SPDY server push &gt; image inlining
</blockquote>

また、Velocity2012での[nderstanding and Optimizing Web Performance Metrics](https://perf-metrics-velocity2012.appspot.com/#41)でも、DataURIは慎重に使用しろと言われてますので、用法用量お守りの上、お使いくださいませ。

個人的な意見としては一回ぐらいしか出てこない画像をDataURIするのであれば、そこのビューに埋め込めばいいし、かと言って何回も出てくるような画像であれば、毎回のデコードコストが気にかかるし、ならスプライトでまとめてキャッシュさせたほうが無難でしょ！と思う。またスプライト画像も今回述べたようにレンダリングのブロックにつながる可能性があるので絶対CSSファイルには入れない、使わない。

### 参考

+ [Optimize CSS Delivery - Google Developers](https://developers.google.com/speed/docs/insights/OptimizeCSSDelivery)
+ [Reduce the size of the above-the-fold content - Google Developers](https://developers.google.com/speed/docs/insights/PrioritizeVisibleContent)