---
date: 2013-06-29
title: OOCSSをbuildする
categories:
- css
---

ちょいーんっす！あたーんっす！

みなさん、<a href="http://oocss.org/">OOCSS</a>って知ってますか？僕は知らないです。嘘です。

<a href="https://twitter.com/stubbornella">@stubbornella</a>が、4,5年前に言い出したオブジェクト指向なCSSの考え方みたいなもんです。CSSでこんなもの表現できた！とかそうゆうTips系の話題は事欠かないのですが、CSSの設計についての話はそれ以前には（今もそうですが）あまりなかったので、個人的にはとても衝撃的でした。
<ul>
	<li><a href="http://www.slideshare.net/hiloki/a-good-css-and-sass-architecture">ちゃんとCSSを書くために - CSS/Sass設計の話</a>（あ、最近イカス資料見つけたぞ！）</li>
</ul>
まぁそんなOOCSSですが、ここ一年は目立った更新はありませんでした。と思ってたら最近、<a href="https://github.com/stubbornella/oocss">GitHubのレポジトリ</a>見たらなんかすごく変わってる！

新しくなったドキュメントを読みたいのですが、<a href="https://github.com/stubbornella/oocss/tree/master/oocss">Readme</a>を読むと、どうやらSassやらHandlebarsやら使っててbuildしなきゃいけないっぽいす。んで仮想環境をVagrantで提供してるとのこと。

なんと！俺はただCSSのドキュメントを読みたいだけなのに、めんどくせ(๑･ิω･ิ๑)yー～

と言っててもしょうがないのでがんばってみる。
<h2>事前インストール</h2>
<ul>
	<li><a href="http://git-scm.com/downloads">Git</a> (多分入ってると思うけど...)</li>
	<li><a href="https://www.virtualbox.org/wiki/Downloads">VirtualBox</a> (現状、4.2.14やめて、4.2.12を選択しとく)</li>
	<li><a href="http://downloads.vagrantup.com/">Vagrant</a> (gemでインスコしたことある人はアンスコしとく)</li>
</ul>
<h2>OOCSSをbuild</h2>
<p style="text-align: center;"><img class="aligncenter  wp-image-4963" src="/static/blog/2013/06/4f7c94e2ab3fb1d22b3c581a01721d3b.png" alt="" width="900" /></p>
<a href="https://github.com/stubbornella/oocss">OOCSS</a>をForkする(フォークボタン押すだけね！)

<pre><code class="bash">git clone https://github.com/{user_name}/oocss.git

Cloning into 'oocss'...
remote: Counting objects: 1800, done.
remote: Compressing objects: 100% (1261/1261), done.
remote: Total 1800 (delta 580), reused 1626 (delta 445)
Receiving objects: 100% (1800/1800), 1.53 MiB | 121.00 KiB/s, done.
Resolving deltas: 100% (580/580), done.</code></pre>

ForkしたのをCloneしてくる

<pre><code class="bash">cd oocss/oocss</code></pre>

Vagrantfileのある場所に移動する

<pre><code class="bash">$ vagrant up

Bringing machine 'default' up with 'virtualbox' provider...
[default] Box 'stubbornella' was not found. Fetching box from specified URL for
the provider 'virtualbox'. Note that if the URL does not have
a box for this provider, you should interrupt Vagrant now and add
the box yourself. Otherwise Vagrant will attempt to download the
full box prior to discovering this error.
Downloading or copying the box...
Extracting box...te: 206k/s, Estimated time remaining: 0:00:01))
Successfully added box 'stubbornella' with provider 'virtualbox'!
[default] Importing base box 'stubbornella'...</code></pre>

vagrant upで起動させる

<pre><code class="bash">$ vagrant ssh</code></pre>

vagrant sshで中に入る

<pre><code class="bash">vagrant@lucid32:/vagrant$ make build

##################################################
Clean the project
##################################################
Removing the build directory
cleaning compass files...
Remove Node modules
##################################################
Clean project                               ✔ Done
##################################################

##################################################
Building OOCSS...
##################################################


##################################################
Building CSS Files with Sass...

##################################################
Building Documentation...
##################################################

Build done at : Fri Jun 28 2013 20:36:15 GMT+0200 (CEST)

##################################################
##################################################

OOCSS Build                                 ✔ Done
##################################################</code></pre>

makeする

<a href="http://localhost:8080/docs/library.html">http://localhost:8080/docs/library.html</a> にアクセスしてドキュメントみる。わーい見れた！
<p style="text-align: center;"><a href="/static/blog/2013/06/b37d8e01bd98623ba758276cde9747da.png"><img class="aligncenter  wp-image-4964" src="/static/blog/2013/06/b37d8e01bd98623ba758276cde9747da.png" alt="" width="900" /></a></p>
ほらね、簡単でしょ？

<pre><code class="bash">$ vagrant halt</code></pre>
ちなみにvagrant haltでVMを停止させることができます。
