# expsite_app FAQ

## Why is the extension nearly empty?

By design. It is the per-project customization holder: template overrides, project settings and project helper classes live here so the theme (`sevenx_themes_simple`) and the feature extensions (`explayouts*`, `expsite_*`) stay untouched and upgradeable.

## Why doesn't the ActiveExtensions[] line in its settings activate it?

An extension's own `settings/site.ini.append.php` is only read after the extension is active — an extension cannot activate itself. Activate `expsite_app` from `settings/override/site.ini.append.php` or a siteaccess `site.ini.append.php`.

## My template override is not picked up — what should I check?

Three things: `expsite_app` is active, it is registered in `design.ini` `[ExtensionSettings]` `DesignExtensions[]` ahead of the theme extension, and caches were cleared (`php bin/php/ezcache.php --clear-all --purge --allow-root-user`). Note that the shipped `settings/site.ini.append.php` puts `DesignExtensions[]` under `site.ini` `[DesignSettings]`, which the design resolver does not read — see `TODO.md`.

## What does expSiteApp actually do?

Currently only `expSiteApp::designExtensionName()` exists, returning `'expsite_app'`. It marks the class as the future home for per-project helpers.

## Which design does the override directory target?

`design/simple/override/templates/` targets the `simple` site design. If your siteaccess uses another `SiteDesign`, create the matching `design/<yourdesign>/` tree in this extension.
