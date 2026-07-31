Exponential Site App
===================

General description
-------------------

Exponential Site App (extension name: `expsite_app`) is the application-level
glue for Exponential CMS (eZ Publish Legacy) sites: the designated place for
per-project design overrides and installation customizations, layered over
the `simple` site design shipped by `sevenx_themes_simple`. Site projects
put their template overrides and project-specific settings here instead of
editing the theme or feature extensions. It provides the following
capabilities:

- Template override layer - Shadow any template of the managed `simple`
  design by placing a copy at the same relative path, or via `override.ini`
  matched templates, without touching composer-managed theme code.
- Project settings container - Hold project-wide INI additions
  (`override.ini` rules, `content.ini` tweaks, project `site.ini` URL and
  mail settings) in one committed, per-project extension.
- Project PHP helpers - Host site-specific helper classes so feature
  extensions (`explayouts*`, `expsite_*`) stay generic and reusable across
  projects.

How it fits in: `sevenx_themes_simple` provides the composer-managed,
read-only `simple` design. `expsite_app` sits on top of it in the design
cascade so that a project can override any `simple` template without
touching managed code. Feature extensions stay generic; anything
site-specific belongs in this extension.

Features
--------

Exponential Site App provides the following features in detail:

What is included

- `classes/expsiteapp.php` - the `expSiteApp` helper class. Currently a
  placeholder exposing `designExtensionName()`, which returns
  `'expsite_app'`.
- `design/simple/override/templates/` - empty override directory for the
  `simple` site design; project template overrides go here.
- `settings/site.ini.append.php` - declares the extension in
  `[DesignSettings]` `DesignExtensions[]` and `[ExtensionSettings]`
  `ActiveExtensions[]` (see [doc/TODO.md](doc/TODO.md) for caveats about
  these declarations).

Key classes

| Class | File | Purpose |
| --- | --- | --- |
| `expSiteApp` | `classes/expsiteapp.php` | Placeholder helper; `designExtensionName()` returns the extension name |

Design cascade behaviour

- The design cascade searches design extensions in `DesignExtensions[]`
  order, so `expsite_app` must be listed before the theme extension for its
  template copies to win.
- Direct overrides go to
  `extension/expsite_app/design/simple/templates/<path>/<template>.tpl`.
- Full-template overrides driven by `override.ini` conditions go into
  `extension/expsite_app/design/simple/override/templates/`.
- Other extensions can in turn override `expsite_app` by registering their
  design extension earlier in `DesignExtensions[]` - the cascade is
  ordinary INI-ordered design resolution.

Settings layering

- Extension `settings/` files are merged once the extension is active and
  can still be trumped by siteaccess and `settings/override/` files.
- Because `expsite_app` is itself the top customization layer, most
  projects put their per-site values directly in this extension's
  `settings/`, reserving `settings/override/` for machine-specific values
  that must not be committed.

Version
-------

- The current version of Exponential Site App is 1.0.0
- Last Major update: July 30, 2026

Copyright
---------

- Exponential Site App is copyright 1998 - 2026 7x
- See: [LICENSE.md](LICENSE.md) for more information on the terms of the copyright and license

License
-------

Exponential Site App is licensed under the GNU General Public License.

The complete license agreement is included in the LICENSE.md file.

Exponential Site App is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 2 of the License or at your
option a later version.

Exponential Site App is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

The GNU GPL gives you the right to use, modify and redistribute
Exponential Site App under certain conditions. The GNU GPL license
is distributed with the software, see the file LICENSE.md.

It is also available at https://www.gnu.org/licenses/gpl.txt

You should have received a copy of the GNU General Public License
along with Exponential Site App in LICENSE.md.  If not, see https://www.gnu.org/licenses/.

Using Exponential Site App under the terms of the GNU GPL is free (as in freedom).

For more information or questions please contact info@se7enx.com

Requirements
------------

The following requirements exists for using Exponential Site App extension:

Exponential CMS / eZ Publish Legacy version
- Make sure you use Exponential 6 (eZ Publish Legacy) or higher.

PHP version
- Make sure you have PHP 8.1 or higher.

Site design
- A site design named `simple` (normally provided by `sevenx_themes_simple`)
  if you intend to use the shipped `design/simple/override/templates/`
  directory.

Installation
------------

Installation is the standard legacy extension procedure with one design
step: place the extension in `extension/expsite_app`, activate it via
`[ExtensionSettings]` `ActiveExtensions[]` (or `ActiveAccessExtensions[]`
for a single siteaccess), register it as a design extension in
`DesignExtensions[]` (listed before the theme extension so its overrides
win), regenerate the extension autoloads and clear all caches.

Note: do not rely on the `ActiveExtensions[]` line inside the extension's
own `settings/site.ini.append.php` - an extension cannot activate itself;
that file is only read after the extension is already active.

See the complete step-by-step instructions in [INSTALL.md](INSTALL.md).

Usage
-----

`expsite_app` is intentionally almost empty: it is the container a site
project fills with its own overrides and glue.

The helper class:

```php
$name = expSiteApp::designExtensionName(); // 'expsite_app'
```

`classes/expsiteapp.php` is a placeholder for per-project site application
helpers. Add your project's static helpers to this extension (in new class
files under `classes/`) rather than extending kernel or theme extensions.

Override a template from the `simple` design by copying it from the theme
(for example `extension/sevenx_themes_simple/design/simple/...`) into the
same relative path under this extension:

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

Add project PHP helpers by creating
`extension/expsite_app/classes/<yourclass>.php`, regenerating autoloads
with `php bin/php/ezpgenerateautoloads.php -e`, and using the class from
templates (via a template operator extension) or modules.

See [doc/USAGE.md](doc/USAGE.md) for the full walk-through and the
customization guide covering all three layers: the settings layer (the INI
cascade and where per-site values belong), the template layer (how the
`design/simple/` tree shadows the theme and how other extensions can shadow
this one) and the PHP layer (extend the extension by adding new classes,
not by subclassing the placeholder).

Documentation
-------------

| Document | Description |
| --- | --- |
| [INSTALL.md](INSTALL.md) | Activation and design cascade setup |
| [doc/USAGE.md](doc/USAGE.md) | Override and customization walk-through plus the settings / template / PHP customization layers |
| [doc/FAQ.md](doc/FAQ.md) | Answers to the most common questions |
| [doc/TODO.md](doc/TODO.md) | Known gaps and planned improvements |
| [doc/SUPPORT.md](doc/SUPPORT.md) | How and where to get help |
| [LICENSE.md](LICENSE.md) | The complete GNU General Public License agreement |

Troubleshooting
---------------

Read the FAQ
- Some problems are more common than others. The most common ones are listed in the [doc/FAQ.md](doc/FAQ.md).

Use our support systems
- If you have any questions not handled by this document or the FAQ you can reach us through [se7enx.com](https://se7enx.com)
- If you find a bug or defect, please report it to the [Exponential Site App: Issue Tracker](https://github.com/se7enxweb/expsite_app/issues)
