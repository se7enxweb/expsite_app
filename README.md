# expsite_app

Application-level glue for Exponential CMS (legacy) sites: the designated place for per-project design overrides and installation customizations, layered over the `simple` site design shipped by `sevenx_themes_simple`. Site projects put their template overrides and project-specific settings here instead of editing the theme or feature extensions.

## What is included

- `classes/expsiteapp.php` — the `expSiteApp` helper class. Currently a placeholder exposing `designExtensionName()`, which returns `'expsite_app'`.
- `design/simple/override/templates/` — empty override directory for the `simple` site design; project template overrides go here.
- `settings/site.ini.append.php` — declares the extension in `[DesignSettings]` `DesignExtensions[]` and `[ExtensionSettings]` `ActiveExtensions[]` (see `doc/TODO.md` for caveats about these declarations).

## Key classes

| Class | File | Purpose |
| --- | --- | --- |
| `expSiteApp` | `classes/expsiteapp.php` | Placeholder helper; `designExtensionName()` returns the extension name |

## How it fits in

`sevenx_themes_simple` provides the composer-managed, read-only `simple` design. `expsite_app` sits on top of it in the design cascade so that a project can override any `simple` template without touching managed code. Feature extensions (`explayouts*`, `expsite_*`) stay generic; anything site-specific belongs in this extension.

## Documentation

- `INSTALL.md` — activation and design cascade setup
- `doc/USAGE.md` — how to add overrides and project customizations
- `doc/FAQ.md` — common questions
- `doc/TODO.md` — known gaps
- `doc/SUPPORT.md` — how to get help
