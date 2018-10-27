# docker_lamp_laravel
Laravel 用の Docker Compose です。

## 構成
- Apache
- MySQL
- PHP

## 前提
- Laravel のプロジェクトは作成されているものとします。
  - 例えば、プロジェクトディレクトリ名を、`sample` として説明します。

## 構成
次のようなディレクトリ構成で使用します。

```
.
└── app
    ├── docker_lamp_laravel
    └── sample
```

## 導入

```bash
cd app/
git clone https://github.com/oki2a24/docker_lamp_laravel.git
```

```bash
docker-compose run --rm node npm install
docker-compose run --rm node npm run dev
docker-compose run --rm node npm run watch
```
