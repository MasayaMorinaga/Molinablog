+++
author = "Molina"
categories = ["Hugo"]
date = "2019-09-03"
subtitle = "WindowsでHugoの環境構築"
aliases = ["/blog/Hugoでブログ的なものを作る/"]
linktitle = ""
title = "Hugoでブログ的なものを作る"
summary = "Hugoでブログを作ったのでその記録"
type = "post"

+++

とりあえずHugoでブログを作ったのでその記録を残しておきます. 

# Hugoとは
静的サイトジェネレーターの1つです. 
静的サイトジェネレーターについて詳しくはggってください. めちゃくちゃ雑に説明するとホームページをhtml + css + JavaScriptでいい感じに作れるものです.

静的サイトジェネレーターにはいろいろ種類がありますが, [公式サイト](https://gohugo.io)曰くHugoはビルドが速いそうです. Go言語で作られているらしい.
ちょっと調べた感じあんまりテーマはそこまで豊富でないのかな. まぁいいや.
別にHugoにこだわりはなかったんですが, 自分の周りにHugoを使ってる人が多かったのもあってHugoにしました. 何か困ったときに周りに聞ける人がいるの大事.

# 環境構築
## Ubuntu
apt installで入ると思います. そんなに苦労しなかったので特に説明しません. おしまい.
gitが入ってない場合はgitも入れておくことをおすすめします. こちらもapt installで入ると思います.

## Windows
なんかchocolateyとかScoopとか使えばコマンドで入るらしいですね. 面白そうなので今度試してみよう.

とりあえず情弱Windowsユーザーなので手動で入れます. 64bit前提で話をすすめます.

まず, [公式のgithub](https://github.com/gohugoio/hugo/releases)からhugo_extended_0.57.2_Windows-64bit.zip
を落としてきます. 別にextendedじゃないやつでもいいんですけど動かないテーマがあったりするので特に理由がなければextendedをおすすめします. 

次に, 落としてきたzipファイルを展開します. 展開先はどこでもいいですが, Cドライブ直下あたりがいいんじゃないでしょうか. 自分はCドライブ直下に新たにhugoというフォルダを作ってそこに中身のファイルを展開しました. 以下, C:\hugoにhugo.exeがあるという前提で話を進めます.

ファイルの展開が終わったらパスを通します. 
パスを通すってなんぞやって人はここを読むと良いと思います. 
https://proengineer.internous.co.jp/content/columnfeature/5205

具体的な作業としては, システムの詳細設定の画面を開きます.
スタートメニューの検索窓で「システムの詳細設定」とでも打てば出てくると思います.
そこの右下にある「環境変数」をクリック
{{< figure src="/img/2019/09/controlsysdm.png" title="" class="center" width="400" height="" >}}

環境変数の「Path」を選択し編集をクリックします.
{{< figure src="/img/2019/09/path.png" title="" class="center" width="400" height="" >}}

新規をクリックすると新しいパスを入力できるようになるので, 「hugo.exe」をおいてあるパス(自分の場合はC:\hugo)を入力してOKをします.
{{< figure src="/img/2019/09/pathedit.png" title="" class="center" width="400" height="" >}}

これでパスは通っているはずなんですが, Windowsは気まぐれなので再起動しないと通ってないことがあります(通っていたと思いきや途中で通らなくなることもあります). 再起動することをおすすめします.

とりあえず再起動が終わったらコマンドプロンプトとかを開いてちゃんとインストールできてるか確認します. ところで最近, Microsoftはコマンドプロンプトの代わりにWindowsPowerShellを推しているようですね. 右クリックメニューからコマンドプロンプトが消えててびっくりしてしまった. まぁ今回はどっちでやってもいいと思います.

コマンドプロンプトを開きたい場合は, スタートメニューの検索窓で「コマンドプロンプト」と入力するとコマンドプロンプトが開きます. コマンドプロンプトに

```bash
hugo version
```

と入力してバージョン番号が表示されたら成功です. やったね.

とりあえずhugoが入ったら, gitがコマンドで使える状態になってない場合は, gitも導入しておくことをおすすめします. [git for windows](https://gitforwindows.org/)で良いんじゃないでしょうか. インストーラーをダウンロードして走らせるだけで全部よろしくやってくれます. 詳しいことは説明しないので必要なら調べてください.

# ブログを作ってみる
hugoとgitが入っていれば基本公式ページの[Quick-Start](https://gohugo.io/getting-started/quick-start/)に沿っていけばとりあえずのそれらしいものが作れます. 

まず, ブログを生成したいディレクトリ(フォルダ)までコマンドプロンプトで移動します. 

```bash
cd ディレクトリのパス
```
で移動できます.

移動できたら, 

```bash
hugo new site quickstart
```

を実行. ``quickstart``の部分は任意です. 好きな名前にしてください.

すると新しく「quickstart」というディレクトリ(フォルダ)が生成され, 中に最低限のディレクトリなりができていると思います. 

次に, 

```bash
cd quickstart
```

で作ったディレクトリに移動し, 

```bash
git init
```

で新たにGitのリポジトリを作ります.
その後, サブモジュールでテーマを追加します. 

```bash
git submodule add https://github.com/budparr/gohugo-theme-ananke.git themes/ananke
```

上記は, 公式に沿って[Ananke theme](https://themes.gohugo.io/gohugo-theme-ananke/)の時の例ですが, 他のテーマの場合はgitのリンクの部分と一番うしろのディレクトリの指定の部分を変えれば良いです.

これでthemes/anakeにテーマがダウンロードできました. 

テーマを適用するにはconfig.tomlでテーマを設定してやる必要があります. 
コマンドプロンプトを使う場合は

```bash
echo theme = "ananke" >> config.toml
```

とすればconfig.tomlに``theme = "ananke"``が追記されます. 別にメモ帳とかで追記しても問題ないです. 

とりあえず適当に記事を生成してみます. 

```bash
hugo new posts/my-first-post.md
```

で, content/postに新しくmy-first-post.mdが生成されると思います. 
ここにMarkdown形式で記事を書くことができます. 書き終わったら, 上から3行目あたりにある, ``draft: true``を``draft: false``にすると公開できます.

実際の出来栄えは, 

```bash
hugo server -D
```

で確認できます. 上記のコマンドを実行後, ブラウザで[http://localhost:1313/](http://localhost:1313/)にアクセスしてください. できたサイトが表示されているはずです.

なんかエラーが出てきたらエラーメッセージでggってください. テーマによってはconfig.tomlを真面目に書かないとエラーになるものがあります.

まぁだいたいどのテーマもexampleSiteのところにconfig.tomlとかの例があるので, 適宜コピーしてきて自分の気にいるように編集すると良いと思います. 

なんか思ったより長くなったので, 公開する方法はまた次回に書こうと思います.


# 参考
https://gohugo.io/getting-started/installing/

https://gohugo.io/getting-started/quick-start/

{{% ad %}}