---
date: 2010-11-24
title: Long Life Web Performance Optimization ~ 心理学から考えるWebパフォーマンス ~
categories:
- performance
---

クライアントサイド・パフォーマンス・ロード・オブ・ザ・リング3部作ついに完結ですw。ということで、<a href="http://dotcoder.net/2010/10/09/1219">dotcoder</a>や<a href="http://www.wcaf.jp/archives/2010/10/29/144/">WCAF</a>で話しました Long Life Web Performance Optimization について薄れゆく記憶の復習も兼ねて思いの丈を綴ってみた。

過去の作品
<ul>
	<li><a href="http://t32k.me/mol/log/high-performance-web-design/">High Performance Web Design ~デザインから考えるハイパフォーマンスWebサイト~</a></li>
	<li><a href="http://t32k.me/mol/log/coding-web-performance/">Coding Web Performance ~Webパフォーマンス最適化のためのコーディング手法~ </a></li>
</ul>
<div id="__ss_5860661" style="width: 425px;"><strong><a title="Long Life Web Performance Optimization" href="http://www.slideshare.net/t32k/long-life-web-performance-optimization">Long Life Web Performance Optimization</a></strong><object id="__sse5860661" classid="clsid:d27cdb6e-ae6d-11cf-96b8-444553540000" width="425" height="355" codebase="http://download.macromedia.com/pub/shockwave/cabs/flash/swflash.cab#version=6,0,40,0"><param name="allowFullScreen" value="true" /><param name="allowScriptAccess" value="always" /><param name="src" value="http://static.slidesharecdn.com/swf/ssplayer2.swf?doc=longlifewebperformanceoptimization-101122074016-phpapp02&amp;stripped_title=long-life-web-performance-optimization&amp;userName=t32k" /><param name="name" value="__sse5860661" /><param name="allowfullscreen" value="true" /><embed id="__sse5860661" type="application/x-shockwave-flash" width="425" height="355" src="http://static.slidesharecdn.com/swf/ssplayer2.swf?doc=longlifewebperformanceoptimization-101122074016-phpapp02&amp;stripped_title=long-life-web-performance-optimization&amp;userName=t32k" name="__sse5860661" allowscriptaccess="always" allowfullscreen="true"></embed></object>

</div>
タイトルのLong Life Web Performance Optimization、長いのでLong Life WPOって略します。これは、Long Life Designからもじりました。Long Life Design というのは<a href="http://www.d-department.com/jp/">D&amp;DEPARTMENT PROJECT</a>のナガオカケンメイさんが行っているプロジェクトで、60年代とか70年代の昔の優れたデザインを今にも伝えて使い続けていこうって趣旨だったと思います。それに対して、Webの世界はどうでしょう？移り変わりが早いですよね。「お！これが90年代のテーブルレイアウトかぁ〜エクセレント！」なんてことはない。そこで今回はLong Life Designのように長〜く使えるWPOを心理学などの人間側から考えてみようってのが今回のセッションの狙いです。

<!--more-->
<h3>Agenda</h3>
<ul>
	<li><a href="#news">最近のパフォーマンス事情 | News</a></li>
	<li><a href="#machine">機械は変わる | Machine</a></li>
	<li><a href="#human">人間は変わらない | Human</a></li>
	<li><a href="#practice">プラクティス | Practice</a></li>
	<li><a href="#conclusion">まとめ | Conclusion</a></li>
</ul>
<h3 id="news">Web Performanceは死んだか？</h3>
<img class="fig" title="wpo.003" src="/static/blog/2010/11/wpo.003.png" alt="" width="470" height="352" />

ちょっと釣りっぽい見出しですが、最近パフォーマンスの話、聞かないですよね？去年の暮れあたりが賑わい的にピークだったかと思います。(※Googleのランキングアルゴリズ追加に関しても情報全くないですし...)とはいえ、結構パフォーマンス事情は賑やかですってのをこれから紹介していきます。
<h4>Web Performance Working Group</h4>
もっともビッグなニュースと言えば、W3Cで8/19にWeb Performance Working Groupが設立されたことです。このWorking GroupのミッションはWebアプリケーションのパフォーマンス計測のための仕様を作ることです。共同議長にはMicrosoftとGoogleの方がいます。この辺からMSのWebパフォーマンスに対する本気度が伺えますよね。今後に期待です。
<h4>API for Measuring Web Performance</h4>
そこで策定されていくのが計測のためのAPIです。で、そのAPIとはなんぞや？ってことなんですけども、
<ul>
	<li>Navigation Timing
