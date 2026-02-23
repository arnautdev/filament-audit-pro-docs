## 📦 Overview

**Filament Audit Pro** brings a powerful audit log interface directly into your Filament admin panel.

Built on top of `spatie/laravel-activitylog`, it provides a production-ready UI to:

- View all system activities
- Track model changes
- Inspect causers & subjects
- Monitor events in real time
- Filter & search logs efficiently

Perfect for SaaS apps, admin dashboards, and enterprise back-offices.

---

## ✨ Features

- Filament Resource for Activity Logs
- Advanced filters (event, causer, subject)
- JSON properties viewer
- Change diff inspection
- User & model relations
- Global search support
- Multi-panel compatible
- Dark mode ready
- Localization support
- Production-ready performance

---

# Installation

```bash
composer require arnautdev/filament-audit-pro

composer require spatie/laravel-activitylog

php artisan vendor:publish --provider="Spatie\Activitylog\ActivitylogServiceProvider" --tag="activitylog-migrations"

php artisan migrate

php artisan vendor:publish --tag="filament-audit-pro-config"
```

# Register Plugin

After installation, you must register the plugin inside your Filament panel.

Open your `AdminPanelProvider` (or custom panel provider) and register the plugin:

```php
use Arnautdev\FilamentAuditPro\FilamentAuditProPlugin;

public function panel(Panel $panel): Panel
{
    return $panel
        ->plugins([
            FilamentAuditProPlugin::make(),
        ]);
}
```

# Enable audit for models

This plugin displays logs produced by **spatie/laravel-activitylog**.

To start recording activities, enable logging for the models you want to audit.

---

## Install & configure Spatie Activitylog

If you haven't done it yet, follow the Spatie installation steps and run migrations.

---

## Add the LogsActivity trait to your model

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

## Log custom events (optional)

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

# Diff & masking

The View page includes a **Changes** section that displays a diff table:

- **Old values** (from `properties.old`)
- **New values** (from `properties.attributes`)

This is especially useful when using `logOnlyDirty()` in Spatie Activitylog.

---

## Ensure Spatie stores `old` and `attributes`

Example of manual logging:

```php
activity()
    ->withProperties([
        'old' => ['name' => 'Old'],
        'attributes' => ['name' => 'New'],
    ])
    ->log('User updated');
```

## Mask / hide sensitive fields
You can hide or mask sensitive fields in the diff table using the plugin config.

Publish config (optional):
```bash
php artisan vendor:publish --tag="filament-audit-pro-config"
```

Then configure:
```php
// config/filament-audit-pro.php
return [
    'diff' => [
        // Completely hide fields from the diff table:
        'hidden_fields' => [
            'password',
            'remember_token',
            'token',
        ],

        // Mask fields (keep key but replace value):
        'masked_fields' => [
            'email',
            'phone',
        ],

        // Replacement used for masked values:
        'mask' => '********',
    ],
];
```

Tip: Use hidden_fields for secrets, and masked_fields for personal data you still want to indicate as “changed”.

# Permissions

By default, the Activity Log resource is **read-only**:
- No create actions
- No edit actions
- View page only

How you restrict access depends on your Filament setup.

---

## Option A: Filament Shield (recommended)

If you use Filament Shield, generate permissions and assign them to roles.

Typical permissions:
- `view_any_activity_log`
- `view_activity_log`

(Exact names depend on your resource policy / Shield configuration.)

---

## Option B: Resource authorization

You can restrict access by implementing authorization in your app:

### 1) Register a Policy for the Activity model

Example policy methods:
- `viewAny`
- `view`

Then ensure Filament is using it.

---

## Option C: Panel-level access

If your panel is only accessible to admins, that may be enough.

---

## Notes

- This plugin does not force a permission system.
- The recommended approach is to use Filament Shield or your own policies.


### List page

![Activity Logs - List](assets/arnautdev-activity-logs-list.png)

### View page & changes

![Activity Logs - View](assets/arnautdev-activity-logs-view.png)
