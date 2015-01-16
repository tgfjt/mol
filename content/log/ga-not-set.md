---
date: 2010-09-30
title: Google Analytics (not set) エントリの異常増加
categories: analytics
---

最近、<strong><span style="color: #888888;">ユーザー</span></strong> &gt; <strong><span style="color: #888888;">PC環境</span></strong><span style="color: #888888;"> </span>&gt; <strong><span style="color: #888888;">画面の解像度</span></strong> のメニューにおいて、 (not set) エントリが異常に多いことを同僚から指摘されました。調べてみると、一部のプロファイルでそのような現象を確認することが出来ました。

<!--more-->

Google Analytics で (not set)  と言えば、<a href="http://analytics-ja.blogspot.com/2010/02/not-set-entries.html">キャンペーントラッキングでのキーワード</a>問題が有名ですね。キーワードが取れていないから (not set) であるので、<strong><span style="color: #888888;">画面の解像度</span></strong><strong> </strong>においても解像度が取れてないのだろうと考えましたが、全体の中で10％も (not set) というのは、ちょっとどうゆう状況なのか考えらませんでした。数件、数十件であれば、トラッキングコード実行時になんらかのエラーがあって取れないということも考えられるでしょうが、今回の場合は多すぎます。

<a href="/static/blog/2010/09/screenresolution.png"><img class="alignnone size-full wp-image-1701" title="Screen Resolution" src="/static/blog/2010/09/screenresolution.png" alt="" width="470" height="379" /></a>

そこで、年次スパンで (not set) エントリの推移を見てみますと、どうやら5/3から急激に増加しているのが分かります。ということで、該当の期間にトラッキングコード、フィルタの変更を行ったかといえば、それも思い当たるふしがありません。

ということで、他のGAユーザーも同じ問題になっていないのか検索してみました。たいていは、<a href="http://www.comedywaltz.com/search/a2iforum/">アクセス解析Q＆Aフォーラム</a>を調べれば分かるのですが、分からないこともあります。

そんなときは、<a href="http://www.google.com/support/forum/p/Google+Analytics?hl=en">Google Analytics  Help forum</a> のほうを検索してみましょう。
<ul>
	<li><a href="http://www.google.com/support/forum/p/Google+Analytics/thread?tid=1f5631b2c79a2bf5&amp;hl=en">Huge increase in Screen Resolution/Colors of (Not Set) - Google Analytics Help</a></li>
</ul>
検索してみると、どうやら上記の質問と似ているように思えます。時期的にも同じだし、症状も似ている。この人は <strong><span style="color: #888888;">画面の解像度</span></strong><strong> </strong>だけでなく、<strong><span style="color: #888888;">画面の色</span></strong><strong> </strong>でも (not set) エントリが出ているみたい。そこで、自分のプロファイルを再度確認したところ、<strong><span style="color: #888888;">画面の色</span></strong><strong> </strong>においても同じ現象が確認できました。その他にも(not set)  エントリの異常増加は、自分のプロファイルでは、下記メニューで確認できました。
<ul>
	<li>画面の解像度　（Screen Resolutions）</li>
	<li>画面の色　（Screen Colors）</li>
	<li>フラッシュのバージョン　(Flash Versions）</li>
	<li>言語　（Languages）</li>
	<li>ホスト名 （Hostnames） <span style="color: #888888;">※これは増加ではなく急激な減少でした。</span></li>
</ul>
そんわけでいろいろ調べた結果、なんかGA自体のバグくさいので、今回はGooogle さんに問い合わせてみようと思いました。実はちゃんとGoogle Analytics のお問い合わせフォームが存在してまして、調べてみてどうしてもわからない場合は問い合わせてみるのも良いでしょう。（要：AdWordsアカウント）
<ul>
	<li><a href="https://www.google.com/support/googleanalytics/bin/request.py?cf_question=qc_5_1">https://www.google.com/support/googleanalytics/bin/request.py?cf_question=qc_5_1</a></li>
</ul>
問い合わせた結果、この問題について既に認識しており、現在、エンジニアが問題の解決に向けて鋭意対応中ではあるが、改善時期は未定とのことです。

Google Analyticsでは、以前にも<a href="http://blog.livedoor.jp/seo24/archives/1672067.html">ナビゲーション サマリーにおいて離脱数が0%になるバグ</a>が発生していました。（現在は修正済）　そういったバグは他にもあるかもしれませんので、こういった問題は僕らにはどうしようもできないことです。つまり、何を言いたいかと言いますと、バグ見つけたら記事にしてGoogleに問い合わせて共有しようぜってことです。3人よれば文殊の知恵ですがな。