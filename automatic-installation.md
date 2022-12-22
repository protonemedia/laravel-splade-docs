# Automatic Installation

A fresh Laravel application is an ideal way to get started with Splade. The package provides a convenient Artisan command, which installs [Tailwind CSS 3.0](https://tailwindcss.com) and [Vue 3.0](https://vuejs.org) on the frontend. On the backend, it installs a Route Middleware, an Exception Handler, and it prepares a default view and its route.

```bash
laravel new example-app

cd example-app

composer require protonemedia/laravel-splade

php artisan splade:install
```

The `splade:install` command will also build the frontend assets. Just like [regular Laravel applications](https://laravel.com/docs/9.x/vite#running-vite), you may run the Vite development server:

```bash
npm run dev
````