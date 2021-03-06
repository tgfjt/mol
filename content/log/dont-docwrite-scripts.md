---
date: 2012-04-11
title: 【翻訳】document.writeでSCRIPTを書き出すなやで！
subtitle: Don’t docwrite scripts
categories: 
    - translation
excerpt: Don’t docwrite scripts | High Performance Web Sites
---

<cite class="citation">
![Steve Souders](/mol/images/people/steve_souders.jpg)
Original：[Don’t docwrite scripts](http://www.stevesouders.com/blog/2012/04/10/dont-docwrite-scripts/)（<time>2012-04-10</time>）by [Steve Souders](http://stevesouders.com/)
</cite>

昨日のブログ記事の<a href="http://www.stevesouders.com/blog/2012/04/09/making-the-http-archive-faster/">HTTP Archiveが速くなっている</a>、大きな要因の一つとしてはスクリプトローダーを<strong>使用しない</strong>ことです。そのスクリプトローダーとはスクリプトを動的に読み込むためにdocument.writeを使用しているものです。振り返れば、私は2009年4月の<a href="http://www.stevesouders.com/blog/2009/04/27/loading-scripts-without-blocking/">ブロッキングなしのスクリプト読み込み</a>、<a href="http://www.amazon.co.jp/dp/4873114462/">続・ハイパフォーマンスWebサイト(4章)</a>において、document.writeテクニックについて記述していました。それは以下のようなものです。

```javascript
document.write('<script src="' + src + '" type="text/javascript"><\/script>’);
```

document.writeを使ったスクリプトローダーの問題点：
<ul>
	<li>挿入したスクリプトより下の全てのDOM要素はスクリプトのダウンロードが完了するまでレンダリングがブロックされます。 (<a href="http://stevesouders.com/cuzillion/?c0=bi1hfff0_0_f&amp;c1=bj1wfff4_0_f&amp;c2=bi1hfff0_0_f">example</a>).</li>
	<li>また、ほかの動的読み込みもブロックします (<a title="document.write script blocks async script" href="http://stevesouders.com/cuzillion/?c0=hj1wfff2_0_f&amp;c1=bj1dfff2_0_f">example</a>)。例外としては、複数のスクリプトが同一のSCRIPTブロック内にdocument.writeを使用して注入された場合です(<a title="Two document.write scripts in one SCRIPT block" href="http://stevesouders.com/cuzillion/?c0=hj1wfff2_0_f&amp;c1=hj1wfff2_0_f">example</a>)。</li>
</ul>

document.writeを使用したスクリプトローダーのため、私が最適化しようとしたページのレンダリングは遅れ、同じページ内の他の非同期スクリプトに関しても読み込みに時間がかかるようになりました。私はこのスクリプトローダーをはずし、非同期にスクリプトを読み込むために代わりのコードを書きました。それはGoogle アナリティクスの非同期スニペットで有名になった createElement-insertBefore パターンです。

```javascript
var sNew = document.createElement("script");
sNew.async = true;
sNew.src = "http://ajax.googleapis.com/ajax/libs/jquery/1.5.1/jquery.min.js";
var s0 = document.getElementsByTagName('script')[0];
s0.parentNode.insertBefore(sNew, s0);
```

なぜdocument.writeによる非同期読み込みはこのような悪いパフォーマンスになってしまうのでしょうか？

順を追って考えて見れば、そんなに不思議なことではありません。通常のマークアップによるSCRPT SRC=読み込みは後続のDOM要素のレンダリングを止めることを既に私たち知っています。また、スクリプト実行段階を抜ける前にdocument.writeは評価され、その後、ページのパースが再開されることも理解しています。したがって、通常のSCRPT SRC=をスニペットとした document.writeテクニックは残りのページのレンダリングをブロックしてしまいます。

反対に、createElement-insertBeforeテクニックはレンダリングを<strong>ブロックしません</strong>。事実、document.writeでcreateElement-insertBefore スニペットを生成しても、そのときレンダリングはブロックされません。

<a href="http://www.stevesouders.com/blog/2009/04/27/loading-scripts-without-blocking/">ブロッキングなしのスクリプト読み込み</a>記事での私の結論はスクリプトロード手法を選択するための<a href="http://stevesouders.com/efws/images/0405-load-scripts-decision-tree-04.gif">決定木</a>を使用することです。そうすれば異なるシナリオにおいてどの非同期読み込みテクニックを選択するか困らないでしょう。また注意深く見れば、document.writeの使用は決して推奨されないこともお分かりになるでしょう。Webの世界はたいへん多くのものが移り変わりますが、私が2009年に出したアドバイスは今日においても正しいのです。

***

改めて、続・ハイパフォーマンスWebサイトを読んでみるとdocument.writeテクニックはIEのときだけ並列ダウンロードされますと書いてある。しかもスクリプトのダウンロード中は他のリソースはブロックされるとも書いてあり、そもそも使う用途が限定的だなと印象。
