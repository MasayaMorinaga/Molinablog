+++
author = "Molina"
categories = ["Hugo","GitLab CI","Netlify"]
date = "2020-02-15T21:33:25+09:00"
description = "Hugoで日時管理しようとしたときに詰まったメモ"
subtitle = "Hugoで日時管理しようとしたときに詰まったメモ"
images = [""]
linktitle = ""
aliases = ["/blog/hugoで更新日時の管理/"]
title = "Hugoで更新日時の管理"
summary = "Hugoの日付管理機能の使い方とその注意"
type = "post"
math = "false"

+++

# Hugoで日付管理
## Hugoの日付管理パラメータ
Hugoでブログやドキュメントを作るにあたって, 日付管理が必要な場合があります. 
HugoはFront Matterで日付を管理することができ, 以下の項目の設定が可能です. 


- `date`: 記事の作成日
- `lastmod`: 更新日
- `publishDate`: 公開日
- `expiryDate`: 有効期限

これらのパラメータに応じて, 最新記事の一覧を作ったり, 最近更新された記事の一覧を作ることができます. また, デフォルトでは公開日に達していない記事や, 有効期限を過ぎた記事はビルドされないはずです. 

これらのパラメータですが, 各記事に記述されているFront Matterの中の`date`や`lastmod`などの値が呼び出される様になっていて, その値が存在しない場合は他の値が呼び出されます. どの値が呼び出されるかはデフォルトでは以下のようになっています. 

```toml
[frontmatter]
date = ["date", "publishDate", "lastmod"]
lastmod = [":git", "lastmod", "date", "publishDate"]
publishDate = ["publishDate", "date"]
expiryDate = ["expiryDate"]
```
たとえばdateであれば, 基本的に`date`の値が呼び出されますが, 存在しない場合は`publishDate`, それも存在しなければ`lastmod`の値が呼び出されます. どの値も存在しない場合は全部0埋めになるようです. 

この順序や呼び出す値については, `config.toml`に`[frontmatter]`を記述することで上書きすることが可能です. 
面白い設定値としては以下のようなものがあります. 
- `:fileModTime` ファイルの更新日時
- `:filename` ファイル名. 例えばファイル名が2018-02-22-popopo.mdであれば2018-02-22となります. 

## Gitのコミット日時を日付管理に使う
ところで, 上の設定に, `:git`という設定値がlastmodにあると思います. これは対象の記事のgitコミットの最新日時を自動的に取得し, その日付が呼び出されます. 

これによって, 記事を更新したときにその記事の`lastmod`をわざわざ更新しなくても, 自動的に更新日が取得されます. これをうまく使うと, 更新記事の一覧を作れたりします. 
この記事に詳しいやり方があるのでそのようなページを作りたい場合はこちらが参考になるかと思います. 

https://maku77.github.io/hugo/list/recents.html

さて, このGitのコミット日時を使用するためには, `config.toml`で`enableGitInfo = true`にしておくか, ビルドするときにオプションで`--enableGitInfo`をつけてビルドする必要があります. 

また, 私の環境では, 記事のファイル名がASCII文字のみで構成されていないと, うまくGitのコミット日時が取得できませんでした. なにかのバグなのかな？

# ビルド時の注意
さて, ビルド時の話ですが, ローカルでビルドするのであればあまり問題にならないのですが, サーバー側でビルドする場合, サーバー側のタイムゾーンがずれていると, 更新日時がずれてしまうことがあります. 恐らく(日本にいる方は)JSTをローカルタイムに使うと思いますが, 外部のサービスでは必ずしもJSTがローカルタイムとしてセットされているわけではなく, デフォルトではUTCがセットされている場合も多々あります. 
## GitLab CIの場合
GitLab CIの場合, こちらに書いてあるようにVariablesからタイムゾーンを指定できるようです. 

https://gitlab.com/gitlab-com/support-forum/issues/4051

しかし, 自分が使っているHugoのイメージは`/usr/share/zoneinfo`が消されているようで, この方法ではうまくいきませんでした. 

そこで, ビルドコマンドの前に
```bash
- apk --no-cache add tzdata && \
- cp /usr/share/zoneinfo/Asia/Tokyo /etc/localtime
```
としてやることでJSTがをローカルタイムにセットしてやることができました.

## Netlifyの場合
Netlifyの場合は環境変数を設定してやればうまくいくようです. 環境変数の指定方法は2つあります. 
### netlify.tomlを作成しそこに書く
netlify.tomlを作成し,
```toml
[build.environment]
  TZ="Asia/Tokyo"
```
としてやると環境変数を指定できます. 

### Netlifyの設定ページ上で設定する
サイト選択 > settings > Build & Deploy > Environment から環境変数を登録出来るので, Keyを`TZ`, Valueを`Asia/Tokyo`としてやります. 

# まとめ
- Hugoには日付管理機能がある
- ビルドする環境には注意

# 参考
https://gohugo.io/getting-started/configuration/#configure-front-matter

http://kawaken.hateblo.jp/entry/2018/08/30/190954

https://qiita.com/tommy_aka_jps/items/3cf937942e5a060e5d72

{{% ad %}}