# Jetstream Starter Kit

You can use the [Laravel Jetstream](https://laravel.com/docs/10.x/starter-kits#laravel-jetstream) starter kit to give you a head start. Jetstream provides a beautifully designed application scaffolding for Laravel and includes login, registration, email verification, two-factor authentication, session management, API support via Laravel Sanctum, and optional team management. We maintain a fork with a Splade stack and keep it similar to the upstream as much as possible.

The installation process is similar to the [Automatic Installation](/automatic-installation.md). Note that Jetstream for Splade requires Laravel 10.

```bash
laravel new example-app

cd example-app

composer require protonemedia/laravel-splade-jetstream

php artisan jetstream:install --teams --api --verification
```

The `jetstream:install` command will also build the frontend assets. Just like [regular Laravel applications](https://laravel.com/docs/10.x/vite#running-vite), you may run the Vite development server:

```bash
npm run dev
````