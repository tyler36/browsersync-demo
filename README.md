# Browsersync with DDEV <!-- omit in toc -->

- [Intro](#intro)
- [Requirements](#requirements)
- [Setup](#setup)
- [Errors](#errors)
  - ['400 Bad Request: The plain HTTP request was sent to HTTPS port'](#400-bad-request-the-plain-http-request-was-sent-to-https-port)

## Intro

This project is designed as a demo for [BrowserSync](https://browsersync.io/) with DDEV.

It is based on [Laravel](https://laravel.com/) 9.
It runs BrowserSync through [`laravel-mix`](https://laravel-mix.com/).

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

- Install depenendencies & packages

```shell
ddev composer install
ddev npm install
```

- Configure Laravel environment

```shell
ddev exec cp .env.example .env
ddev artisan key:generate
```

- Add [browsersync](https://laravel-mix.com/docs/4.0/browsersync) to Laravel Mix's `./webpack.mix.js`
  - Replace `$DDEV_HOSTNAME` with your site's hostname.

```js
let url = $DDEV_HOSTNAME;

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

- Run the watcher. Note: On the first attempt, Laravel-mix may download additional required packages.

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

- Due to the the way DDEV router works you must access the site via HTTPS
  - EG. `https://browsersync-demo.ddev.site:3000`

## Errors

### '400 Bad Request: The plain HTTP request was sent to HTTPS port'

- Access the site via HTTPS, and **not** the HTTP address shown.EG.
  - ❌ `http://browsersync-demo.ddev.site:3000`
  - ✅ `https://browsersync-demo.ddev.site:3000`

This is due to how DDEV router works.
