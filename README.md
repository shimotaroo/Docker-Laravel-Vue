# Docker×Laravel環境構築手順

## 環境構成

|種類|名前|
|:--:|:--:|
|OS|linux|
|Webサーバー|Nginx|
|DBサーバー|MySQL|
|アプリケーション|PHP|

## 前提

- DockerとDocker Composeが使えること
- Docker for MacでOK
## 手順

### 1.リポジトリクローン

```
$ git clone https://github.com/shimotaroo/Yanbaru-Docker.git .
```

もしくは

```
$ git clone https://github.com/shimotaroo/Yanbaru-Docker.git
```

### 2.`.env`作成

クローンしたソース（docker-compose.ymlとかREADNE.mdとか）と同じ階層に`.env`を作成する

`.env`を以下のように修正する

```
DATABASE_NAME=データベース名
USER_NAME=ユーザー名
PASSWORD=パスワード名
ROOT_PASSWORD=パスワード（rootユーザー用）
DATABASE_NAME_TEST=データベース名（テスト用）
USER_NAME_TEST=ユーザー名（テスト用）
PASSWORD_TEST=パスワード名（テスト用）
```

各値は自由に設定してOK

※`.gitignore`に`.env`をGit管理下から外すように記載しています。

### 3.`docker-compose.yml`の修正

`web`コンテナ、`db`コンテナ、`db_test`コンテナ用ポートはご自身が使えるポートを設定

```yml
（略）

web:
    image: nginx:1.18
    ports:
      - '88:80' //88はご自身で自由に変更してOK

（略）

db:
    image: mysql:5.7
    ports:
      - '7306:3306'　//7306はご自身で自由に変更してOK

（略）

db_test:
    image: mysql:5.7
    ports:
      - '7307:3306' //7307はご自身で自由に変更してOK
```
### 4.Dockerイメージのビルド、コンテナの起動

`docker-compose.yml`があるディレクトリで以下のコマンド実行

```
$ docker-compose build
```

Dockerコンテナを起動

```
$ docker-compose up -d
```
## DBの接続を確認

※Sequel Proを使う前提で書いていますがクライアントツールは何でもOK<br>

MySQlのクライアントツールである`Sequel Pro`をインストール<br>

参考：[Mac MySQL Sequel Proの導入方法](https://qiita.com/miriwo/items/f24e6906105386ddfa83)

Sequel Proを起動します。<br>
左下の「+」ボタンを押して以下の通り入力欄を埋めます。

|入力欄|埋める文字|
|:--:|:--:|
|名前|自由|
|ホスト|127.0.0.1|
|ユーザー名|.envのUSER_NAME|
|パスワード|.envのPASSWORD
|データベース|空欄でOK|
|ポート|空欄でOK|

接続の前に「お気に入りに追加」を押しておくと次回からすぐに接続できます。<br>
お気に入り登録した後、「接続」ボタンで接続。<br>
左上の「データベースを選択...」で`.env`の`DATABASE_NAME`に指定したデータベースを選択すれば完了です。