ネットワークや読み込みなどの時間、リクエスト回数などの情報を取得することができます。</li>
	<li>Resource Timing
画像やスクリプトなどのリソースを読み込むときの時間、情報を取得することができます。</li>
	<li>User Timing
UAがコードを実行した時間を取得することができます。</li>
</ul>
<p style="text-align: right;"><span style="font-size: x-small;"><a href="http://standards.mitsue.co.jp/archives/001473.html">2010年8月のW3C | Web標準Blog | ミツエーリンクス</a></span></p>
window.performance.navigation.loadEnd みたいな感じでページの読み込み時間が簡単に分かるようになるんですね。つまり、YSlow や PageSpeed などのプラグインなしでも詳細な情報が取得することができます。このAPIを実装しているブラウザは共同議長の選出先と同じようにIE9とChrome6からとなっています。
<h4>Boomerang.js</h4>
それじゃ利用するにはまだまだ先の話だと思うのですが、そうでもないんですね。次に紹介するのはオープンソースのパフォーマンス計測ライブラリの<a href="http://yahoo.github.com/boomerang/doc/ja/">Boomerang.js </a>です。開発者は Yahoo! Inc. の Philip Tellis さんです。この Boomerang.js 先に紹介したパフォーマンス計測APIに似た機能を提供してくれます。（はてブコメントより、ブラウザがそのAPIをサポートしていればそれを利用するみたいですね。ありがとうございます。参考：<a href="http://developer.yahoo.com/blogs/ydn/posts/2010/07/boomerang_webtiming_api/">Boomerang and the WebTiming API · YDN Blog</a> ）

<img class="fig" title="wpo.006" src="/static/blog/2010/11/wpo.006.png" alt="" width="470" height="352" />

普通、自前でページのパフォーマンス計測しようと思うと、HEAD要素あたりに、new DateしてgetTimeで現在時刻を取ってきて、BODYの最後のほうでまたgetTimeして、その差分の時間を計測するといったことをするかと思います。これですと、最初のnew Dateから次のnew Dateするまでの時間、つまりドキュメント内での時間しか計測できてないわけですね。もっと重要なサーバーとのやりとりの時間、ネットワークのレイテンシなども考慮されていないわけです。これじゃいかんやろうと言うことで、このBoomerang.js を使うとどうなるかと言うと、BODY要素の最後らへんでBoomerang.jsを読み込み、初期化関数を書くだけで、ネットワークのレイテンシなどの時間を計測することが可能です。また取得した情報を任意のサーバーに送れることも可能です。

これをうまく利用できたら、Google Analytics と連携してパフォーマンスの良いユーザーがどれだけコンバージョンがあげたかなど詳細に記録できるかと思います。楽しみですね。
<h4>Beyond Design</h4>
最後に話題を変えて、今年5月に開かれたGoogle I/O 。<a href="http://www.google.com/intl/ja/events/io/2010/sessions/beyond-design-user-experience.html">GoogleとYouTubeの人が話したセッション</a>より良いユーザーエクスペリエンスの作り方の原則として、第1に速くあれ！ってことを挙げていました。もともと、Googleのデザインガイドラインでも速さってのは重要だと言ってきているので、これ自体は特に珍しいことではないのですが、そこで引用されていた言葉が興味深かったので挙げてみました。
<blockquote>“Speed is the most important feature. If your application is slow, people won’t use it. I see this more with mainstream users than I do with power users. I think that power users sometimes have a bit of sympathetic eye to the challenges of building really fast web apps, and maybe they’re willing to live with it, but when I look at my wife and kids, they’re my mainstream view of the world. If something is slow, they’re just gone.”
<p style="text-align: right;"><span style="font-size: x-small;">— Fred Wilson (Union Square Ventures)</span></p>
</blockquote>
フレッド・ウィルソンさんはTwitterにも投資している有名なベンチャー投資家ですね。この人が言った言葉に「スピードはもっとも重要な特徴だ、もしアプリケーションが遅ければ誰も使わなくなるだろう。この傾向はパワーユーザーよりもメインストリームのユーザーで顕著だ」と。パワーユーザーというのは日本で言えば、はてなを使っているような人たちでしょう。彼らはそれなりの知識があるので、ページが遅くても同情的な感情になるらしいです。負荷分散大変だろうなとか。大して、メインストリームユーザー、つまりノンデベロッパーの人はそんなこと知りませんから、なんで遅いん？は？みたいな感じですぐ去ってしまうんですね。

