# Browsersync with DDEV <!-- omit in toc -->

- [Intro](#intro)
- [Requirements](#requirements)
- [Setup](#setup)
- [Errors](#errors)
  - ['400 Bad Request: The plain HTTP request was sent to HTTPS port'](#400-bad-request-the-plain-http-request-was-sent-to-https-port)

## Intro

This project is demo for [Browsersync](https://browsersync.io/) with DDEV.

It uses [Laravel](https://laravel.com/) 9.
It runs Browsersync through [`laravel-mix`](https://laravel-mix.com/).

## Requirements

- PHP 8
- DDEV 1.19

## Setup

- Clone project

```shell
git clone https://github.com/tyler36/browsersync-demo
cd browsersync-demo
```

- Start DDEV

```shell
ddev start
```

- Install dependencies & packages

```shell
ddev composer install
ddev npm install
```

- Configure Laravel environment

```shell
ddev exec cp .env.example .env
ddev artisan key:generate
```

- Add [Browsersync](https://laravel-mix.com/docs/4.0/browsersync) to Laravel Mix's `./webpack.mix.js`
  - Replace `$DDEV_HOSTNAME` with your site's hostname.

```js
let url = 'browsersync-demo.ddev.site';

mix.js('resources/js/app.js', 'public/js')
    .postCss('resources/css/app.css', 'public/css', [
        //
    ])
    .browserSync({
        proxy: url,
        host:  url,
        open:  false,
    });
```

- Run the watcher. Note: On the first attempt, Laravel-mix may download required packages.

```shell
$ ddev npm run watch
webpack compiled successfully
[Browsersync] Proxying: http://browsersync-demo.ddev.site
[Browsersync] Access URLs:
 ---------------------------------------------------
       Local: http://localhost:3000
    External: http://browsersync-demo.ddev.site:3000
 ---------------------------------------------------
          UI: http://localhost:3001
 UI External: http://localhost:3001
```

- You must access the site via HTTPS
  - For example `https://browsersync-demo.ddev.site:3000`

## Errors

### '400 Bad Request: The plain HTTP request was sent to HTTPS port'

- Access the site via HTTPS, and **not** the HTTP address shown.For example
  - ❌ `http://browsersync-demo.ddev.site:3000`
  - ✅ `https://browsersync-demo.ddev.site:3000`

This is because of how DDEV router works.
