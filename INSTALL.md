# Installing expsite_app

## Requirements

- Exponential CMS (legacy) installation, PHP 8.1 or newer.
- A site design named `simple` (normally provided by `sevenx_themes_simple`) if you intend to use the shipped `design/simple/override/templates/` directory.

## Steps

1. Place the extension in `extension/expsite_app`.

2. Activate it in `settings/override/site.ini.append.php` (site-wide) or in a siteaccess `site.ini.append.php`:

   ```ini
   [ExtensionSettings]
   ActiveExtensions[]=expsite_app
   ```

   For a single siteaccess use `ActiveAccessExtensions[]` instead. Do not rely on the `ActiveExtensions[]` line inside the extension's own `settings/site.ini.append.php` — an extension cannot activate itself; that file is only read after the extension is already active.

3. Make sure `expsite_app` is listed as a design extension so its `design/` tree joins the cascade. The clean way is a `design.ini.append.php` override:

   ```ini
   [ExtensionSettings]
   DesignExtensions[]=expsite_app
   ```

   List `expsite_app` before the theme extension so its overrides win.

4. Regenerate the extension autoloads:

   ```bash
   php bin/php/ezpgenerateautoloads.php -e
   ```

5. Clear all caches:

   ```bash
   php bin/php/ezcache.php --clear-all --purge --allow-root-user
   ```

## Verifying

Drop a test override into `extension/expsite_app/design/simple/override/templates/`, clear caches, and confirm it is picked up ahead of the `sevenx_themes_simple` copy.
