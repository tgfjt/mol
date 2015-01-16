---
date: 2012-12-02
title: パフォーマンスからみるSass/Compass 第2回：CompassによるCSS Sprite
categories: css
---
<blockquote>Sass、Less、StylusなどCSS Preprocessorに関するAdvent Calendarです。
<a href="http://www.adventar.org/calendars/1">CSS Preprocessor Advent Calendar 2012 </a></blockquote>
<ul>
	<li>【1日目】<a href="http://webtech-walker.com/archive/2012/12/less_extend.html">最近のLessのextendの進捗 - Webtech Walker </a></li>
	<li>【3日目】<a href="http://tech.naver.jp/blog/?p=2268">compassのベンダープリフィックス制御 « NAVER Engineers' Blog </a></li>
</ul>
パフォーマンスの勉強ができてなおかつSass/Compassのお勉強にもなるお得なシリーズまさかの2回目。Adventにぶつけてきた！ややもするとシリーズものの2作目というのは駄作になりがちだが、そんなプレッシャーはねのけて乱反射！やっていくYO! これまでの。
<ul>
	<li><a href=" http://t32k.me/mol/log/sass-nesting-and-import/">パフォーマンスからみるSass/Compass 第1回：Nestingと@import</a></li>
	<li><a href="http://t32k.me/mol/log/compass-ie-hex-str-function/">パフォーマンスからみるSass/Compass 番外編：MSは青かった</a></li>
</ul>
<div><!--more--></div>
<h2>CSS Spriteの利点・欠点</h2>
「<a href="http://www.amazon.co.jp/exec/obidos/ASIN/487311361X/warikiru-22/">ハイパフォーマンスWebサイト</a>」（オライリー・ジャパン）の書籍には「高速サイトを実現する14のルール」というものがある。その中でも最も効果のある対策として「HTTPリクエストを減らす」ことを挙げている。HTTPリクエストというのは画像やJavaScriptファイル、CSSファイル等をWebページで読み込む（サーバーにリクエストする）ことを言う。 このリクエストが多ければ多いほど、HTMLページの読み込み速度は遅くなってしまう。フロント（マークアップ）側でできる対策としてはポピュラーなものとしてCSSスプライトという手法があり、これは複数の画像をひとつにまとめ、背景画像としてbackground-positionを調整することで一つ一つの画像として表示する方法だ。

http://www.youtube.com/watch?v=s__XwfwxMW8
<ul>
	<li><a href="http://www.youtube.com/watch?v=s__XwfwxMW8">Minimize HTTP Requests! Requests 30 vs 1 (CSS Sprite) - YouTube</a></li>
</ul>
いかに、CSSスプライトが有用かは上記の動画を見てもらえれば分かるだろう。アイコン画像を30個をimg src=""で読み込む場合と、30個のアイコンを全てひとつの画像にまとめたCSSスプライトで読み込む場合では（環境によっても変化はあるが）3倍の差が出た。<span style="color: #888888;">（3倍だと！？そいつはシャアだ。赤い彗星だ！）</span>
<ul>
	<li><a href="http://t32k.github.com/speed/rtt/SpriteImages.html"><strong>CSSスプライトに画像をまとめる | LPWS</strong></a></li>
</ul>
どのような画像をまとめたらよいのかは上記のリソースを参考にすればよい。 ただ、CSSスプライトにも問題がある。それは管理が非常に面倒な点だ。まずひとつひとつの画像をPhotoshopなどの画像編集ツールでまとめる必要あり、またその配置した画像の座標をbackground-positionに反映させる必要がある。
<ul>
	<li><a href="http://ja.spritegen.website-performance.org/"><strong>CSS Sprite Generator | Project Fondue</strong></a></li>
