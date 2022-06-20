# Python のプログラムを Heroku にデプロイする手順

## Git をインストールする

Heroku では Git を使用したデプロイができる。そのため、Git をインストール必要がある。  
[Git の公式ページ](https://gitforwindows.org/)から Git for Windows のインストーラをダウンロードする。  
インストーラを起動し、「Next」をクリックし続けて最後に「Install」ボタンでインストールする。

## Heroku のアカウントを作成する

以下の URL から必要事項を入力し、Heroku のアカウントを作成する。  
<https://signup.heroku.com/jp>  
入力したメールアドレス宛に認証メールが届くので、メール内のリンクをクリックしてアカウントを有効化する。  
パスワード設定を求められるので、任意のパスワードを設定することでアカウント作成が完了する。

## Heroku CLI をインストールする

Heroku をコマンドライン上から操作するためのツールを以下の URL からインストールする。  
<https://devcenter.heroku.com/articles/heroku-cli>  
Windows には専用のインストーラがあるのでダウンロードし、インストーラを起動する。  
基本的にはすべてデフォルトで進んで「Install」ボタンをクリックして大丈夫。  
コマンドプロンプト等で以下のコマンドを入力し、バージョン情報が表示されれば CLI のインストールは完了。

```bash
$ heroku --version
heroku/7.60.2 win32-x64 node-v14.19.0
```

## Heroku にログインする

作成したアカウントを使い、CLI から Heroku にログインする。  
`heroku login -i` を入力するとコマンドライン上でログインを求められるので、メールアドレスとパスワードを入力する。  
`-i` をつけない場合はブラウザ上で Heroku のログイン画面が立ち上がるのでそこでログインする。

```bash
$ heroku login -i
heroku: Enter your login credentials
Email: メールアドレス
Password: パスワード
Logged in as メールアドレス
```

## プログラムのデプロイを行う

Python でのデプロイの方法を記載する。

### 1. プロジェクトフォルダを作成し、移動する

```bash
mkdir project
cd project
```

### 2. プロジェクトフォルダにデプロイするために必要なファイルを準備する

- Python のプログラムファイル  
   main.py, app.py などの Python の実行ファイル
- runtime.txt  
   Python のバージョンを記載するテキストファイル
  Heroku がサポートしているバージョンを記載すること

  ```bash
  # 環境に合わせたPythonのバージョンを記載
  echo python-3.7.13 > runtime.txt
  ```

- requirements.txt  
   実行に必要なモジュールを記載する

  ```bash
  # モジュール一覧の作成
  pip freeze > requirements.txt
  ```

  ※ローカルにインストールしたモジュールがすべて出力されてしまうので、不要なものは削除すること。

- Procfile
  プログラムの実行方法を記載するファイル  
   Flask、Django 等の web アプリケーションであれば、`web: gunicorn app:app --log-file=-` を Procfile に記載する

### 3. Heroku にアプロを作成する

Heroku にログインした状態し、アプリを作成する。

```bash
# Herokuにログインしてなければログインする
heroku login
# アプリを作成する（createの後に名前を入れることで任意の名前をつけることができる）
heroku create
```

### 4. Heroku にプログラムをデプロイする

リポジトリの作成とリモートリポジトリの紐づけ（一番最初のみ）

```bash
# gitの初期ファイルを作成
git init
# ローカルリポジトリに結びつくリモートリポジトリを設定
heroku git:remote -a アプリ名
```

デプロイ（実行プログラム等の変更があった場合はここから）

```bash
# 変更したファイルをリポジトリに書き込む
git commit -am "コメント"
# Herokuにローカルで作成したファイルをpush（デプロイ）
git push heroku master
```

## 作成したアプリの確認方法

作成したアプリは `heroku open -a アプリ名` から確認できる。  
または、[Heroku の Web ページ](https://jp.heroku.com/home)からログインをした後、
Dashboard のアプリページから「Open app」をクリックするか、URL に`https://アプリ名.herokuapp.com` を入力しても確認することができる。

## その他の便利なコマンド

| コマンド               | 内容                                                       |
| ---------------------- | ---------------------------------------------------------- |
| `heroku logs`          | ログを確認 <br> `-t`をつけることでリアルタイムで確認できる |
| `heroku list`          | アプリ一覧を表示                                           |
| `heroku info アプリ名` | WebURL や Git リポジトリの URL などを確認できる            |

これら以外のコマンドについては、[公式ページ](https://devcenter.heroku.com/ja/articles/heroku-cli-commands)もしくは`heroku help`で確認できる。
