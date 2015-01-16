---
date: 2009-06-11
title: Cookieいろいろ〜！
categories: analytics
---
Cookieが嫌いです。カントリーマアムは好きですが。。。
などと言っておられなくなってきたので、本腰入れて調べてみた。
Google Analyticsを理解するには避けられない道なのだよ(´・ω・｀)

まずは、基本的な意味から
<blockquote><span style="font-size: 85%;">Cookieとは、Webサイトの提供者が、Webブラウザを通じて訪問者のコンピュータに一時的にデータを書き込んで保存させるしくみ。
Cookieにはユーザに関する情報や最後にサイトを訪れた日時、サイトの訪問回数など記録しておくことができる。Cookieはユーザの識別に使われ認証システムやWWWによるサービスをユーザごとにカスタマイズするパーソナライズシステムの要素技術として利用される。
1つのCookieには4096バイトのデータを記録でき、最大で300のCookieを保存できる。1台のサーバが同じコンピュータに対して発行できる Cookieの数は20個に制限されている。Cookieにはそれぞれ有効期限を設定することができ、有効期限を過ぎたCookieは消滅する。</span>
<div style="text-align: right;"><span style="font-size: 85%;"><a href="http://e-words.jp/w/Cookie.html">Cookieとは 【クッキー】 - 意味/解説/説明/定義 ： IT用語辞典</a></span></div></blockquote>
<a href="http://www.amazon.co.jp/gp/product/4873113024?ie=UTF8&amp;tag=warikiru-22&amp;linkCode=as2&amp;camp=247&amp;creative=7399&amp;creativeASIN=4873113024">Web解析Hacks</a>より、もうすこしアクセス解析よりな視点からの説明
<blockquote><span style="font-weight: bold; color: #000000; font-size: 100%;">セッション Cookie</span>
セッション Cookieは訪問者がサイトにいる間だけ有効で、ユーザがWebブラウザを閉じたり。ある一定時間（通常は30分間）何もしなかったりすると、削除される。

<span style="font-weight: bold; color: #000000; font-size: 100%;">永続 Cookie</span>
永続 Cookieは１回の訪問を超えて存在し続け、将来のある時間に有効期限がくる。設定方法によってファーストパーティとサードパーティの２つに分類できる。

<span style="font-weight: bold; color: #000000; font-size: 85%;">ファーストパーティ Cookie</span>
企業自身が所有するWebサーバーやドメインから発行されたCookie。例えば、eBayが独自のトラッキングCookieをebay.comから設定している場合、これらのCookieは『ファーストパーティ』である。

<span style="font-weight: bold; color: #000000; font-size: 85%;">サードパーティ Cookie</span>
アクセスしたWebサイト以外のドメインから発行されたすべてのCookie。例えば、DisneyがWebSideStoryのトラッキングドメイン（www.disney.comのWebサイト上のehg-disney.htbox.com）を使っていた場合、WebSideStoryの Cookieはサードパーティ Cookieである。</blockquote>
セキュリティと精度の点から言ってファーストパーティ Cookieの仕様が望ましいのだ。
ちなみに、<a href="http://www.google.com/support/analytics/bin/answer.py?hl=jp&amp;answer=55614">Google Analyticsはファーストパーティ Cookie</a>を使用している。

あと、<a href="http://code.google.com/intl/en/apis/analytics/docs/concepts/gaConceptsCookies.html#cookiesSet">Cookies Set By Google Analytics</a>ってところにGoogle Analyticsで使われているCookieの説明があったので訳してみた。逆に意味わかんなくってるけどごめん。
<h3>__utma</h3>
このクッキーは通常、初めてあなたのサイトを訪れるユーザーのブラウザにセットされる。もし、ブラウザ操作によってクッキーを削除したのなら、そしてそれ以降にサイトを訪れたとしたら、新しくて異なるユニークIDをもつ__utmaクッキーがセットされる。このクッキーはユニークユーザーを決めるのに使用され各ページを見るごとにアップデートされる。また、このクッキーはGoogle Analyticsが使用するユニークIDを持つことで正当性とクッキーへの安全なアクセス性を保証する。

<span style="font-size: 85%;">有効期限：セット、上書きされてから2年</span>
<h3>__utmb</h3>
このクッキーはあなたのサイトでのユーザーセッションを確認するために使用される。ユーザーがページを見たときに、Google Analyticsコードはこのクッキーをアップデートしようとする。もしこのクッキーが見つからない場合は、新しくセットされ新たなセッションとしてカウントされる。あなたが違うページを見るごとに30分の有効期限のクッキーが上書きされる、つまり30分以内の間隔でユーザーが活動する限り、１つのセッションが続くということです。このクッキーは30分以上ページでなにも活動しなかった場合、期限が切れてしまいます。あなたは _setSessionTimeout()メソッドを使用することでデフォルトのセッション時間を変更することができます。

<span style="font-size: 85%;">有効期限：セット、上書きされてから30分</span>
<h3>__utmc</h3>
このクッキーは__utmbクッキーと共同して新しいユーザーセッションかどうか決める。特に有効期限はないがブラウザが終了したときが期限になる。ユーザーはブラウザを終了させて30分以内にまたサイトを訪れることもあるだろう。このときに__utmcがないことは__utmbクッキーの有効期限内であるけれども新しいセッションであることを物語っている。

<span style="font-size: 85%;">有効期限：セットされていない</span>
<h3>__utmz</h3>
このクッキーはユーザーがあなたのサイトを訪れた時に、それがノーリファラーなのか、参照サイト経由なのか、検索で来たのか、それとも広告やEmailなどのキャンペーンなのかなどの情報を記録する。そして検索エンジントラフィックや、広告キャンペーン、ページ遷移の計算に使われる。またこのクッキーの有効期限は各ページを見るごとにアップデートされる。

