---
date: 2014-05-16
title: Sketch3ハジメマシタ
categories: 
    - design
cover_image: "/2014/05-16-cover.png"
excerpt: "こんちわ、無職のt32kさんだよ。今日は職を失ってお金もないのでAdobe Creative Suiteに代わるソフトを探そうって話だ。"
---

こんちわ、無職のt32kさんだよ。今日は職を失ってお金もないのでAdobe Creative Suiteに代わるソフトを探そうって話だ。そうゆうことで、いま世間を騒がしている[Sketch.app](http://bohemiancoding.com/sketch/)を触ってみたよ。

<div class="fluid"><iframe src="//player.vimeo.com/video/93348231" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe></div>

Sketchってどんなソフトかはこもりさんの紹介動画を見てみるとよい。ついでに、こもりさんが書いている『Sketch 3 Book for Beginner』電子書籍も買っておくとスタートダッシュが決めれると思う。

+ __[Sketch 3 Book for Beginner w/ Video](https://gumroad.com/l/QOto)__

電子書籍ならではというか、Sketch.app自体の更新も速いので、電子書籍も随時アップデートされているし、説明動画もついているので分かりやすい。

そうゆうわけで、5/13に[こけむさズ](http://kokemusazu.com/)で行われたSketch3勉強会にも行ってきた。勉強会は最初こもりさんからSketch.appの概要を説明してもらって、後半はこもりさんの画面操作を見ながら自分でも動かしてみるってスタイルだった。なにせ、[Content-generator-sketch-plugin](https://github.com/timuric/Content-generator-sketch-plugin)ハンパない！

そうゆうわけで、やはりこれは来る！というSublime Textが出てきた頃のような感覚を得た。特に僕が気に入ったのは以下の2つの点だ。

+ Keynote.appっぽい
+ エコシステムができてる

## Keynote.appっぽい

普段一番使用しているソフトといえばSublime Textだろうか。まぁテキストエディタは使うよね、ってことで次に使うソフトはプレゼンなどで重宝しているKeynote.app。プレゼンだけでなく簡単な企画書的なものを作成するのにも使っているので、このインターフェイスに僕は慣れている。

![](/mol/images/2014/05-16-fig02.png)

たいして、Sketch.appはどうだろう。UIはクリソツである。それもそのはずで、SKetch.appはQuartzであったりCocoaであったりMacの技術に強く依存しているので、Mac標準ソフトとUIが同じである。

> __Is Sketch available for Windows or Linux__  
Sketch relies on a lot of technology that is exclusive to OS X and the fact that no other OS provide a clear business model for software development, we're not considering supporting it.

+ [Bohemian Coding - Default](http://bohemiancoding.com/sketch/support/faq/02-general/5-windows.html)

こんなこと言ってるくらいなのでWindows用とか当分出てこないよね。。。

まぁKeynote.appはあくまでプレゼンソフトなのでレイアウト機能が弱かったりするが、Sketch.appはそこを上手くカバーしている。Make Gridであったり、Show Layoutなどは欠かせない機能といえるだろう。SymbolとかShared Style、Shared Textみたいなスタイルが共有されるシステムも有していてすごく便利である。

世間的には、Illustratorの代替であったりFireworksの代替と表されるSketch.appであるが、個人的にはKeynote.appの延長線上だと認識するのが一番しっくりきている。

Fireworksは使ったことないから分かんないけど、やっぱりパスのコントロールとかは細かいとこでは現時点ではIllustoratorのほうが使いやすかったりする。けどもKeynote.appの延長だと考えれば上出来な結果と言える（お値段もほどほどだし）、さらにWebサイト制作に便利な機能も豊富だし満足している。最近のフラットデザインっぽいシンプルなデザインのサイト・アプリならSketch3の機能で十分まかなえるじゃないかな。

### エコシステムができてる

足りない機能、IllustratorでできるのにSketch3でできないの？って機能はプラグインを探してみるといいだろう。

+ [sketchplugins/plugin-directory](https://github.com/sketchplugins/plugin-directory)
+ [Sketch App Plugin Registry - Sketchpack.in](http://sketchpack.in/)

GitHubにいっぱいまとめてある（レジストリも最近できた！）。

+ [こもりプラグインリスト](http://www.mkdown.com/ccfa37b1bf412aca28c8)

ちなみにこもりさんお墨付きのプラグインリストは上記。プラグインだけじゃなくて、自分たちが作ったテンプレートであったりUIコンポーネントも投稿できるサイトがある。

![](/mol/images/2014/05-16-fig01.png)

+ [Sketch App Sources - Free graphical resources](http://www.sketchappsources.com/)

オープンソースとまで言わないが、比較的安価なソフトなのでいろんな人が使って便利なものやクールなものを共有していて、エンジニアっぽい文化が好きだ。

+ [Keyboard Shortcuts for Sketch App](http://sketchshortcuts.com/)
+ [Import web pages into Sketch — .Sketch App — Medium](https://medium.com/sketch-app/6681ae0b118a)

こんなのもある（ショートカットバンバン使いこなしたい）。

+ [Bohemian Coding - SketchTool](http://bohemiancoding.com/sketch/tool/)

あとは、SketchToolというコマンドラインからエクスポートできるツールがあって、これとか[psd.rb](https://github.com/layervault/psd.rb)みたいにSketchファイルのデータを将来的に読み込めれるようになったら面白いよなーと勝手に妄想している。

+ [CodeCatalyst/grunt-sketch](https://github.com/CodeCatalyst/grunt-sketch)
+ [cognitom/gulp-sketch](https://github.com/cognitom/gulp-sketch)

grunt/gulpのプラグインもある！

+ [Bohemian Coding - Sketch Developer](http://bohemiancoding.com/sketch/support/developer/)

ちなみに、Pluginの開発に関しては<del>[JSTalk](http://jstalk.org/)っていう</del>（v3.0.2からは[CocoaScriptで開発するとのこと](https://github.com/sketchplugins/cocoascript-migration/)）JSからCocoaライブリにアクセスできるやつを使うらしい。ちょっと慣れないけど、JSなら作れないこともない！やるっきゃない！

+ [Framer – A prototyping toolkit](http://framerjs.com/)
+ [bomberstudios/sketch-framer](https://github.com/bomberstudios/sketch-framer)

あと、現時点ではPhotoshopしか正式対応してないけど、Framer.jsってのを使うと、PSDとかsketchファイルからビューファイルを作ってくれるのもあったりするので今後に期待したい。

というわけで、Sketch.app使うとデザイナーだけでなくエンジニアの方も、みんながハッピーになれる可能性を持っているソフトなんじゃないかなと思うのでした。

[追記]

Sketchプラグインつくりました。

+ [Material Design Color Paletteプラグインつくった - MOL](http://t32k.me/mol/log/my-sketch-plugin/)
