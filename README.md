# Laravel + Vue.js（JavaScript）のDocker環境構築

## version

- Laravel : 6.x
- Vue.js : 2.6.12

## attention

- M1 Mac未対応
## clone

```
$ git clone https://github.com/shimotaroo/docker-laravel-vue.git
$ cd docker-laravel-vue
```
## .env作成

- `.env.example`をコピーして`.env`を作成して各項目に値を定義する。
- `docker-compose config`で`.env`に設定した環境変数が`docker-compose.yml`にセットされているか確認する。
## Build & Up

```
$ docker-compose up -d --build
```

## コンテナ起動状態を確認

```
$ docker-compose ps
```

3つのコンテナが`Up`になっていたら正常に起動している。

## src/.env作成

```
$ cd src
$ cp .env.example .env
```

## Package Install

appコンテナに入る

```
$ docker-compose exec app bash
```

以降は全てappコンテナ内で実行

```
composer install
php artisan key:generate
npm install
```

## browerSync起動

appコンテナ内で実行

```
npm run watch
```

※`docker-compose.yml`の以下の記載が無いとエラーになる

```yml
    ports:
      - ${APP_PORT}:3000

```

## Docker Compose Command

```
イメージをビルド
$ docker-compose build

コンテナ起動
$ docker-compose up -d

イメージをビルド＋コンテナ起動
$ docker-compose up -d --build

コンテナ終了（削除）
$ docker-compose down

コンテナ再起動
$ docker-compose restart
```