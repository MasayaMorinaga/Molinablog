+++
author = "Molina"
categories = ["TeX","TikZ"]
date = "2020-01-26T17:56:05+09:00"
description = "TikZを使ってｽｯとフローチャートを書く"
images = ["/img/2020/01/TikZ_flow_1.png"]
linktitle = "TikZでフローチャートを書く"
title = "TikZでフローチャートを書く"
type = "post"
math = "false"

+++
# フローチャート書きたい

まれに, フローチャート書きたいなと思うことがあります. 
ﾏｯｸﾛｿﾌﾄのﾎﾟﾜｰﾎﾟｲﾝﾄで書けみたいなことを言われますが, ﾎﾟﾜｰﾎﾟｲﾝﾄはテキストの大きさが自動で変わったり, 枠の大きさや位置を揃えたりするのが面倒だったり[^1]オブジェクトを増やすと動作が緩慢になったり[^2]するのが好みに合いません.  というかそもそもﾎﾟﾜｰﾎﾟｲﾝﾄは有料ですから, 複数台PCを持っていてどの端末でも編集したい自分にとっては予算面であまり好ましくない[^3]という問題もあります. 

オンラインで使えるサービスだと, [Drow.io](https://www.draw.io/)などはテンプレートとかがちゃんと揃っているし使い心地も悪くないと思います. ﾎﾟﾜｰﾎﾟｲﾝﾄはみたいにテキストの大きさがいつの間にか変わったりすることもないし.

しかし, できればローカルな環境でｻｻｯと作りたいなぁと思ったので, TikZあたりで書こうかと思ったら思ったより情報がなかったのでここにまとめておきます.

# TikZとは
そもそもTikZとは何かというと, TeX[^4]の描画パッケージであるPGF(Portable Graphics Format)のフロントエンド環境です. PGFは低水準言語で, TikZはそれを扱うためのマクロらしい. TeXの環境は入っていればデフォルトで使えるはずです. 

公式マニュアルはここ　[http://ftp.yz.yamagata-u.ac.jp/pub/CTAN/graphics/pgf/base/doc/pgfmanual.pdf](http://ftp.yz.yamagata-u.ac.jp/pub/CTAN/graphics/pgf/base/doc/pgfmanual.pdf)

TeX Wikiにも基本的な使い方は書いてあります. [https://texwiki.texjp.org/?TikZ](https://texwiki.texjp.org/?TikZ)

あとは[TikZ の使い方 (圏論編)](http://alg-d.com/math/kan_extension/TikZ_for_cat.pdf)が役に立ちました. 公式マニュアルの和訳ですが, フローチャートを書くのに必要なところがまとめられていて便利です. 

# フローチャートとは
フローチャートはプログラムなど処理を表す時に用いられます. 
JISで規格化されているので, それに沿って書くのが良いと思います. 

JISの規格書を読んでも良いですが, 個人的にはわかりにくかったので, ちょっと書くくらいならネットで適当に調べて出てくる記事([ここ](https://kashika.biz/flowchart-5/)とか)を見るので良い気がします. 

# TikZでフローチャートを書く
それでは本題のフローチャートを書く話です. フローチャートを書くのには, TikZのNodeコマンドを使用します. まぁﾎﾟﾜｰﾎﾟｲﾝﾄで言うところのテキストボックスってやつですね. 
```
\documentclass[dvipdfmx]{jsarticle}% 適切なドライバ指定が必要
\usepackage{tikz}

\begin{document}
\begin{tikzpicture}
    \node{po};
\end{tikzpicture}
\end{document}
```
とすると, 白紙の上にこんなのが出てくると思います. 

|![po](/img/2020/01/TikZ_po_1.png)|
|:-:|

これだけではアレなので, 囲ったりしてみます. 
```
\documentclass[dvipdfmx]{jsarticle}% 適切なドライバ指定が必要
\usepackage{tikz}

\begin{document}
\begin{tikzpicture}
    \node[rectangle,  draw,  text centered]{po};
\end{tikzpicture}
\end{document}
```
こうすると, こうなったと思います. 

|![popo](/img/2020/01/TikZ_po_2.png)|
|:-:|

`\node`の後ろの`[]`の中にオプションを与えることでスタイルを指定できます. 
`rectangle`はテキストを四角で囲う, `draw`は枠線表示, `text centered`はテキスト中央揃えです. 

```
\documentclass[dvipdfmx]{jsarticle}% 適切なドライバ指定が必要
\usepackage{tikz}

\begin{document}
\begin{tikzpicture}
    \node[rectangle,  draw,  text centered, text width=3cm, minimum height=1.5cm]{po};
\end{tikzpicture}
\end{document}
```
こうするとこうなります

|![popopo](/img/2020/01/TikZ_po_3.png)|
|:-:|

`text width`で幅,  `minimum height`で最小高さ(必要に応じて自動的に広がる)を指定しました. ところで, これらをいちいち書いていくのは面倒なので, `\tikzset`コマンドを使ってまとめます. するとこのように書き換えることが出来ます. 
```
\documentclass[dvipdfmx]{jsarticle}% 適切なドライバ指定が必要
\usepackage{tikz}

\begin{document}
\begin{tikzpicture}
    \tikzset{po/.style={rectangle,  draw,  text centered, text width=3cm, minimum height=1.5cm}};
    \node[po]{po};
\end{tikzpicture}
\end{document}
```
さて, ここまでわかれば, フローチャートに必要なNodeのオプションを定義します. 
```
\documentclass[dvipdfmx]{jsarticle}% 適切なドライバ指定が必要
\usepackage{tikz}
\usetikzlibrary{shapes.geometric}
\usetikzlibrary {shapes.misc}

\begin{document}
\begin{tikzpicture}
    \tikzset{Terminal/.style={rounded rectangle,  draw,  text centered, text width=3cm, minimum height=1.5cm}};
    \tikzset{Process/.style={rectangle,  draw,  text centered, text width=3cm, minimum height=1.5cm}};
    \tikzset{Decision/.style={diamond,  draw,  text centered, aspect=3,text width=5cm, minimum height=1.5cm}};
    \node[Terminal](a)at (0,0){たんし};
    \node[Process](b)at (4,0){ぷろせす};
    \node[Decision](c)at (10,0){はんだん};
\end{tikzpicture}
\end{document}
```
表示結果

|![flow](/img/2020/01/TikZ_flow_1.png)|
|:-:|

デフォルトでは, 両丸の四角形やひし形は使えないので, `\usetikzlibrary{shapes.geometric}` `\usetikzlibrary {shapes.misc}`を読み込む必要があるのに注意です. Nodeのオプションの後の`(a)`とかはNodeの名前で, 好きに決められます. 
`at(hoge,hoge)`で座標を指定できます.

次にNode間を矢印でつなぎます. 
`\draw[->]  (a) --(b);`とか書けばいい感じにつながってくれます. 
```
\documentclass[dvipdfmx]{jsarticle}% 適切なドライバ指定が必要
\usepackage{tikz}
\usetikzlibrary{shapes.geometric}
\usetikzlibrary {shapes.misc}

\begin{document}
\begin{tikzpicture}
    \tikzset{Terminal/.style={rounded rectangle,  draw,  text centered, text width=3cm, minimum height=1.5cm}};
    \tikzset{Process/.style={rectangle,  draw,  text centered, text width=3cm, minimum height=1.5cm}};
    \tikzset{Decision/.style={diamond,  draw,  text centered, aspect=3,text width=5cm, minimum height=1.5cm}};
    \node[Terminal](a)at (0,0){たんし};
    \node[Process](b)at (4,0){ぷろせす};
    \node[Decision](c)at (10,0){はんだん};
    \draw[->]  (a) --(b);
    \draw[->]  (b) --(c);
\end{tikzpicture}
\end{document}
```
|![flow](/img/2020/01/TikZ_flow_2.png)|
|:-:|

ほらね.
矢印の大きさとかも`\usetikzlibrary{arrows.meta}`を読み込んでｺﾞﾆｮｺﾞﾆｮすればイジれるはずです. 

まぁここまで来たら実質フローチャート書けたようなものですね. 矢印も`(a)`の代わりに座標を指定してやれば任意の場所から任意の場所まで引けます. Nodeも任意の場所におけるし矢印も任意の場所に引けるようになったと思います. ただ場合によってはNodeを相対位置で指定出来たほうが嬉しいかもしれません. `\usetikzlibrary{positioning}`することで相対位置でも場所を指定できるようになります. これを用いるとこのようなフローチャートが書けます. 

```
\documentclass[dvipdfmx]{jsarticle}% 適切なドライバ指定が必要
\usepackage{tikz}
\usetikzlibrary{shapes.geometric}
\usetikzlibrary {shapes.misc}
\usetikzlibrary{positioning}

\begin{document}
\begin{tikzpicture}
    \tikzset{Terminal/.style={rounded rectangle,  draw,  text centered, text width=3cm, minimum height=1.5cm}};
    \tikzset{Process/.style={rectangle,  draw,  text centered, text width=3cm, minimum height=1.5cm}};
    \tikzset{Decision/.style={diamond,  draw,  text centered, aspect=3,text width=5cm, minimum height=1.5cm}};
    \node[Terminal](a)at (0,0){Start};
    \node[Process, below=1.5 of a.center](b){ぷろせす};
    \draw[->, thick]  (a) --(b);
    \node[Process, below=2 of b.center](c){ぷろせす};
    \draw[->, thick]  (b) --(c);
    \node[Decision](d)at (0,-7.5){はんだんするぞ！};
    \draw[->, thick]  (c) --(d);
    \node[Process, above left=of d ](da){なんらかのぷろせす};
    \draw[->,thick]  (d)node[below, xshift=-100]{No} -| (da);
    \draw[->,thick]  (da) |- (0,-3.5);
    \node[Process, below=1.75 of d.center](e){第一関門クリア！};
    \draw[->, thick]  (d) --(e) node[right,yshift=30]{Yes};
    \node[Decision, below=1.35 of e.center](f){はんだんその2};
    \draw[->, thick]  (e) --(f);
    \node[Process, above right=of f](fa){もどる};
    \draw[->, thick]  (f)node[below, xshift=100]{No}  -| (fa);
    \draw[->,thick]  (fa) |-(0,-3.5);
    \node[Process, below=1.75 of f.center](g){第二関門突破！};
    \draw[->, thick]  (f) --(g)node[right,yshift=30]{Yes};
    \node[Terminal, below=1.75 of g.center](h){End};
    \draw[->, thick]  (g) --(h);
\end{tikzpicture}
\end{document}
```
|![flow](/img/2020/01/TikZ_flow_3.png)|
|:-:|

`below=1.5 of a.center`で`(a)`の中心から1.5cm下を表します. 
`above left=of d `はdの左上です. 
`\draw[->,thick]  (d)node[below, xshift=-100]{No} -| (da);`とすると, (d)の始点からx方向に-100ptずれた場所にNoというNodeを置き, 折れ線で(da)に向かう矢印が書けます. 

# まとめ
とりあえずフローチャートを書くのに必要なTikZの使い方をまとめました. TikZにはもっと機能がたくさんあり, もっときれいに書く方法もあるかと思いますので, 興味のある人は調べてみると面白そうです. 

以上TikZでフローチャートを書く話でした. 

# 参考
https://texwiki.texjp.org/?TikZ

https://en.wikipedia.org/wiki/PGF/TikZ

https://www.opt.mist.i.u-tokyo.ac.jp/~tasuku/tikz.html

http://kawa0616.hatenablog.com/entry/2018/03/26/Tex_%28TikZ%29_%E3%81%A7%E3%83%96%E3%83%AD%E3%83%83%E3%82%AF%E7%B7%9A%E5%9B%B3%E3%82%92%E6%9B%B8%E3%81%8F

[^1]:実は最近はだいぶ改善されているので騒ぎ散らすほど大変ではない
[^2]:メモリの暴力(32G超)で殴ればある程度解決できることが知られている
[^3]:Office 365 Soloを契約すると2020年1月現在, 年間12984円で最大5端末使えるらしいので思ったよりは安かった(が, フローチャートを書くために契約する価値があるかは微妙である). 
[^4]:フリーの「組版システム」. 詳しくは[https://texwiki.texjp.org/](https://texwiki.texjp.org/)