ここで重要なのは、スピードというのは決して一部のパフォーマンスオタクのだけのもではないということです。フレッドさん自体、投資家であり開発者ではありませんので、<strong>スピード</strong>というのは全ての人が当然のように期待するモノなのかなと考えます。

これまで見てきたように言わずもがなWPOというのは私たちにとって常に最重要課題なのではないかと考えます。さらに、前述のパフォーマンス計測の標準化が進めば、皆が同じルールの環境下で比較されるので、パフォーマンスが良いということはセールスポイントになることはもちろん、今後はミリ秒単位での厳しい争いになったりするのかなと考えます。
<h3 id="machine">機械は進化する</h3>
<img class="fig" title="wpo.011" src="/static/blog/2010/11/wpo.011.png" alt="" width="470" height="352" />

Webパフォーマンスというのは重要なのは理解できたかと思うので、今度はそれを対策するために機械（コンピュータ）の部分に目を向けてみましょう。
<h4>ムーアの法則</h4>
機械というのは当然のことながらものすごく進化が速いです。有名な<a href="http://ja.wikipedia.org/wiki/%E3%83%A0%E3%83%BC%E3%82%A2%E3%81%AE%E6%B3%95%E5%89%87">ムーアの法則</a>で半導体の集積密度は18〜24ヶ月で倍増するというのがあります。まぁ簡単に言ったら、半導体の性能はものすごいスピード向上していくことになります。２年前の自分と比べて今の自分が２倍成長してるかと言うと、そうではないですよね？人間と比べれば進化のスピードというのは比較にならないほど速いものです。

Webの世界においても同じことが言えます。現在、WPO でもっとも有効な対策であるHTTPリクエストを減らすこと。これまで２度クライアントサイドでのパフォーマンスについて話してきましたが、言っていることはただ一つです。HTTPリクエストを減らすことしか僕は言っていません。現状はリクエストを減らすには、イメージマップ、CSSスプライト、インライン画像、ファイルの結合といったことが挙げられます。では、こういった対策がブラウザ（機械）の改善（進化）が進めばどのように変わるのか次は見ていきましょう
<h4>CSS3</h4>
<img class="fig" title="wpo.014" src="/static/blog/2010/11/wpo.014.png" alt="" width="470" height="352" />

まずはCSS3。画像はコニッターさんの<a href="http://re-dzine.net/apple/iphone/2010/06/17/css3-iphone4/">iPhone4をCSS3で描いてみた</a>です！このような込み入ったものもCSS3で描けるということは、言わずもがなグラデーションボタンや見出しの背景画像といった比較的簡単なものならCSS３で十分事足りる可能性は大きいわけです。

ちなみに<a href="http://code.google.com/intl/ja/speed/articles/web-metrics.html">Googleが42億ページ調べた結果</a>、１ページにあるリソースの和は平均44個でそのうち画像の平均は29個と最も多く全体の60-70％に当たるらしいです。このようなデータからも画像を減らすことはHTTPリクエストを減らす有効な対策だと考えます。
<h4>Application Cache</h4>
アプリケーションキャッシュは、HTML5のオフライン機能ですね。マニフェストファイルというテキストファイルにキャッシュさせたいファイル名（パス）を記述しHTMLのマニフェスト属性に指定するだけで、セッションを越えての、オフラインでのキャッシュが可能になります。

