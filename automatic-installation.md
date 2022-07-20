# Automatic Installation

A fresh Laravel application is the ideal way to get started with Splade. The package provides a convinient Artisan command, which installs [Tailwind CSS 3.0](https://tailwindcss.com) and [Vue 3.0](https://vuejs.org) on the frontend. On the backend, it installs a Route Middleware, an Exception Handler, and it prepares a default view and with its route.

```bash
laravel new example-app

cd example-app

composer require protonemedia/laravel-splade

php artisan splade:install

npm install

npm run dev
```