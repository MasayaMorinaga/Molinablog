+++
author = "Molina"
categories = ["Hugo","GoogleAdSense"]
date = 2020-10-07T00:23:52+09:00
subtitle = "GoogleAdSenseをHugoに導入するときのメモ"
summary = "GoogleAdSenseの審査が通ったので導入していく"
linktitle = ""
aliases = [""]
title = "GoogleAdSenseの導入"
type = "post"
math = "false"
+++

GoogleAdSenseの審査に通ったので, 広告を設置してみました. 

# GoogleAdSenseの審査
GoogleAdSenseはブログに広告を掲載できる定番のサービスです. 

せっかくブログを作ったので貼ってみようかなとは前から思っていたのですが, 審査にようやく通ったので貼ってみました. 

具体的な審査の申請方法は「副業でブログ運営！年間5000兆円稼いだボクが成功法を教えます！」みたいなサイトを作ってる高度Wordpress人材の方々が非常に詳しく書いてあるのでそちらを参考にすれば良いと思います. 

GoogleAdSenseは簡単に審査が通るとは言われていますが, 記事数だけでなくある程度の閲覧者数はいないと厳しい気がします. 自分の場合, 記事数や内容は同じでも毎日コンスタントに閲覧者が来るようになってから審査に通りました. コピペみたいな記事だと通らないのは言わずもがな. 

# 広告の設置
広告を挿入したい位置に, コードをコピペすれば良いんですが, HugoはShortcodesという機能がつかえるので, これを使っていきます. 
`layouts/shortcodes`に適当な名前, 例えば`ad.html`というファイルを作ってそこに以下のようなAdsenseのコードを記述します. 
```
<script async src="//pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<!-- 広告1 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="xxxxxxxxxxxxxxxxxxx"
     data-ad-slot="xxxxxxxxxxxxxxx"
     data-ad-format="auto"></ins>
<script>
(adsbygoogle = window.adsbygoogle || []).push({});
</script>
```
これで, 広告を挿入したい場所に`｛｛% ad %｝｝`と記述するだけで, 広告を挿入することができます. (何故かﾊﾞｸﾞるので`{`を全角で記述していますが正しくは半角です)
記事の中などにも簡単に広告を挿入できるようになります. 

# その他
わざわざ広告貼ったんですが, 収入には全く期待していません. 別にそんなに大人数が見に来るブログではないですし, そもそもこのブログを見に来るような人はAdBlockとか入れてそう（偏見）.

# 参考
https://blog.zzzmisa.com/customize_hugo_theme3/

https://gohugo.io/templates/shortcode-templates/

{{% ad %}}