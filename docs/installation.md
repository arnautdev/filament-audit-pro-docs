# Installation

## 1. Install via Composer

```bash
composer require arnautdev/filament-audit-pro

composer require spatie/laravel-activitylog

php artisan vendor:publish --provider="Spatie\Activitylog\ActivitylogServiceProvider" --tag="activitylog-migrations"

php artisan migrate

php artisan vendor:publish --tag="filament-audit-pro-config"
```