---
title: Github Pages
---

<!-- omit in toc -->
# Github PagesでBlogサイトを作る
GithubにはPagesという機能があり、GithubにアップしたHTMLファイルをWebサイトとして公開することができる。<br>
静的サイトなのでアクセス数の多いページを表示させるとかタグでIndexを生成するとかはできないが、個人用のBlogサイトとしては十分な機能が無料で利用できる。<br>

またGithub PagesではJekyllという静的サイトジェネレーターを使えるので、Markdownで書いたドキュメントをGithubリポジトリにアップすると自動的にHTMLに変換してアクセスできるようにしてくれる。<br>

MarkdownとGitの仕組みを組み合わせてドキュメントのメンテナンスを効率的に行えるので、無料で利用できる個人用のドキュメントアーカイブとしては大変便利なサービスである。<br>

ここではまずGithub PagesでJekyllを使った静的サイトが動くようにするまでの手順をまとめる。<br>

- [1. Github Pagesサイトをつくる](#1-github-pagesサイトをつくる)
  - [1.1 Githubアカウントを作る](#11-githubアカウントを作る)
  - [1.2 リポジトリを作る](#12-リポジトリを作る)
- [2. ローカル環境をつくる](#2-ローカル環境をつくる)
  - [2.1 ローカルリポジトリの作成](#21-ローカルリポジトリの作成)
  - [2.2 Jekyllをローカルで動かす](#22-jekyllをローカルで動かす)
    - [(1) GEMの保存先を設定](#1-gemの保存先を設定)
    - [(2) Ruby をインストール](#2-ruby-をインストール)
    - [(3) jekyll と bundler をインストール](#3-jekyll-と-bundler-をインストール)
    - [(4) Jekyll サイトを ./myblog に作成](#4-jekyll-サイトを-myblog-に作成)
    - [(5) Gemfile を編集して gem を一括インストール](#5-gemfile-を編集して-gem-を一括インストール)
    - [(6) Jekyll サイトを起動](#6-jekyll-サイトを起動)
  - [2.3 ローカルファイルをpush](#23-ローカルファイルをpush)
- [3. Github Pagesサイトを更新する](#3-github-pagesサイトを更新する)

# 1. Github Pagesサイトをつくる
## 1.1 Githubアカウントを作る
<https://github.com> へアクセスして適当なアカウントを作る。

## 1.2 リポジトリを作る
Githubへログインしてリポジトリを新規作成する。<br>
PagesでJekyllを使う場合は以下のようなネーミングルールで作成しないとダメ。<br>
- リポジトリ名: `account-name.github.io`<br>

Pagesはデフォルトで有効になっている。<br>
リポジトリの `Settings > Pages` で設定の変更ができる。<br>
カスタムドメインの設定はここで実施する。<br>

これだけで `https://account-name.github.io/` で静的Blogサイトが作られる。<br>
リポジトリを作成したばかりで `README.md` しかない状態だとこのように表示される。<br>
![Default Page](./images/github-pages-setup-1.png)

ここにHTMLファイルを作りこんでいくこともできるが、Jekyllを使ってMarkdownで記事を書いていくのが楽でいい。<br>
ということでここからJekyllを使えるように設定していく。

まだコンテンツを何もアップロードしていないので、この時点で上記URLにアクセスしても 404 File not found となる。

以下が公式のクイックスタートガイド。基本的にこれに沿って作業していく。<br>
<https://docs.github.com/ja/pages/setting-up-a-github-pages-site-with-jekyll/creating-a-github-pages-site-with-jekyll>

以下はHexo(Js)で設定した例だがイメージをつかむのに参考になる。<br>
Hexoの場合は上記のリポジトリ名ルールでなくても動くみたい。<br>
<https://www.bedroomcomputing.com/2020/08/2020-0815-engineer-static-site-gen-blog/><br>
<https://www.bedroomcomputing.com/2020/11/2020-1123-hexo-github/>

# 2. ローカル環境をつくる
設定の流れは以下のようになる。
- ローカルでGitリポジトリを作って、そこにJekyll環境を作る
- Jekyll環境をローカルでチューニングする
- できあがったJekyll環境をリモートリポジトリにPushする

ローカル環境にはWSLを使う。

## 2.1 ローカルリポジトリの作成
```
$ mkdir path/to/local/repo
$ cd pas/to/local/repo
$ git init
$ git remote add origin git@github.com:account/account.github.io.git
$ git pull origin main
```

## 2.2 Jekyllをローカルで動かす
<https://jekyllrb.com/docs/><br>
<https://jekyllrb.com/docs/installation/><br>
<https://docs.github.com/ja/pages/setting-up-a-github-pages-site-with-jekyll/creating-a-github-pages-site-with-jekyll><br>
ここらを参照して設定。<br>
一連の作業が無事終わると、Jekyllにデフォルトで入っているminimaというThemeでサイトが起動する。

### (1) GEMの保存先を設定
gemはRubyのパッケージ（ライブラリ）管理の仕組みで、Pythonでいうところのpip。<br>
rootでインストールするとパッケージの依存関係で先々困ることになるので、ユーザー毎にgemの保存先を設定して管理するような仕組みになっている。<br>
詳細は以下のサイトなどを参照。<br>
<https://qiita.com/oshou/items/6283c2315dc7dd244aef>

以下を.bashrcに書いて `source .bashrc` で読み込む。
```
export GEM_HOME="$HOME/gems"
export BUNDLE_PATH="$HOME/gems"
export PATH="$HOME/gems/bin:$PATH"
```

### (2) Ruby をインストール
```
sudo apt-get install ruby-full build-essential
```

### (3) jekyll と bundler をインストール
```
gem install jekyll bundler
```

### (4) Jekyll サイトを ./myblog に作成
```
jekyll new myblog
cd myblog
```

### (5) Gemfile を編集して gem を一括インストール
gem "jekyll" で始まる行をコメントアウト。<br>
gem "github-pages" で始まる行にバージョン `"~> 229"` を追記してアンコメント。<br>
webrickも入れないとダメみたい。
```
#gem "jekyll", "~> 4.3.3"
gem "github-pages", "~> 229", group: :jekyll_plugins
gem "webrick", "~> 1.8"
```

github-pagesのバージョンはここから確認。<br>
<https://pages.github.com/versions/>

次のコマンドを実行。<br>
正しいバージョンの Jekyll が github-pages gem の依存関係としてインストールされる。<br>
```
bundle install
```

### (6) Jekyll サイトを起動
```
bundle exec jekyll serve
```
成功すると、<http://127.0.0.1:4000> でサイトが起動する。

## 2.3 ローカルファイルをpush
ローカルで動いたJekyll環境を、リモートリポジトリにPushする。<br>
うまくいったら <https://account-name.github.io/> に反映される。
```
$ git add .
$ git commit -m "Initial commit"
$ git push origin main
```

# 3. Github Pagesサイトを更新する
ローカル環境で作成したMarkdownファイル(.md)をリモートリポジトリにPushすると、自動的にHTMLファイルが生成されてブラウザでアクセスできるようになる。<br>
