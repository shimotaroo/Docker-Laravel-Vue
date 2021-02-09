## Laravelプロジェクトのルートディレクトリsrcを作成

```
$ ls 
README.md		docker			docker-compose.yml	src
```

## clone

```
$ cd src
$ git clone https://github.com/shimotaroo/engineer-platform.git .
```

もしくは

```
$ git clone https://github.com/shimotaroo/engineer-platform.git
```

実行後、`engineer-platform`ディレクトリを`src`に名称変更

## .envを作成

```
$ cp .env.sample .env
```
## .env修正

```
DB_CONNECTION=mysql
DB_HOST=db
DB_PORT=3306
DB_DATABASE={Docker環境用の.envのDATABASE_NAME}
DB_USERNAME={Docker環境用の.envのUSER_NAME}
DB_PASSWORD={Docker環境用の.envのPASSWORD}
```

## Composer install & APP_KEY作成

```
$ docker-compose exec app bash
$ composer install
$ php artisan key:generate
```

## `config/app.php`を修正

```
'timezone' => 'Asia/Tokyo',
'locale' => 'ja',
'fallback_locale' => 'ja',
'faker_locale' => 'ja_JP',
```

## npmでパッケージをインストールする

```
$ npm install
```

## Vue.jsをインストール

```
$ npm install -D vue
```

## Laravel Mix

- browerSyncのパッケージをインストール

```
$ npm install browser-sync-webpack-plugin -D
```

- `webpack.mix.js`を修正

```js
mix.js('resources/js/app.js', 'public/js')
    .browserSync({
        proxy: "web", //nginxコンテナのサービス名
        files: [
            './resources/**/*',
            './public/**/*',
        ],
        open: false,
        reloadOnRestart: true,
    })
    .version();
```

以下のコマンドをターミナルから実行

```
$ docker-compose up -d
$ docker-compose exec app bash
$ npm run watch

 DONE  Compiled successfully in 5386ms                                                                                                                             10:56:33 PM

     Asset     Size   Chunks             Chunk Names
/js/app.js  334 KiB  /js/app  [emitted]  /js/app
[Browsersync] Proxying: http://web
[Browsersync] Access URLs:
 -----------------------------------
       Local: http://localhost:3000
    External: http://172.27.0.3:3000
 -----------------------------------
          UI: http://localhost:3001
 UI External: http://localhost:3001
 -----------------------------------
[Browsersync] Watching files...
```

この状態で`http://localhost:3000'にアクセスする

※PHPコンテナのportsに'3000:3000'を追記しないとダメだった

## .gitignore修正

以下を追加

```
/public/css
/public/js  
/public/mix-manifest.json
```

