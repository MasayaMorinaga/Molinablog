+++
author = "Molina"
categories = ["Hugo","GitLab CI","Netlify"]
date = "2020-02-03T03:09:39+09:00"
subtitle = "Hugoを使って個人wikiを作った話"
summary = "HugoとDocsyを使って個人wikiを作ってみたのでその記録"
linktitle = ""
aliases = ["/blog/hugo+docsyで個人wikiを作る/"]
title = "Hugo+Docsyで個人Wikiを作る"
type = "post"
math = "false"

+++
HugoとDocsyを使って個人wikiを作ってみました. 
# 概要
## そもそも個人wikiとは
Wikiといえば[Wikipedia](https://ja.wikipedia.org/wiki/%E3%83%A1%E3%82%A4%E3%83%B3%E3%83%9A%E3%83%BC%E3%82%B8)を思い浮かべる人が多いと思います. Wikipediaは不特定多数の人が編集でき, 公開されていますが, あんな感じのサイトを自分専用に作りたいなぁということです. 
そんなんいるか？みたいな人のためにどういう内容が欲しいか上げると

- よく使うコマンドのメモ
- 参考になるサイトの整理
- 作業日誌
- 思いついたことのメモ

などです. まぁ要するにブラウザで見れるメモ帳みたいなものです. 

メモ帳(物理)とかノート(物理)で良いのでは？みたいな話はあるかもしれませんが, 紙ベースのものはかさばるし, 検索性が悪いのであまり使いたくありません. 持ち運んでいると傷んでいってしまうのも悲しい. 

外部サービス(EvernoteとかOneNoteとか)を使えば良いのでは？というのはありますが, サービスが終了したり, 突如課金性になったりすると, 移行などの手間を考えるとあまり外部のサービスに乗っかりたくないというのがあります. できればMarkdown等の汎用性の高い形で記録しておきたいですね. [HackMD](https://hackmd.io/)はその点は問題ないのでちょっとしたメモを置いておくのに結構使っていますが, 量が増えてくると管理がめんどくさいかなというのと, オフラインになってしまうと完全に死んでしまうので厳しい. 

## 使えそうなサービス
wikiを作るのに使えそうなサービスはいくつかあります. [MediaWiki](https://www.mediawiki.org/wiki/MediaWiki/ja)とかが有名ですが, 最近は[Crowi](https://site.crowi.wiki/)やそのforkである[GROWI](https://growi.org/ja/)が勢いがあるように思います(完全に主観). 個人的にはちょっと使ってみた感じではGROWIが良さそうなので, サーバー立てたらGROWI使ってみようかなと思っています. 

今回は偶然DocsyというWikiとして使えそうなテーマを見つけたので, とりあえず作ってみようかなという気持ちで作りました. どのくらい使えるか, 今度どうしていくかは未知ですが, 別にすぐできるしやってみようかなという気持ち. GitLabとGitLab Pagesを利用してオンラインでアクセスできるようにします. 

# Wikiの構築
## Docsyとは
[Docsy](https://github.com/google/docsy)はHugoのテーマの1つで, オープンソースプロジェクト向けのテーマとしてGoogleが公開しています. 

ドキュメントとブログが作れるようになっており, ドキュメントはそのページからGithubの編集ページに飛べるようになっていたり, issueを投げられたり出来るようになっています. 検索機能もついています. 

まぁドキュメントを管理するのに必要な機能は一通り揃っていそうですね. デザインも悪くないかなと思ったのでこれを採用しようかなと. ちなみにHugoでほかにドキュメントが管理できそうなテーマは[Learn](https://themes.gohugo.io/hugo-theme-learn/)やそのforkである[DocDock](https://docdock.netlify.com/), [Book](https://themes.gohugo.io/hugo-book/)などがあります. 

## 環境構築
Docsyを使用するにはHugo-Extendedのv0.53以降が必要となるので, こちらを導入します. 
WindowsでのHugoの導入は[昔の記事](/blog/hugoでブログ的なものを作る/)に書いたので参考になれば. Linuxの場合は`sudo apt-get install hugo`で入れるとextendedなversionが入らないらしいので, [公式](https://github.com/gohugoio/hugo/releases)から最新のextended versionを落としてきてインストールしたほうがいいらしいです. 

また, DocsyはPostCSSを使用しており, これを使うにはNode.jsの実行環境とパッケージマネージャーのnpmが使える環境が必要です. 
Node.jsとnpmはWindowsなら[公式サイト](https://nodejs.org/ja/)からLTSバージョンを落としてきてインストーラーをポチポチしてたら入ります. 特にpathを通す必要はなかったですがコマンドプロンプトとかは再起動しないといけなかったかな. Linuxとかは詳しくは知りません. なんか`apt-get`で入れるのは評判が悪いっぽい. PPAで入れるとかn packageで入れるとかネットでちょっと調べるといろいろ出てくるので自分にあった方法を選択すれば良いと思います. 
とりあえずnpmコマンドが使える環境を作れば大丈夫です. 

あとgitも使います. 

## wiki本体の作成
ではHugoでwikiを作っていきたいと思います. 
wikiを作りたいディレクトリに移動し, 
```bash
hugo new site wiki
```
で最低限のディレクトリが揃います. 
```bash
cd wiki
```
でwikiのディレクトリに移動し, 
```bash
git init
```
でGitのリポジトリを作ります.

その後サブモジュールでテーマを加えます. 
```bash
git submodule add https://github.com/google/docsy.git themes/docsy
```
とし, 
```bash
git submodule update --init --recursive
```
でサブモジュールをアップデートします. 結構時間がかかります. 
サブモジュールのアップデートが終わったらPostCSSを導入します. 
```bash
npm install autoprefixer
npm install postcss-cli
```
を実行します. これで導入はひとまず終了です.

## Docsyの設定
Docsyが導入できたら設定をしていきます.
[サンプルサイト](https://example.docsy.dev/)を真似していけば楽にできると思います. 
ソースは[GitHub](https://github.com/google/docsy-example)にあるので, `config.toml`など, 必要なものだけコピーそてくれば良いと思います. 
とりあえず言語設定のところを
```toml
[languages]
  [languages.ja]
    languageName = "日本語"
    languageCode = "ja"
    contentDir = "content/ja"
```
とすればサイトが日本語になります. あとは`github_repo`のところに自分がPushする予定のGitHubなりGitLabなりのレポジトリのURLを書いとくと良さそう. 検索機能はGoogle Custom SearchとAlgolia DocSearchとLunr.jsを用いたものから選べますが, オフラインで使えるのはLunr.jsを用いたもののみのはずです. 

その他の細かい設定は[公式ドキュメント](https://www.docsy.dev/docs/)に書いてあるのでそちらを読みつつカスタマイズしていくと良いと思います. 
```bash
hugo server
```
でweb サーバーをローカルに起動できるので, [http://localhost:1313/](http://localhost:1313/)にアクセスすれば実際に出来栄えを確認しながらカスタマイズできます. 

## GitLab Pagesにデプロイ
さて, Wikiをデプロイするのにあたって今回はGitLab CIとGit Lab Pagesを使います. 
なぜGit Lab Pagesかというと, アクセス制限をかけられるからです. 自分の知る限り無料でアクセス制限のあるページが作れるのはGit Lab Pagesのみだったので, 今回はこちらを使おうと思います. 

まずGitLabにリポジトリを作って作ったwikiをpushします. 
その際に, ディレクトリの直下に.gitlab-ci.ymlというファイルを作成します. 
内容は以下のようにします. 
```yaml
image: registry.gitlab.com/pages/hugo/hugo_extended:latest

variables:
  GIT_SUBMODULE_STRATEGY: recursive

pages:
  stage: deploy
  environment: production
  script:
  - apk --update add git nodejs nodejs-npm
  - npm install autoprefixer
  - npm install postcss-cli
  - hugo

  artifacts:
    paths:
    - public

```
中身をざっくり説明すると, `image:`で使用するイメージを指定します. `hugo_extended`でないとDocsyは失敗するので注意です. `variables:`のところでサブモジュールを更新し, `pages:`でページのデプロイを行います. デフォルトのままでは`npm`が使えないので`- apk --update add git nodejs nodejs-npm`で`npm`を使えるようにし, 必要なパッケージをインストールした後, `hugo`コマンドでビルドします.

このファイルを追加しておくだけで, GitLab CIが自動で上記の処理をやってくれます. より詳ししい内容は[公式マニュアル](https://docs.gitlab.com/ee/ci/README.html)を参照してください. 

アクセス制限はプロジェクトページの「Settings」から「Visibility, project features, permissions」のブロックを開き「Pages」という設定からできます. 

設定項目は少ないですが、

- Everyone（一般公開）
- Everyone with access (Gitlabにログインしている全てのメンバー / プロジェクトがpublicの時のみ)
- Only Project Member (プロジェクトのメンバーのみ)

の3種類から選ぶことが出来ます. 

今回は自分以外に見えてほしくないのでOnly Project Memberを選択しました. 

以上の内容を設定してpushすれば, GitLab CIがビルドをしてGitLab Pages にサイトがホスティングされます. 

URLは、Settings > Pages から確認, 変更できます. 

最初にpushしてから30分位は不安定でバグバグしいです. 気長に待ちましょう. 僕は20分くらいで安定しましたが, 3時間位かかる人もいるらしい. 

# 使用感, 懸念点など
## 良い点
とりあえず使ってみた感じでは便利につかえています. 大規模な編集はリポジトリをpullしてきてローカルで編集できますし, localhostでローカル環境のみで使うこともできます. 

また, 細かい設定はGitLabのページから直接編集できます. `github_repo`にリポジトリを設定しておけばページのサイドバーの編集リンクから直接そのページの編集ページへ飛ぶことができます. GitLabのWeb IDEを使えば, もう少し高度な編集も楽にできて便利です. GitLab上でpushもできるので, ビルドの間ちょっと待てばサイトも更新されるので, ブラウザ上ですべての操作が完結します. 

## 悪い点
### Gitlab CIからページ公開までが遅い
これはまぁでも仕方がないかな. そもそもGitLab CIはソフトウェア構築向けのもので, 静的サイトをビルドして公開するのには向いていないらしい. 公開されるページのほうも最初のpushはかなりバグバグしかったが, 時間が立つに連れてそこまで遅くはなくなった. pushしてからビルドしてサイトが公開されるまで少しは待たないといけないが, 待てないほどではないのでまぁいいかなと言う感じ. 
### 検索機能がゴミ
具体的には日本語の検索機能が使い物にならない. 一応Lunr.jsは日本語に対応しているはずなんですが, なんかパッケージが足りてないのかな……　ここらへんは勉強不足なのでそのうち改善していきたいと思っています. まぁ別にLunr.jsにこだわらなくても使えそうな検索エンジンがあればそれを導入すればいいかなくらいに考えています. 

# その他Tips
Netlifyでビルド&公開する場合は, Netlifyのビルドコマンドの設定を
```bash
cd themes/docsy && git submodule update -f --init && cd ../.. && npm install autoprefixer && npm install postcss-cli && hugo
```
とすれば良い. その際,「archetypes」「assets」「data」「layouts」「satatic」フォルダが直下のディレクトリに存在しないとエラーになる. gitでは空フォルダは同期されないので, 空フォルダには「.gitkeep」ファイルを追加するなどして空フォルダ出ない状態にして置く必要がある. 

# 参考
https://knowledge.sakura.ad.jp/22908/

https://www.docsy.dev/docs/

https://www.sejuku.net/blog/84238

https://qiita.com/otuhs_d/items/217d58bc3171f6a38016

https://www.serversus.work/topics/iir5lhcfgivsdxpxgdfb/

https://www.gwtcenter.com/gitlab-pages-static-content