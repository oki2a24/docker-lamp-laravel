# docker_lamp_laravel
Laravel 用の Docker Compose です。

## Docker Compose の構成
### 常駐させて動かすもの
- Apache
- MySQL
- PHP

### 必要に応じて単発で動かすもの
- Composer
- Node.js

## 使い方
本ドキュメントで例として作成するプロジェクト名を `blog` とします。
`blog` という名前のディレクトリへ、必要とするパッケージが全部揃った、真新しい Laravel をインストールします。

次のようなディレクトリ構成で使用します。

```
.
└── app
    ├── docker_lamp_laravel
    └── blog
```

### 本リポジトリの導入方法と設定ファイルのコピー、設定
```bash
cd app/
git clone https://github.com/oki2a24/docker_lamp_laravel.git
cd docker_lamp_laravel
cp env-example .env
```

`APP_CODE_PATH_HOST` の値を設定します。この名前は、Laravel のプロジェクト名となります。
今回の例のような場合は次のように編集します。

```
APP_CODE_PATH_HOST=../blog/
```

### 開発環境の起動
開発環境を起動します。

```bash
docker-compose up -d
```

### Laravel プロジェクトのインストールコマンド関する注意点
Laravel プロジェクトをインストールします。
補足です。
[インストール 5.5 Laravel](https://readouble.com/laravel/5.5/ja/installation.html) を見ると、`blog` とディレクトリを指定していますが、ここでは `.` と指定しています。
`blog` と指定した場合、`blog` ディレクトリを作成し、その中へ Laravel をインストールします。
`.` と指定した場合、現在のディレクトリへ Laravel をインストールします。
`.env` に blog と設定しているため、インストールコマンドを実行するディレクトリは `blog` となります。
したがって、コマンド実行時は `blog` ではなく `.` を指定します。

```bash
docker-compose run --rm composer composer create-project --prefer-dist laravel/laravel . "5.5.*"
```

### 補足。マイグレーション実行について
マイグレーションに先立ち、`blog/.env` の設定を確認してください。
最低でも次の値を設定する必要があります。

```
DB_HOST=mysql # docker-compose.yml の services を設定
DB_DATABASE=default # docker_lanmp_laravel/.env の MYSQL_DATABASE 値を設定
DB_USERNAME=default # docker_lanmp_laravel/.env の MYSQL_USER 値を設定
```

コンテナに入り、Laravel のマイグレーションを実行してください。

```bash
docker-compose exec php_apache bash
php artisan migrate
```

### 補足。MySQL 操作について
コンテナに入り、操作するには次のコマンドを実行します。

```bash
docker-compose exec mysql bash
mysql -u root -proot
```

補足。MySQL の root ユーザのパスワードを設定するには、`docker_lanmp_laravel/.env` の MYSQL_ROOT_PASSWORD を変更してください。

### 補足。アセットコンパイルに関する操作方法
[アセットのコンパイル(Laravel Mix) 5.5 Laravel](https://readouble.com/laravel/5.5/ja/mix.html) を行う場合に実行するコマンドです。
`npm` ではなく、 `yarn` を使用することも可能です。
Windows の場合は、`npm install` は失敗すると思います。
その場合は、`npm install --no-bin-links` を試してみてください。
これでも失敗する場合がありますが、 `yarn install` を試してみてください。
なお、最初から `yarn install` を使用しても問題ありません。

注意点として、`blog` ディレクトリではなく、`docker-compose.yml` のある、`docker_lanmp_laravel` ディレクトリで実行してください。
インストール等は、`blog` ディレクトリへ反映されます。

```bash
# Laravel Mix 依存パッケージのインストール
docker-compose run --rm node npm install

# Mixの実行
docker-compose run --rm node npm run dev

# アセット変更の監視
docker-compose run --rm node npm run watch
```
