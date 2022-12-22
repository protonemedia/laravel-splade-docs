# Breeze Starter Kit

You can use the [Laravel Breeze](https://laravel.com/docs/9.x/starter-kits#laravel-breeze) starter kit to give you a head start. Laravel Breeze is a minimal, simple implementation of all of Laravel's authentication features, including login, registration, password reset, email verification, and password confirmation. We maintain a fork with a Splade stack and keep it similar to the upstream as much as possible.

The installation process is similar to the [Automatic Installation](/automatic-installation.md).

```bash
laravel new example-app

cd example-app

composer require protonemedia/laravel-splade-breeze

php artisan breeze:install
```

The `breeze:install` command will also build the frontend assets. Just like [regular Laravel applications](https://laravel.com/docs/9.x/vite#running-vite), you may run the Vite development server:

```bash
npm run dev
````