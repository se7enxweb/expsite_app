# Using expsite_app

`expsite_app` is intentionally almost empty: it is the container a site project fills with its own overrides and glue. Everything below describes how to put project customizations here without touching the managed theme or feature extensions.

## The helper class

```php
$name = expSiteApp::designExtensionName(); // 'expsite_app'
```

`classes/expsiteapp.php` is a placeholder for per-project site application helpers. Add your project's static helpers to this extension (in new class files under `classes/`) rather than extending kernel or theme extensions.

## Scenario: override a template from the simple design

Copy the template you want to change from the theme (for example `extension/sevenx_themes_simple/design/simple/...`) into the same relative path under this extension:

```
extension/expsite_app/design/simple/templates/<path>/<template>.tpl
```

Full-template overrides driven by `override.ini` conditions go into:

```
extension/expsite_app/design/simple/override/templates/<template>.tpl
```

with a matching rule in your siteaccess `override.ini.append.php`:

```ini
[article_full_view]
Source=node/view/full.tpl
MatchFile=article_full.tpl
Subdir=templates
Match[class_identifier]=article
```

Then clear caches:

```bash
php bin/php/ezcache.php --clear-all --purge --allow-root-user
```

The design cascade searches design extensions in `DesignExtensions[]` order, so `expsite_app` must be listed before the theme extension for its copies to win.

## Scenario: hold project settings

Put project-wide INI additions in this extension's `settings/` directory as `<file>.ini.append.php`. They are merged once the extension is active, and can still be trumped by siteaccess and `settings/override/` files (see Customization below). Typical examples: `override.ini.append.php` rules, `content.ini` tweaks, project `site.ini` URL and mail settings.

## Scenario: add project PHP helpers

1. Create `extension/expsite_app/classes/<yourclass>.php`.
2. Regenerate autoloads: `php bin/php/ezpgenerateautoloads.php -e`.
3. Use the class from templates (via a template operator extension) or modules.

Keeping helpers here keeps `explayouts*` / `expsite_*` feature extensions generic and reusable across projects.

## Customization

### Settings layer

This extension ships `settings/site.ini.append.php` declaring `[DesignSettings]` `DesignExtensions[]=expsite_app` and `[ExtensionSettings]` `ActiveExtensions[]=expsite_app`. Overrides follow the standard INI cascade (later wins):

1. Extension defaults — `extension/expsite_app/settings/*.ini.append.php`
2. Siteaccess — `settings/siteaccess/<siteaccess>/*.ini.append.php`
3. Extension siteaccess — `extension/<ext>/settings/siteaccess/<siteaccess>/*.ini.append.php`
4. Global override — `settings/override/*.ini.append.php`

Because `expsite_app` is itself the top customization layer, most projects put their per-site values directly in this extension's `settings/`, reserving `settings/override/` for machine-specific values that must not be committed.

### Template layer

`design/simple/override/templates/` is shipped empty on purpose. Any template of the `simple` design (from `sevenx_themes_simple`) can be shadowed by placing a copy at the same relative path in this extension's `design/simple/` tree, or by `override.ini`-matched templates in `design/simple/override/templates/`. Other extensions can in turn override `expsite_app` by registering their design extension earlier in `DesignExtensions[]` — the cascade is ordinary INI-ordered design resolution.

### PHP layer

`expSiteApp` is a stateless placeholder; extend the extension by adding new classes, not by subclassing it. If a project needs a different design extension name, add its own helper alongside rather than patching `designExtensionName()` — feature code that consults the design extension name should read INI settings where possible.
