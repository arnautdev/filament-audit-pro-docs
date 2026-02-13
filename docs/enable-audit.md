# Enable audit for models

This plugin displays logs produced by **spatie/laravel-activitylog**.

To start recording activities, enable logging for the models you want to audit.

---

## 1) Install & configure Spatie Activitylog

If you haven't done it yet, follow the Spatie installation steps and run migrations.

---

## 2) Add the LogsActivity trait to your model

Example:

```php
use Illuminate\Database\Eloquent\Model;
use Spatie\Activitylog\Traits\LogsActivity;
use Spatie\Activitylog\LogOptions;

class Order extends Model
{
    use LogsActivity;

    public function getActivitylogOptions(): LogOptions
    {
        return LogOptions::defaults()
            ->logOnly(['status', 'total'])
            ->logOnlyDirty()
            ->dontSubmitEmptyLogs();
    }
}
```

## 3) Log custom events (optional)

```php
activity()
    ->log('Something happened');

Or attach a subject / causer:

activity()
    ->performedOn($order)
    ->causedBy(auth()->user())
    ->withProperties([
        'attributes' => ['status' => 'paid'],
        'old' => ['status' => 'pending'],
    ])
    ->log('Order updated');
```