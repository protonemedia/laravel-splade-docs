# Jetstream Starter Kit

You can use the [Laravel Jetsteram](https://laravel.com/docs/10.x/starter-kits#laravel-jetstream) starter kit to give you a head start. Laravel Jetstream is a minimal, simple implementation of all of Laravel's authentication features, including login, registration, password reset, email verification, and password confirmation. We maintain a fork with a Splade stack and keep it similar to the upstream as much as possible.

The installation process is similar to the [Automatic Installation](/automatic-installation.md).

```bash
laravel new example-app

cd example-app

composer require protonemedia/laravel-splade-jetstream

php artisan jetstream:install
```

The `jetstream:install` command will also build the frontend assets. Just like [regular Laravel applications](https://laravel.com/docs/10.x/vite#running-vite), you may run the Vite development server:

```bash
npm run dev
```