つまり、１回リクエストしてキャッシュしてしまえば、次回以降は余計なリクエストは発生させないということです。この機能と同じことを実現しようと思うと、サーバーの方でEXPIREヘッダに遠い未来を指定しなければならないんですけど、アプリケーションキャッシュの場合クライアント側ですべてできるのは使い勝手がよいですね。
<h4>Resource Package</h4>
次に、リソースパッケージなんですがこれは<a href="http://hacks.mozilla.org/2009/11/a-proposal-resource-packages-to-improve-performance/">Mozillaの中の人提案した機能</a>で、簡潔にゆうとZIPでくれ！ってことです。何回もリクエストするから効率が悪いので全部ZIPでまとめて、そのZIPファイルだけリクエストするというやり方です。いやーこれ考えた頭いいですね。てかなんで今までなかったのか分からないほどクレバーな技術なんですけど、Firefox3.7に実装されるかどうかって話しだったんですけど、全然話にきかないので、たぶんFirefox5とかになるのでしょうか。

実装方法はZIPで配信したいファイル + マニフェストテキストにリソースのパスを書いてlink要素でリクエストするといった感じですね。考え方としてはCSS Sprite（1つの画像に複数の役割を持たせる）に近いのですが、Background Positionをちまちま設定しなくてもいい分、リソースパッケージのほうが圧倒的に管理が楽です。
<h4>SPDY</h4>
最後は、<a href="http://www.chromium.org/spdy">Googleが提案したSPDY</a>というプロトコルです。こちらはブラウザの機能改善というより現状のHTTP通信変えようぜってことです。だからといって、http://example.com のところが spdy://example.com となるということではありません。もっと裏側の部分が変わるんですね。SPDY で実現できるのはこの３つです。
<ul>
	<li>多重化ストリーム</li>
	<li>リクエストの優先付け</li>
	<li>HTTPヘッダーの圧縮</li>
</ul>
一番重要なのは多重化ストリームです。これは現状のHTTP通信だと、1のホスト名に同時に2つのリクエストしかできません。2コネクション貼られて2リクエストするわけですから、つまり1コネクション1リクエストの原則です。これを解消するために、最新のブラウザは6つまでコネクションを貼ることができるのですが、これが1000ユーザー、10000ユーザーが同時接続してきたらどうでしょう？6万コネクションがサーバーに貼られるわけですからあまり効率のよいことではないですよね。。そこで、SPDYでは1コネクションにリクエストする制限撤廃しました。1つのコネクションを何度も再利用するんですね。これが多重化ストリームです。なぜ、HTTPリクエストがコストが高いのかというと、リクエストするたびに接続・解除を繰り返すから効率が悪いので、SPDYはこの点を解決できるのではないでしょうか。

これまで4つ見てきたように、現在最も注意しなければならないHTTPリクエストも、近い将来、ブラウザの機能改善やネットワークの仕組みが変われば特に意識する必要はなくなるかもしれません。

