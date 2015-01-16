---
date: 2013-08-21
title: HTTPリクエストを減らすために【WebFont編】ドラッグ＆ドロップしてコマンド叩いてウェーイ
categories:
- performance
---

このシリーズはHTTPリクエストの理解を通じてWebパフォーマンスの重要性について考える5章構成になっております。
<ul>
	<li><a href="http://t32k.me/mol/log/reduce-http-requests-overview/">【序章】HTTPリクエストは甘え</a></li>
	<li><a href="http://t32k.me/mol/log/reduce-http-requests-css-sprite/">【CSS Sprite編】スプライト地獄からの解放</a></li>
	<li><strong>【WebFont編】ドラッグ＆ドロップしてコマンド叩いてウェーイ</strong></li>
	<li><a href="http://t32k.me/mol/log/reduce-http-requests-datauri/">【DataURI編】遅延ロードでレンダリングブロックを回避</a></li>
	<li><a href="http://t32k.me/mol/log/reduce-http-requests-one-second/">【終章】我々には1000msの猶予しか残されていない</a></li>
</ul>
3日目は、スマホ環境であればHTTPリクエストを減らすためにWebフォントの採用について考慮しても、やぶさかではないでしょう。

まずは下記の画像をご覧頂きたい。

<img class="aligncenter size-full wp-image-5274" alt="arrows" src="/static/blog/2013/08/arrows.png" width="607" height="30" />

これは私がプロジェクトで使用していたスプライト画像ですが（実際は縦にして使用）、このような単純な形状、単色のアイコンであれば、Webフォント化したほうがなにかと都合がよいです。

このスプライトであれば、カラー×矢印の向き×シャドウのありなしでパターンが増えていく可能性があり、スプライトすれば1リクエストで収まりますが、それでも画像が肥大化していけば、<strong>Receiving </strong>が無視できない状況になっていきます。

そこでWebフォント化すれば、色は自由に変更できますし、フォントなので<span class="code">text-shadow</span>で影を当てることもできますし、<span class="code">font-size</span>で大きさも変更でき柔軟に対応することができます。

それではWebフォントって一体どうやって作るのでしょうか？高いフォント作成アプリケーションを購入しなければならないのでしょうか？オンライン作成ツールもあるようですが、毎回新しいアイコン追加の度にそのツールのサイトを訪問しなければならないのでしょうか？
<ul>
	<li><a href="http://icomoon.io/"> IcoMoon</a></li>
</ul>
以前は上記のオンラインツールを使ってましたが、やはり更新対応を考えるとめんどくさかったです。ということで、<a href="https://github.com/t32k/maple">Mapleプロジェクト</a>では<a href="https://github.com/sapegin/grunt-webfont"><strong>grunt-webfont</strong></a>を導入しました。

ということで、前回の記事を見ながら、<span class="code">grunt-init maple</span>しましょう^_^

必要環境として以下のものインストールしておきましょう 。
<a href="http://brew.sh/">Homebrew</a>も入っていなければインストールしておきましょう。
<pre class="bash"><code>$ brew install fontforge ttfautohint
$ brew install https://raw.github.com/sapegin/grunt-webfont/master/Formula/sfnt2woff.rb</code></pre>
あと事前に必要になるのは、フォントの元となるSVGファイルです。<a href="https://github.com/cognitom/symbols">SVGファイルを作る上で便利なテンプレート</a>があるのでそれを拝借してきましょう。

<img class="aligncenter  wp-image-5163" alt="arrow" src="/static/blog/2013/07/c2eef1dbac4917459d28818432f9c6b8.png" width="640" />

テンプレートを使ってMapleでは上記のような.aiを作成しました。<a href="http://iconmonstr.com/">iconmonstr</a>にはいろいろ使い勝手がいいアイコンが無料で配布されていますので、ここから必要な物をとってくるのもいいでしょう。ここからIllustratorでアートボード別に書きだしてSVGファイルとして保存します。要注意なのはこのアートボードの空白部分も含めてSVGファイルなので気をつけてください。

<img class="aligncenter size-full wp-image-5282" alt="fontdir" src="/static/blog/2013/08/fontdir.png" width="675" height="387" />

基本的にこの部分はデザイナーさんにやってもらえればよいことなので、フロントエンドデベロッパーは完成したSVGファイルを、<span class="code">/src/files/font/svg</span> ディレクトリに投げ込んでコマンド打つだけです。
<pre><code>$ grunt webfont
Running "webfont:dist" (webfont) task
Font 'myfont-b5fd89266afbbfbfc281a0ce9a5bf50e' with 13 glyphs created.</code></pre>
で、<span class="code">/src/tools/</span>で<span class="code">grunt webfont</span>を実行すれば上記のようなログなとともに、<strong>.woff</strong>と<strong>.ttf</strong>と_myfont.scssが作成されます。
<pre><code>// _setting.scss
//-------------------------------------
@import "../vendors/myfont";

// _myfont.scss
.icon-hoge:before {
content:"\f100";
}</code></pre>
_myfont.scssは_setting.scssで<span class="code">@import</span>されていますので、特にCSSをいじることはありません。

<img class="aligncenter size-full wp-image-5170" alt="icon" src="/static/blog/2013/08/icon.png" width="640" height="250" />

上記のように、<span class="code">.icon-{SVGファイル名}</span>のクラスを当てれば、すぐさま使えます。

Webフォント使えば、CSSスプライトに頼らなくても大丈夫だー＼(-o-)／って使う前は思っていましたが、やはり単純な表現に限定されます。グラデーションカラーかつシャドウ有りや、部分的に色を変えるなどはWebフォントでは無理がありますので、そこはCSSスプライトを採用するか、デザインを少し調整してWebフォント化するかはデザイナーと相談しましょう。

また、Webフォントも複数のアイコンのリクエストをまとめることができますが、Webフォント自体のリクエストが必要になるので、フォントがダウンロードされるまでは、当たり前ですが表示されません。そのため、数種類のパターンで収まることがわかっていればWebフォント化可能でもCSSスプライトでまとめることも考慮すべきです。

なにごともバランスが重要です。