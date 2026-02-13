# Diff & masking

The View page includes a **Changes** section that displays a diff table:

- **Old values** (from `properties.old`)
- **New values** (from `properties.attributes`)

This is especially useful when using `logOnlyDirty()` in Spatie Activitylog.

---

## 1) Ensure Spatie stores `old` and `attributes`

Example of manual logging:

```php
activity()
    ->withProperties([
        'old' => ['name' => 'Old'],
        'attributes' => ['name' => 'New'],
    ])
    ->log('User updated');
```

## 2) Mask / hide sensitive fields
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