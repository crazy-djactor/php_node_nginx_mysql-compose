# CoreUI Free Laravel Bootstrap Admin Template

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

[![Bootstrap Admin Template](https://coreui.io/images/github/vue-free-template-3.gif)](https://coreui.io/laravel/)

Curious why I decided to create CoreUI? Please read this article: [Jack of all trades, master of none. Why Bootstrap Admin Templates suck.](https://medium.com/@lukaszholeczek/jack-of-all-trades-master-of-none-5ea53ef8a1f#.7eqx1bcd8)

CoreUI offers 6 versions: [Bootstrap](https://github.com/coreui/coreui-free-bootstrap-admin-template), [Angular](https://github.com/coreui/coreui-free-angular-admin-template), [Laravel](https://github.com/coreui/coreui-free-laravel-admin-template), [React.js](https://github.com/coreui/coreui-free-react-admin-template), [Vue.js](https://github.com/coreui/coreui-free-vue-admin-template), and [Vue.js + Laravel](https://github.com/coreui/coreui-free-vue-laravel-admin-template).

CoreUI is meant to be the UX game changer. Pure & transparent code is devoid of redundant components, so the app is light enough to offer ultimate user experience. This means mobile devices also, where the navigation is just as easy and intuitive as on a desktop or laptop. The CoreUI Layout API lets you customize your project for almost any device – be it Mobile, Web or WebApp – CoreUI covers them all!

## Table of Contents

* [Installation](#installation)
* [Usage](#Usage)
* [Features](#Features)
* [Creators](#creators)
* [Community](#community)
* [Community Projects](#community-projects)
* [License](#license)
* [Support CoreUI Development](#support-coreui-development)

## Installation

``` bash

# go into app's directory
$ cd my-project

# install app's dependencies
$ composer install

# install app's dependencies
$ npm install

```

### If you choice to use MySQL

Copy file ".env.example", and change its name to ".env".
Then in file ".env" complete this database configuration:
* DB_CONNECTION=mysql
* DB_HOST=127.0.0.1
* DB_PORT=3306
* DB_DATABASE=laravel
* DB_USERNAME=root
* DB_PASSWORD=

### Set APP_URL

> If your project url looks like: example.com/sub-folder 
Then go to `my-project/.env`
And modify this line:

* APP_URL = 

To make it look like this:

* APP_URL = http://example.com/sub-folder


### Next step

``` bash
# in your app directory
# generate laravel APP_KEY
$ php artisan key:generate

# run database migration and seed
$ php artisan migrate:refresh --seed

# generate mixing
$ npm run dev

# and repeat generate mixing
$ npm run dev
```

## Usage

``` bash
# start local server
$ php artisan serve

# test
$ php vendor/bin/phpunit
```
