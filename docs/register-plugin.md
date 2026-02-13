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