とはいえ、実際問題、各ブラウザが一斉に改善（アップデート）されるわけではありませんので、各ブラウザの実装度を見つつ対応しなければならない状況は続きます。つまり、私たち製作者はは常に走り続けなければなりません(‘･ω･｀）...
<h3 id="human">人間は変わらない</h3>
<img class="fig" title="wpo.020" src="/static/blog/2010/11/wpo.020.png" alt="" width="470" height="352" />

そこで、人間側に着目する必要があります。人間というのは昔から何も変わっていません。オギャーって生まれて恋をしてカクカクシカジカで死んでいくわけですよ。少なくとも、2年毎に2倍成長しているかといえばそうではありません。

<img class="fig" title="wpo.021" src="/static/blog/2010/11/wpo.021.png" alt="" width="470" height="352" />

要はこれまでは機械側ばっかりに着目してきたわけですけど、ここは進化が速い、ついて行くのはしんどいってことで、もっと変化の遅い人間側に着目しましょう。そうすればもっと楽できるかもしれませんってのが今回のセッションの狙いでもあります。
<p style="text-align: center;"><span style="font-size: medium;"><span style="color: #800080;"><strong><span style="color: #999999;">認知・知覚・感受・体感</span> | Perception</strong></span></span></p>
そこで、人間は知るうえで重要になってくるのが知覚の問題です。理論的に人間は最大で、1秒間126bit、 1分感で7560ビット、1時間50万ビットくらいの大量の情報を処理できるそうです。でも実際は毎秒10ビットくらいしか処理していません。つまり、結構省エネ、 相対的に判断したり、これまでの知識を元に判断したりして情報を簡略して処理しています。 <strong>つまり、世界をありのままにインプットしているわけではありません。</strong>

<strong><a href="http://ja.wikipedia.org/wiki/%E3%83%81%E3%82%A7%E3%83%83%E3%82%AB%E3%83%BC%E3%82%B7%E3%83%A3%E3%83%89%E3%83%BC%E9%8C%AF%E8%A6%96"><img class="fig" src="/static/blog/2010/11/wp.023-001.png" alt="チェッカーシャドウ錯視" width="470" height="352" /></a>
</strong>

このことを理解するのに良い例があります。上記の図のA、Bのマスの色は違いますよね、実は2つは同じ色なんですね。2本のグレーの線を引きますと、同じ#6B6B6Bのグレーだと理解できます。

<a href="http://ja.wikipedia.org/wiki/%E3%82%A8%E3%83%93%E3%83%B3%E3%82%B0%E3%83%8F%E3%82%A6%E3%82%B9%E9%8C%AF%E8%A6%96"><img class="fig" src="/static/blog/2010/11/wp.024-001.png" alt="エビングハウス錯視" width="470" height="352" /></a>

もう1つ例がありますので紹介します。こちらは2つのオレンジ色の円の大きさは左の円の大きさの方が小さく感じます。しかし、これも周りのオブジェクトを消してみると同じ大きさというのが分かりますよね。

つまり、人間というのは曖昧に世界を知覚しているのが理解できるかと思います。

<img class="fig" title="wp.025-001" src="/static/blog/2010/11/wp.025-001.png" alt="" width="470" height="352" />

これは、別に視覚だけに限ったことではありません。時間感覚においても同じようなこと言えます。<a href="http://www.newscientist.com/article/mg15220571.700-why-time-flies-in-old-age.html">アメリカの神経科学学会の人が発表したレポート</a>によると、同じ3分という時間でも、若い人とお年を召した方では体感時間が違うといったデータが報告されています。この調査によると若い人にとっては3分は自分で秒数をカウントするとで実際は平均3:03秒くらいだったそうです。対して、60歳以上の人にとって3分は3:40秒に感じられたようです。つまり、お年を召した方のほうが流れている時間は速く感じているようです。

時間感覚は年齢以外にも、地理的条件で違ったり、文化気候、体温によっても影響をうけるようです。もうちょっと具体的にWebではどんな事例があるかと言いますと、

<img class="fig" title="wp.027-001" src="/static/blog/2010/11/wp.027-001.png" alt="" width="470" height="352" />

User Interface Engineering Blog が<a href="http://www.uie.com/articles/download_time/">2001年に調べた調査</a>があります。ユーザーに10個の違うサイトを使ってもらってどのサイトが一番速く感じたか？という調査です。

調査結果を見てみると、About.comは実際の平均読み込み時間が8秒で一番速かった（※2001年調べです）にもかかわらず、ユーザーからは一番遅いと判断されました。対して、amazon.comは実際の平均読み込み時間が36秒ともっとも遅かったにもかかわらず、ユーザーからは最も速いと評価されました。

この調査のまとめでは、実際のダウンロード時間と体感速度に相関性はなく、それよりもユーザーのタスク処理の成功度具合と体感速度に相関性があると締めくくっていました。
<p style="text-align: center;"><span style="color: #888888;"><span style="font-size: medium;"><strong>スピードは重要じゃない!? <span style="color: #800080;">| Speed</span></strong></span></span></p>
え、これって、スピードって重要じゃないってこと？

このことを理解するのに重要なキーワードを持つ人物がアメリカの心理学者のミハイチクセントミハイです。彼が提唱した概念にフローというのがあります。

フローつまり流れるというこの言葉の意味は、
<blockquote>１つの活動に深く没入し他の何ものも問題とならなくなる状態

注意が自由に個人の目標達成のために投射されている状態</blockquote>
のことを指します。つまり、完全に集中している状態のことですね。皆さんも経験ありませんか？コーディングがのりに乗って気づいたらお昼だった。またはもう定時？といったことないですかね？ああいった感じを想像してもらえれば理解が速いかと思います。

フローの構成要素にはこうっいったものが挙げられます。
<ul>
	<li>明確な目的</li>
	<li>専念と集中</li>
	<li>活動と意識の融合</li>
	<li><strong><span style="color: #800080;">時間感覚のゆがみ</span></strong></li>
	<li><span style="color: #000000;">直接的で即座な反応</span></li>
	<li>能力の水準と難易度とのバランス</li>
	<li>自分で制御している感覚</li>
	<li>活動に本質的な価値がある</li>
</ul>
明確な目的をもって集中している状態ですよね。ここで重要なのは時間感覚のゆがみです。つまり、先程のamazon.comの結果をフローを用いて、もう1度説明すると実際速度が遅かったにも関わらず速いと評価されたのはタスクの成功率、使いやすさのおかげで集中することができ、軽いフローみたいな状態にユーザが近づいたため、時間感覚がゆがみ、実際の速度以上に速いと感じるようになったのではと考えられます。

つまり、このフローをうまく利用することができれば、サイトの体感速度も向上させることができるのではないでしょうか？

この人間編をまとめますと、人間を曖昧で相対的です。しかし、フローといったモデルをうまく利用することでWPOにもうまく利用できると考えます。
<h3 id="practice">実践編</h3>
<img class="fig" title="wp.035-001" src="/static/blog/2010/11/wp.035-001.png" alt="" width="470" height="352" />

さて、ここからはWebサイトにフローを落とし込むためにはどうしたら良いのか考えてみましょう。

ここまで人間側の部分を考えてきました。しかし、体感速度を向上させるために人間の頭をこねくりまわすことはできないので、知覚される前になにか対策する必要性がありますね。つまり、インターフェイスが重要になってくるわけです。

Webインターフェイスをデザインするパターンはいろいろあるのですが、ここではフローの構成要素から、<strong>直接的で即座な反応</strong>と、<strong>自分で制御している感覚</strong>から考えるに、
<p style="text-align: center;"><span style="font-size: medium;"><span style="color: #999999;"><strong>フィードバック<span style="color: #800080;"> |Feedback</span></strong></span></span></p>
フィードバックというパターンがフローを作り出すに最適なパターンかと今回は考えます。では、実際にフローのためにどのようなフィードバックをしたらよいのか見ていきましょう。
<h4>Live Preview</h4>
まずはライブプレビュー。Twitterのパスワード設定とかナイスだと思うんですね。まぁなんてこたないんですけども、パスワードの安全度をタイプするたびに提示（プレビュー）してくれるんですね。もしこれがライブではなくて、とりあえず入力して、設定ボタン押してページが切り替わってから「パスワードに不備があります。もう一度設定しなおしてください。」言われたらどうでしょう。さらに一度入力したフォームの情報もクリアになってたらどうでしょうか？完全にフローは断ち切られますよね。ライブプレビューはそういった意味で、即座の反応と自分で操作している感からフローを作るのに最適化と考えます。
<h4>Progressive Rendering</h4>
次のフィードバックはもう少し短いスパンにおいてのフィードバックです。プログレッシブレンダリングとは、まぁプログレッシブエンハンスメントという言葉もあるように、漸次的、段階的レンダリングと考えればよいでしょう。つまり、真っ白なページから、いきなりページが表示されるのではなく、ヘッダー、サイドナビ、メインコンテンツといった具合に順々にレンダリングしていく具合です。

<img class="fig" title="wp.041-001" src="/static/blog/2010/11/wp.041-001.png" alt="" width="470" height="352" />

プログレッシブレンダリングで有名な<a href="http://radar.oreilly.com/2009/06/bing-and-google-agree-slow-pag.html">Bingの事例</a>のよると、Bingの検索結果画面はそれまでプログレッシブレンダリングしてませんでした。検索クエリを入力して結果が表示されるまでに真っ白な画面をユーザーに表示させていました。そこで、スライドの紫色の選より上の部分、ビジュアルヘッダーの部分是が非でもまず表示させるように対応したようです。そうしただけで、各種評価は軒並み上昇しました。中でも、ユーザーの満足度は0.7％上昇と、ページをリニューアルしたとき並の効果があったようです。

ここで重要なのは全体のページの読み込み速度自体は変わってはいないのですが、まず何か見せておくことでユーザーに安心感を与えるいるわけですね。
<h4>Progress Indication</h4>
最後はお馴染みのインジケータですね。ページの読み込み速度が速いことにこしたことはないのですが、やはり、どうしても読み込みに時間がかかる場合もあるわけですよね、FLASHサイトとか。そういった場合に、ユーザーをつなぎとめとくために、フィードバックを返しつづけることで安心感を与えることは大切です。それは、読み込み中... ていった文言でもいいですし、くるくるとか、 プログレスバーなどのケアを心がけたいところです。
<h4><a href="http://www.websiteoptimization.com/speed/2/">Flow in Web Design</a></h4>
<ul>
	<li>Clear navigation</li>
	<li>Match challenge to skills</li>
	<li>Simplicity</li>
	<li>Importance</li>
	<li>Design for fun and utility</li>
	<li>Avoid cutting-edge technology</li>
	<li>Minimize animation</li>
</ul>
Web Designにフローを落としこむテクニックはフィードバック以外にも上記のようなものがありますので、みなさんも取り入れてみてはどうでしょうか？
<h3 id="conclusion">まとめ</h3>
<img class="fig" title="wp.044-001" src="/static/blog/2010/11/wp.044-001.png" alt="" width="470" height="352" />

これからもブラウザは進化し続けます。結局、何のためにWebパフォーマンス最適化をするのか？といったことですよね。決して、YSlow のスコアのためでもなければ、開発者の自己満足のためにでもありまん。ユーザーのために最適化するわけです。

そうしたらやっぱりユーザー（人間）側についても考える必要性があるのではないでしょうか。知覚や認知ついて知れば、それは今後数十年といったスパンで使える知識です。

そういった知識をWebに落としこむことで、冒頭で申し上げました Long Life Web Performance Optimization は可能になると考えます。みなさんもより良いWeb Developer Lifeにしてみてください。

<img class="fig" title="wp.048-001" src="/static/blog/2010/11/wp.048-001.png" alt="" width="470" height="352" />

ありがとうございました。
<h3>参考書籍</h3>
<table border="0" cellpadding="5">
<tbody>
<tr>
<td valign="top"><a href="http://www.amazon.co.jp/exec/obidos/ASIN/4873112710/warikiru-22/ref=nosim/" target="_blank"><img class="fig" src="http://ecx.images-amazon.com/images/I/51YMPSBD9AL._SL160_.jpg" border="0" alt="Mind Hacks ―実験で知る脳と心のシステム" width="111" height="160" /></a></td>
<td valign="top"><a href="http://www.amazon.co.jp/Mind-Hacks-%E2%80%95%E5%AE%9F%E9%A8%93%E3%81%A7%E7%9F%A5%E3%82%8B%E8%84%B3%E3%81%A8%E5%BF%83%E3%81%AE%E3%82%B7%E3%82%B9%E3%83%86%E3%83%A0-Tom-Stafford/dp/4873112710%3FSubscriptionId%3D15SMZCTB9V8NGR2TW082%26tag%3Dwarikiru-22%26linkCode%3Dxm2%26camp%3D2025%26creative%3D165953%26creativeASIN%3D4873112710" target="_blank">Mind Hacks ―実験で知る脳と心のシステム</a><img style="border: none;" src="http://www.assoc-amazon.jp/e/ir?t=warikiru-22&amp;l=ur2&amp;o=9" alt="" width="1" height="1" />
Tom Stafford Matt Webb 夏目 大

オライリージャパン  2005-12-01
売り上げランキング : 50592

<a href="http://www.amazon.co.jp/Mind-Hacks-%E2%80%95%E5%AE%9F%E9%A8%93%E3%81%A7%E7%9F%A5%E3%82%8B%E8%84%B3%E3%81%A8%E5%BF%83%E3%81%AE%E3%82%B7%E3%82%B9%E3%83%86%E3%83%A0-Tom-Stafford/dp/4873112710%3FSubscriptionId%3D15SMZCTB9V8NGR2TW082%26tag%3Dwarikiru-22%26linkCode%3Dxm2%26camp%3D2025%26creative%3D165953%26creativeASIN%3D4873112710" target="_blank">Amazonで詳しく見る</a> by <a href="http://www.goodpic.com/mt/aws/index.html">G-Tools</a></td>
</tr>
</tbody>
</table>