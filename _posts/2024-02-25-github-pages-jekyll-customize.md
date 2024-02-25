---
layout: post
title: Github Pages
---

<!-- omit in toc -->
# Jekyllのカスタマイズ
Jekyllのデフォルト状態だと見栄えがいまいちなので、いくつかのカスタマイズを加える。<br>
以下のあたりを設定するだけで大幅に見栄えがよくなる。<br>
・Jekyll Themeの変更<br>
・カスタムドメインを設定<br>

- [1. Themeを変更](#1-themeを変更)
  - [(1) Architect公式リポジトリからclone](#1-architect公式リポジトリからclone)
  - [(2) ローカルでGemfileを編集](#2-ローカルでgemfileを編集)
  - [(3) ローカルで動かしてみる](#3-ローカルで動かしてみる)
  - [(4) Github Pagesで動かす](#4-github-pagesで動かす)
- [2. カスタムドメイン設定](#2-カスタムドメイン設定)
  - [(1) まずDNSレコードを設定する](#1-まずdnsレコードを設定する)
  - [(2) Pagesの設定画面でカスタムドメインを登録](#2-pagesの設定画面でカスタムドメインを登録)
  - [参考リンク](#参考リンク)


# 1. Themeを変更
Github Pagesで利用可能なJekyll Themeは下記ページにリストされている。<br>
<https://pages.github.com/themes/><br>
選択肢が少ないが、比較的ページの構成がよさげだった `Architect` を入れてみる。<br>
<https://github.com/pages-themes/architect><br>
以下のサイトにはTheme以外にもJekyllで利用できるプラグインがリストされている。<br>
<https://qiita.com/noraworld/items/f0da9ecb608476fe3a02>

Theme変更の流れ:<br>
(1) Architect公式リポジトリからclone<br>
(2) ローカルでGemfileを編集<br>
(3) ローカルで動かしてみる<br>
(4) Github Pagesで動かす<br>

## (1) Architect公式リポジトリからclone
```
$ git clone https://github.com/pages-themes/architect.git
$ cd architect
```

## (2) ローカルでGemfileを編集
Gemfileに以下の行を追加(minimaのGemfileから抜き出したもの)
```
gem "github-pages", "~> 229", group: :jekyll_plugins
group :jekyll_plugins do
  gem "jekyll-feed", "~> 0.12"
end
gem "webrick", "~> 1.8"
```

## (3) ローカルで動かしてみる
エラーが出たら適宜修正。
```
$ bundle install
$ bundle exec jekyll serve
```
<http://127.0.0.1:4000/> で動作確認

## (4) Github Pagesで動かす
ローカルリポジトリをarchitectフォルダへ引っ越して、Githubへpushする。<br>
```
cp -r /path/to/original/.git ./
git add .
git commit -m "Theme updated"
git push origin main
```
<https://account-name.github.io/> で動作確認

# 2. カスタムドメイン設定
`yourdomain.com` のようなカスタムドメインでアクセスできるようにする。<br>
<https://yourdomain.com><br>
<https://www.yourdomain.com><br>
どちらでもアクセスできるようになる。

## (1) まずDNSレコードを設定する
ドメインレジストラのDNSサービスなどでDNSレコードを設定する。
- `yourdomain.com` の Aレコード<br>
- `www.yourdomain.com` の CNAMEレコード<br>

設定する内容は以下リンク先を参照<br>
<https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site#configuring-a-subdomain>

## (2) Pagesの設定画面でカスタムドメインを登録
`Custom domain` のところに www.yourdomain.com を設定<br>
これで yourdomain.com と www.yourdomain.com の両方アクセスできるようになる。<br>
`Enforce HTTPS` にチェックを入れると、Let's Encryptで証明書を発行してHTTPSアクセスができるようになる。<br>

## 参考リンク
<https://docs.github.com/ja/pages/setting-up-a-github-pages-site-with-jekyll/adding-a-theme-to-your-github-pages-site-using-jekyll><br>
<https://jekyllrb.com/docs/themes/><br>
