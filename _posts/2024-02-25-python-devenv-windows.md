---
layout: post
title: Python
---

<!-- omit in toc -->
# WindowsでPython開発環境をつくる

Windows10/11でシンプルなPythonの開発環境を作成します。<br>
PythonインタプリタはWindows用のものもありますが、ここではWSLでPythonインタプリタを実行する方法をまとめます。

- [1. WSLをインストール](#1-wslをインストール)
  - [1.1. WSL有効化](#11-wsl有効化)
  - [1.2. Debianインストール](#12-debianインストール)
  - [1.3. パッケージ更新](#13-パッケージ更新)
  - [1.4. (Optional) Windowsフォルダをリンクする](#14-optional-windowsフォルダをリンクする)
  - [1.5. 日本語化](#15-日本語化)
  - [1.6. (Optional) WSL2へアップグレード](#16-optional-wsl2へアップグレード)
  - [1.7. Pythonインストール](#17-pythonインストール)
- [2. Github](#2-github)
  - [2.1. Githubアカウント作成](#21-githubアカウント作成)
  - [2.2. SSHキーを登録](#22-sshキーを登録)
  - [2.3. ローカルリポジトリを作成](#23-ローカルリポジトリを作成)
  - [2.4. 仮想環境を作成](#24-仮想環境を作成)
  - [2.5. (Appendix) Github how to](#25-appendix-github-how-to)
- [3. VS Code](#3-vs-code)
  - [3.1. VS Codeをインストール](#31-vs-codeをインストール)
  - [3.2. WSLに接続](#32-wslに接続)
  - [3.3. その他エクステンションをインストール](#33-その他エクステンションをインストール)
  - [3.4. Workspaceを作成](#34-workspaceを作成)
  - [3.5. Pythonインタプリタを設定](#35-pythonインタプリタを設定)

# 1. WSLをインストール
WSLはWindowsの拡張機能で、Windows上でUbuntu、DebianなどのLinuxディストリビューションを利用することができます。<br>
https://learn.microsoft.com/ja-jp/windows/wsl/install<br>
Bashが使えるだけでも生産性が格段に上がりますので、WindowsをインストールしたらWSLもインストールしておくのがおすすめです。

## 1.1. WSL有効化
コントロールパネルを開き、<br>
プログラム ＞ プログラムと機能 ＞ Windowsの機能の有効化または無効化<br>
より「Linux用Windowsサブシステム」を有効化

## 1.2. Debianインストール
コマンドプロンプトを管理者権限で開き、<br>
1. 利用できるDistributionを確認
```
C:\Windows\System32>wsl -l --online
インストールできる有効なディストリビューションの一覧を次に示します。
'wsl.exe --install <Distro>' を使用してインストールします。
　
NAME                                   FRIENDLY NAME
Ubuntu                                 Ubuntu
Debian                                 Debian GNU/Linux
kali-linux                             Kali Linux Rolling
Ubuntu-18.04                           Ubuntu 18.04 LTS
...snip...
```
2. 対象のDistributionをインストール
```
C:\Windows\System32>wsl --install -d Debian
インストール中: Debian GNU/Linux
[========================64.0%======                       ]
```

3. Linuxのターミナルが開くので指示に従いユーザーを作成

## 1.3. パッケージ更新
パッケージを最新にしておく。
```
$ sudo apt-get update && sudo apt-get upgrade -y
```

## 1.4. (Optional) Windowsフォルダをリンクする
WSLのシェルからWindowsファイルシステムへファイルを書き込めるようにする。<br>
1. おまじない
```
$ sudo vi /etc/wsl.conf
[automount]
enabled = true
root = /mnt/
options = "metadata,umask=22,fmask=11"
mountFsTab = true
```
2. シンボリックリンク
```
$ ln -s /mnt/c/Users/user ./user
```
3. PCを再起動

## 1.5. 日本語化
```
$ sudo dpkg-reconfigure locales
```
`ja_JP.UTF-8` を追加してDefault Localeに設定する。<br>
ターミナルのFontをMSゴシックなどの日本語フォントに設定すると日本語が表示されるようになる。

## 1.6. (Optional) WSL2へアップグレード
WSL2が現行バージョンだが、環境によってはWSL1がインストールされてしまうことがあるので、必要に応じてWSL2へアップグレードする。<br>
MicrosoftによるとWSL2のほうがパフォーマンスが優れているなど何かといいらしい。<br>
https://learn.microsoft.com/ja-jp/windows/wsl/install-manual<br>
1. バージョンの確認 (v1が入っている)
```
C:\Windows\system32>wsl -l -v
  NAME      STATE           VERSION
 * Debian    Stopped         1
```
2. WSL機能を有効化
```
C:\Windows\system32>dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
　
展開イメージのサービスと管理ツール
バージョン: 10.0.19041.3636
　
イメージのバージョン: 10.0.19045.3930
　
機能を有効にしています
[==========================100.0%==========================]
操作は正常に完了しました。
```
3. 仮想マシン機能を有効化(Intel VT-x や AMD-V が必要) 
```
C:\Windows\system32>dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
　
展開イメージのサービスと管理ツール
バージョン: 10.0.19041.3636
　
イメージのバージョン: 10.0.19045.3930
　
機能を有効にしています
[==========================100.0%==========================]
操作は正常に完了しました。
```
4. WSL2へ変換
```
C:\Windows\system32>wsl --set-version Debian 2
変換中です。この処理には数分かかることがあります...
WSL 2 との主な違いについては、https://aka.ms/wsl2 を参照してください
変換が完了しました。
```
## 1.7. Pythonインストール
```
$ sudo apt-get install -y python3 python3-pip python3-venv
```
# 2. Github
## 2.1. Githubアカウント作成
https://github.com でアカウントを作成し、適当な名前でリポジトリを作成する。<br>

リポジトリアクセスは以下のような形で指定する。<br>
[HTTPS] https://github.com/`account`/`repository`.git<br>
[SSH] git@github.com:`account`/`repository`.git

## 2.2. SSHキーを登録
1. SSHキーを作成<br>
`$ ssh-keygen`

2. `.ssh/id_rsa.pub` をGithubへ登録<br>
https://github.com/settings/keys へアクセス<br> 
「New SSH Key」をクリックして `id_rsa.pub` を登録 

## 2.3. ローカルリポジトリを作成
1. gitインストール<br>
```
$ sudo apt-get install git
```

2. Globalパラメータ設定<br>
```
$ git config --global user.name <user name>
$ git config --global user.email <email address>
$ git config --global init.defaultBranch main
```

3. ローカルリポジトリ作成
```
$ mkdir -p path/to/local/repository
$ cd path/to/local/repository
$ git init
$ git branch -M main
```

4. リモートリポジトリと接続
```
$ git remote add origin git@github.com:<account>/<repository>.git
$ git remote -v
origin  git@github.com:<account>/<repository>.git (fetch)
origin  git@github.com:<account>/<repository>.git (push)
```

5. リモートリポジトリとの接続確認 (エラーにならなければOK)
```
$ git pull origin main
$ git push origin main
```

## 2.4. 仮想環境を作成
Pythonはプロジェクトごとに仮想環境を作り、仮想環境ごとに必要なパッケージを読み込むような作りになっている。（おそらくパッケージの依存関係などのトラブルを防ぐため）<br>
そのためpipでパッケージをロードするのも基本的には仮想環境内で実施する必要がある。<br>
ということで開発に入る前にまず仮想環境を作成する。
1. 仮想環境の作成
```
$ cd path/to/local/repository
$ python3 -m venv .venv
```

2. 仮想環境の有効化
```
$ source .venv/bin/activate
(.venv)$    <--- プロンプトが変わる
```

## 2.5. (Appendix) Github how to
1. Github ユーザーの設定
```
git config --global user.name <name>
git config --global user.email <email>
```

2. ローカルリポジトリの初期化
```
cd path\to\repo
git init
git branch -M main
```

3. リモートリポジトリを変更
```
git remote set-url origin git@github.com:<account>/<repository>.git
```

4. リポジトリを取得
```
git pull origin main
```

5. 新規作成/変更をプッシュ
```
git add <ファイル名>
git commit -m <コメント>
git push origin main
```

6. ブランチ関連
```
ローカルに作成 - git branch <ブランチ名>
ローカルで移動 - git checkout <ブランチ名>
リモートを更新 - git push origin <ブランチ名>
```

# 3. VS Code
VS CodeはMicrosoftが開発している多機能エディタです。<br>
プログラマ向けのエクステンションが豊富なので、コードを書くのであればとりあえず何も考えずにVS Codeを使うのがいいでしょう。<br>

## 3.1. VS Codeをインストール
https://code.visualstudio.com/download

## 3.2. WSLに接続
- WSLエクステンションをインストール
- コマンドパレット(CTRL+SHIFT+P)から `WSL: Connect to WSL` を実行

## 3.3. その他エクステンションをインストール
- Python
- Pylance
- (Optional) Vim
- (Optional) Markdown All in One

## 3.4. Workspaceを作成
gitローカルリポジトリフォルダをworkspaceに追加する。<br>

## 3.5. Pythonインタプリタを設定
仮想環境のPythonインタプリタを設定する。<br>
これによりVS Code内でTerminal(WSL)を開いたときに自動的に仮想環境が有効になる。
- コマンドパレット(CTRL+SHIFT+P)から `Python: Select Interpreter` を実行
- 仮想環境のインタープリタを選択 (.venv/bin/python)
これによりVS Code内でTerminal(WSL)を開いたときに自動的に仮想環境が有効になる。
