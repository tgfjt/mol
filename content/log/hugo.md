---
categories: blog
date: 2015-01-16T09:44:52-08:00
excerpt: Hugoに移行しました。
ogimage: /mol/images/2015/0115-00.png
subtitle: Hugo - A fast and modern static website engine
title: Jekyllが許されるのは小学生までだよね
---

[![Hugo](/mol/images/2015/0115-01.png)](http://gohugo.io/)

タイトルは釣り！これまでJekyllを使ってブログを書いてたけど[Hugo](http://gohugo.io/)に移行したよという話。

+ [OctopressからHugoへ移行した | SOTA](http://deeeet.com/writing/2014/12/25/hugo/)

[Frontend Weekly Vol.0](/mol/log/frontend-weekly/)で知ったんだけど、これ見ていいなーって思ったのでやってみた。

## これまでの遍歴

+ __Blogger__  
　Webサービスを使うのはよいことだ。最初にはてなを使ってたら僕の人生変わってたかもしれない。
+ __WordPress + さくらサーバー__  
　僕は若かった。いったい誰が僕のことを責めれようか。いや誰もいない。
+ __Jekyll + GitHub Pages__  
　Markdown最高ww
+ __Hugo + GitHub Pages__  
　イマココ。

## Jekyllのいやなところ

__遅い！__コレに尽きる。上記のように、7年ほどブログを続けていると記事数が450くらいになってた。`jekyll serve --watch`して、記事を変更しても体感的に10秒位かかって更新されるような状態だった。ほんの少しスタイルを変更して確認したいだけなのに何秒も待たされるのは本当にイライラする。限界だ。

+ [Jekyll Incorporated - Modern Blog for Companies](http://incorporated.sendtoinc.com/)

あと、これは別にJekyllのせいじゃないけど、僕の使ってたテーマがRuby 1.9固定だったので、なんでブログ書く度にRubyのバージョンを変えなきゃいけないのか、本当にイライラする。限界だ。

## Hugoのすきなところ

__速い！__コレに尽きる。ほぼリアルタイムで更新されるんじゃないかくらい速い。`hugo server --watch`したら、デフォルトでLiveReloadで更新されるのも地味に便利である。

移行にあたって、いらない記事とか捨てた結果240記事になった。__0.2秒__かからないくらいで生成される。

```shell
$ time hugo
0 draft content
0 future content
240 pages created
0 tags created
12 categories created
in 119 ms

real	0m0.172s
user	0m0.367s
sys	0m0.108s
```
対して、Jekyllは__2.5秒__!!  
Hugoとくらべて14倍時間がかかってる。

```shell
$ time jekyll build
Configuration file: _config.yml
            Source: .
       Destination: ./_site
      Generating...
                    done.
 Auto-regeneration: disabled. Use --watch to enable.

real	0m2.458s
user	0m1.826s
sys	0m0.323s
```

そんなわけで、やっぱりGolang製は速いなと。HugoはGolang製だけどHugoを使う分には別にGoを勉強する必要はない。`brew install hugo`でインスコして、JekyllのようにMarkdownを書いていくだけだ。テンプレートの記法もJekyllに似てるので問題ない。

+ [Introduction to Hugo](http://gohugo.io/overview/introduction/)

あと、[Jekyllからの移行方法](http://gohugo.io/tutorials/migrate-from-jekyll/)も丁寧に記載されてるし、masterにPushするだけであとは[Wreckerでgh-pagesにPushしてくれるビルド](http://gohugo.io/tutorials/automated-deployments/)も用意されているので非常に楽である。

+ [t32k/mol](https://github.com/t32k/mol)

みなさんもよかったらどうぞ。