<span style="font-size: 85%;">有効期限：セット、上書きされてから6ヶ月</span>
<h3>__utmv</h3>
このクッキーは通常のトラッキングコード設定では存在しない。__utmvクッキーはあなたが独自設定した_setVar()メソッドを使うことで表示される。このクッキー情報はutmccパラメーター経由のGIFリクエストURLでAnalyticsサーバーに受け渡される。このクッキーは _setVar()メソッドが追加されたページをロードすることでしか上書きされない。

<span style="font-size: 85%;">有効期限：セット、上書きされてから2年</span>
<h3>__utmx</h3>
このクッキーはWebsite Optimizerに使用され、正確に設定されたWebsite Optimizerトラッキングコードが挿入されたをページをロードしたときしかセットされない。Optimizeコードが実行するとき、訪問者の種類を表すこのクッキーは各実験に割り当てられるので、訪問者はあなたのサイトで一貫した体験をする。詳しくは<a href="http://www.google.com/support/websiteoptimizer/">Website Optimizer Help Center</a>を見てね。

<span style="font-size: 85%;">有効期限：セット、上書きされてから2年</span>

これで何となく、アクセス解析でのCookieの役割が分かったような（´・ω・｀）
<table border="0" cellpadding="5">
<tbody>
<tr>
<td valign="top"><a href="http://www.amazon.co.jp/exec/obidos/ASIN/4873113024/warikiru-22/ref=nosim/" target="_blank"><img class="fig" style="border: 0pt none;" src="http://ecx.images-amazon.com/images/I/41ZCARQCYDL._SL160_.jpg" border="0" alt="Web解析Hacks ―オンラインビジネスで最大の効果をあげるテクニック &amp; ツール" width="111" height="160" /></a></td>
<td style="font-weight: bold;" valign="top"><span style="font-size: 85%;"><a href="http://www.amazon.co.jp/Web%E8%A7%A3%E6%9E%90Hacks-%E2%80%95%E3%82%AA%E3%83%B3%E3%83%A9%E3%82%A4%E3%83%B3%E3%83%93%E3%82%B8%E3%83%8D%E3%82%B9%E3%81%A7%E6%9C%80%E5%A4%A7%E3%81%AE%E5%8A%B9%E6%9E%9C%E3%82%92%E3%81%82%E3%81%92%E3%82%8B%E3%83%86%E3%82%AF%E3%83%8B%E3%83%83%E3%82%AF-%E3%83%84%E3%83%BC%E3%83%AB-Eric-Peterson/dp/4873113024%3FSubscriptionId%3D0G91FPYVW6ZGWBH4Y9G2%26tag%3Dwarikiru-22%26linkCode%3Dxm2%26camp%3D2025%26creative%3D165953%26creativeASIN%3D4873113024" target="_blank">Web解析Hacks </a>
<a href="http://www.amazon.co.jp/Web%E8%A7%A3%E6%9E%90Hacks-%E2%80%95%E3%82%AA%E3%83%B3%E3%83%A9%E3%82%A4%E3%83%B3%E3%83%93%E3%82%B8%E3%83%8D%E3%82%B9%E3%81%A7%E6%9C%80%E5%A4%A7%E3%81%AE%E5%8A%B9%E6%9E%9C%E3%82%92%E3%81%82%E3%81%92%E3%82%8B%E3%83%86%E3%82%AF%E3%83%8B%E3%83%83%E3%82%AF-%E3%83%84%E3%83%BC%E3%83%AB-Eric-Peterson/dp/4873113024%3FSubscriptionId%3D0G91FPYVW6ZGWBH4Y9G2%26tag%3Dwarikiru-22%26linkCode%3Dxm2%26camp%3D2025%26creative%3D165953%26creativeASIN%3D4873113024" target="_blank">―オンラインビジネスで最大の効果をあげるテクニック &amp; ツール</a><img src="http://www.blogger.com/%27http://www.assoc-amazon.jp/e/ir?t=" border="0" alt="''" width="1" height="1" />
株式会社デジタルフォレスト 木下 哲也 有限会社 福龍興業

オライリー・ジャパン  2006-11-08
売り上げランキング : 68060
おすすめ平均  <img src="http://g-images.amazon.com/images/G/01/detail/stars-4-5.gif" alt="" />

<a href="http://www.amazon.co.jp/Web%E8%A7%A3%E6%9E%90Hacks-%E2%80%95%E3%82%AA%E3%83%B3%E3%83%A9%E3%82%A4%E3%83%B3%E3%83%93%E3%82%B8%E3%83%8D%E3%82%B9%E3%81%A7%E6%9C%80%E5%A4%A7%E3%81%AE%E5%8A%B9%E6%9E%9C%E3%82%92%E3%81%82%E3%81%92%E3%82%8B%E3%83%86%E3%82%AF%E3%83%8B%E3%83%83%E3%82%AF-%E3%83%84%E3%83%BC%E3%83%AB-Eric-Peterson/dp/4873113024%3FSubscriptionId%3D0G91FPYVW6ZGWBH4Y9G2%26tag%3Dwarikiru-22%26linkCode%3Dxm2%26camp%3D2025%26creative%3D165953%26creativeASIN%3D4873113024" target="_blank">Amazonで詳しく見る</a><span style="font-size: 85%;"> </span><span style="font-size: 85%;">by <a href="http://www.goodpic.com/mt/aws/index.html">G-Tools</a></span>

</span></td>
</tr>
</tbody>
</table>