</ul>
CSS Sprite Generator のような便利なオンラインツールが存在するが、画像の変更があった場合、再度オンラインにアップロードしてコードをジェネレートする必要があったり、Retina対応を考えると自分で画像のサイズを半分にして計算しないといけない。 これでは、サービスリリース後などの度重なる改善・修正にマークアップが追いつかず不必要な画像までもそのままスプライト画像に残してしまっ た状態で無駄な転送量が発生してしまう。 実際に私がやっている案件でもグローバルナビゲーションや、カテゴリナビゲーションなどにスプライト画像を使用しHTTPリクエストを最小限に抑えている。リリース当時はきれいにまとまったスプライト画像も度重なるサービス改善で、スプライト画像のカテゴリアイコンがなくなったり、追加したりと何度も手入れをしなければならないことが発生し、その度に画像の作成・位置の再計算など非常に面倒な作業だった。<span style="color: #888888;">（李徴はとうとう発狂した！）</span>
<h2>Compassによる解決策</h2>
そこで目につけたのが、Compassフレームワークだ。Sassは開発当初から利用していたが、Compass自体は利用していなかった。Compassにはスプライトを簡単に利用できるAPIが備わっており、画像編集ソフトなど使わなくても画像を一つにまとめたり、それぞれのbackground-positionを算出してくれる。 使い方は非常にシンプルだ。スプライト画像にまとめたい個々の画像を任意のディレクトリに格納しておき、<em>@import</em>でそのディレクトリを指定し、<em>@include all-ディレクトリ名-sprites</em>で展開するだけだ。
<pre><code class="css">//　基本的なCompassスプライトの利用方法
@import "my-icons/*.png";
@include all-my-icons-sprites;</code></pre>
上記のSCSSコードは下記のようにコンパイルされる。
<pre><code class="css">/* コンパイルされたCSS */
.my-icons-sprite, .my-icons-fav, .my-icons-hist, .my-icons-my, .my-icons-new {
	background: url('/images/my-icons-sab4abd7554.png') no-repeat;
}
.my-icons-fav { background-position: 0 -68px; }
.my-icons-hist { background-position: 0 -136px; }
.my-icons-my { background-position: 0 -204px; }
.my-icons-new { background-position: 0 0; }</code></pre>
また上記のコードではmy-iconsディレクトリ以下にある個々の画像をひとつのスプライト画像にまとめてくれる。 <img class="aligncenter size-full wp-image-4465" title="compile" alt="" src="/static/blog/2012/12/compile.png" width="900" height="300" />ただ、基本的な利用方法ではRetina対応や、自分のコードスタイルに合わず、独自でmixinを作成する必要があった。
<pre><code class="css">//　自分で定義したmixin
@mixin sprites($map, $map-item, $isCommon:false) {
     @if $isCommon {
          background-image: sprite-url($map);
          background-repeat: no-repeat;
          background-size: round(image-width(sprite-path($map)) / 2) round(image-height(sprite-path($map)) / 2);
     } @else {
          $pos-y: round(nth(sprite-position($map, $map-item), 2) / 2);
          width: round(image-width(sprite-file($map, $map-item)) / 2);
          height: round(image-height(sprite-file($map, $map-item)) / 2);
          background-position: 0 $pos-y;
     }
}</code></pre>
実際の使い方
<pre><code class="css">// mapを指定する
// $spacingの引数で個々の画像のマージンを指定できる
// この値がなければぴっちり縦に並ぶことになる
$map-tabs: sprite-map("/files/img/sprites/tabs/*.png",$spacing: 4px);

// 共通プロパティのextend
%tabs { @include sprites($map-tabs, common, true); }

// 共通プロパティ指定
i {
	@extend %tabs;
	position: relative;
	display: block;
	margin: 0 auto;
}

// 個別プロパティ指定
.tab-new i { @include sprites($map-tabs, new, false); }
.tab-fav i { @include sprites($map-tabs, fav, false); }
.tab-hist i { @include sprites($map-tabs, hist, false); }
.tab-mypage i { @include sprites($map-tabs, my, false); }
.tab-new.active i { @include sprites($map-tabs, new-on, false);}
.tab-fav.active i { @include sprites($map-tabs, fav-on, false); }
.tab-hist.active i { @include sprites($map-tabs, hist-on, false); }
.tab-mypage.active i {@include sprites($map-tabs, my-on, false); }</code></pre>
上記のコードでは、実際の画像ファイルの縦・横サイズを半分にして計算し、Retina対応をしている。等倍の画像ファイルも用意するのであればメディアクエリーを利用し分岐処理のコードを追加すれば良い。

