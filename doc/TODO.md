# expsite_app TODO

Code-observed gaps; no promises attached.

- `settings/site.ini.append.php` declares `DesignExtensions[]` under `site.ini` `[DesignSettings]`, but design extensions are read from `design.ini` `[ExtensionSettings]` `DesignExtensions[]` — the shipped declaration is ineffective. Ship a `settings/design.ini.append.php` instead.
- The `[ExtensionSettings]` `ActiveExtensions[]=expsite_app` line in the extension's own settings cannot activate the extension (self-activation is impossible); it is documentation at best and should be removed or commented.
- `expSiteApp` contains only the `designExtensionName()` placeholder; no real helpers exist yet.
- `design/simple/override/templates/` is empty and the extension ships no `override.ini` rules; the override wiring is left entirely to the project.
- No `autoloads/` class map; `expSiteApp` relies on the generated extension autoloads only.