このコードの肝というか、個人的に重要だと思っているのはこの<em>image-width(sprite-path($map)</em>部分である。これはスプライトした画像のパスを取り、そのwidthを返すものだ。通常のCSSやSassではこのようにファイルアクセスしそのファイルの情報を得るということはできなかった。故に、人力でbackground-positionを割り出すという面倒なことをやっていたのだ。

これより、非常に簡単にCSSスプライトの画像を管理できるようになった。なぜならスプライトしたい画像をまとめたディレクトリに画像を追加・削除・修正することで、Compassがそれを検知し自動的にスプライト画像を再生成し、background-positionの値をコードに反映してくれる。 これにより度重なるトライ・アンド・エラーに対して、時間的・モチベーション的にも余裕をもって改善のイテレーションを回せるようになった。
<ul>
	<li><a href="http://compass-style.org/reference/compass/helpers/sprites/">CSS Sprite Helpers for Compass | Compass Documentation</a></li>
	<li><a href="http://compass-style.org/help/tutorials/spriting/">Spriting with Compass | Compass Documentation</a></li>
</ul>
<a href="/static/blog/2012/12/sprites.png"><img class="alignleft size-thumbnail wp-image-4464 fig" title="sprites" alt="" src="/static/blog/2012/12/sprites-150x150.png" width="150" height="150" /></a>また気をつけなければならないことに、簡単にCSS Spriteできるようになったからといってすべての画像をスプライト画像にまとめることがないようにしたい。可能な限り、HTTPリクエストを減らすことは良いことだが、何事も限度がある。たとえばサイトで使うすべての画像をまとめてしまえば、そのどれか一つでも更新があれば再度リクエストしなければならない。これはキャッシュ保持できる期間が短くなってしまうことを意味し、結果的に応答性の悪いサイトになってしまう。ヘッダー画像のような大きな画像も含めば、初期ロード時にかなり待たなければならず体感速度的にも大きな影響がある。またそのような大きな画像と上記のような小さなアイコンが縦に並べば、空白の部分が多く生じる。ホワイトスペース部分はファイルサイズに影響しないが画像を表示するのにメモリを多く必要としてしまうため、これは非力なモバイルデバイス端末では無視できない事象である。

そのため、私は種類ごと、どのページでその画像が呼び出されるのかといった基準でスプライト画像を分けて管理している。タブバーで使われるものはtabsディレクトリ、矢印系のものはarrowsディレクトリに分けてなど、スプライト画像を作成している。
<h2>さらなる最適化のために<span style="color: #888888;">（ピリオドの向こうへ）</span></h2>
スプライト画像のPNGはCompassによって生成されるわけだが、これはRubyライブラリのchunky_pngを使用している。何ぶん画像を生成するので重たい処理だ。Cで書かれた拡張機能のoily_pngをインストールすると生成を早くすることができる。インストールは簡単だ。 <code>gem install oily_png</code> を打つだけで、あとはCompassが自動で認識してくれる。

またまた、Compassで生成された画像はもちろん最適化されていないので<a href="http://imageoptim.com/">imageOptim.app</a>などで最適化する必要がある。imageOptimをかけると、中には半分くらいファイルサイズを減量させることも可能で、この手間をかけるのとかけないのでは大きな違いだ。ただ、スプライトの画像を何度も追加・削除・修正するたびにimageOptim.appを起動し、ドラッグ・アンド・ドロップなんて真似事はめんどくさいの極みである。<span style="color: #888888;">（二重の極み！あっーーー！） </span>

そこでGruntである。<a href="http://havelog.ayumusato.com/develop/javascript/e514-grunt_arrange_task.html"><strong>Gruntについては@ahomuブログを参照してもらえると良いだろう</strong></a>。gurnt deployといった具合にリリースする前にかますオリジナルタスク、またはwatchタスクに下記のような画像最適化タスクを入れておけば自動的に最適化してくれる。
<ul>
	<li><a href="https://github.com/heldr/grunt-img">grunt-img</a></li>
	<li><a href="https://github.com/sapegin/grunt-imgo">grunt-imgo</a></li>
	<li><a href="https://github.com/gruntjs/grunt-contrib-imagemin">grunt-contrib-imagemin</a></li>
</ul>
いかがだっただろうか。CSS Spriteという一見複雑な作業も1つ1つのタスクに分けて考えれば機械にできることなので、そういったものはすべて機械に任せるようにし人間はもっとクリエイティブなことに集中したほうが良い。幸いなことに、Sass/CompassやGruntのように機械と対話する環境は以前よりもずっと整備されてきているので利用しない手はない。そう、この波に乗るっきゃない(・ω&lt;)

この続きは・・・
<ul>
	<li><strong><a href="http://lp26.cssnite.jp/ ">CSS Nite LP, Disk 26「CSS Preprocessor Shootout」 </a>で！！</strong></li>
</ul>