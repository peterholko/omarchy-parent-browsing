# Browsing history

Omarchy Quattro plugin for a child installation configured by [PR9750](https://github.com/omacom/omarchy/pull/9750). It uses the existing OS parent password through polkit. No password is stored in the plugin.

This repository contains the user interface. It requires the separately installed `omarchy-parent-addons-browsing` and `omarchy-parent-addons-core` packages. The [current backend release](https://github.com/peterholko/omarchy-parent-addons/releases/tag/v0.1.1) includes matching package-based UI copies. Plugin v0.1.1 also works with the v0.1.0 backend; that backend does not need to be rebuilt for the panel visibility fix. The backend does not depend on the Omarchy Kids distribution or replace Omarchy. Linux installation and authentication checks are still pending; see the [validation record](https://github.com/peterholko/omarchy-parent-addons/blob/v0.1.1/VALIDATION.md).

## Install

First follow the [backend build and installation instructions](https://github.com/peterholko/omarchy-parent-addons#build-and-install-on-a-pr9750-child-laptop) as the normal desktop user. Select `./install browsing` for this feature, or `./install dns browsing` for both. If you already installed the other feature, use `./install --upgrade browsing` from the matching backend release. The installer requests the existing parent password. No ISO or reinstall is needed.

Then install this interface, still as the normal desktop user:

```bash
omarchy plugin add https://github.com/peterholko/omarchy-parent-browsing.git --enable
omarchy bar put io.github.peterholko.parent-browsing --section right
omarchy-shell shell summon io.github.peterholko.parent-browsing
```

Accept Omarchy's plugin confirmation and select the right bar section when prompted. If you already have a package-based copy with this ID, remove that user interface with `omarchy plugin remove io.github.peterholko.parent-browsing` before adding the Git copy. This leaves the backend and its settings in place.

## Enable and use

Installing or enabling the plugin does not enable filtering or collection. Use the explicit controls inside its panel. Private reports require parent authentication and clear when the window closes or after two minutes. DNS filtering changes this laptop's resolver, firewall and supported browser policies. Browsing logs are opt-in; tell the child when collection is enabled.

The first collection includes existing history, followed by collection every minute. Supported history databases are Chromium, Chrome, Brave, Edge and Firefox. The collector records URLs and titles; it does not capture every network request or establish time spent watching a video.

The [backend README](https://github.com/peterholko/omarchy-parent-addons#enable-features-explicitly) documents terminal commands, browser support, policy limitations and the manual `apply` step after installing or reinstalling browsers.

## Upgrade

Follow the [backend upgrade steps](https://github.com/peterholko/omarchy-parent-addons#upgrade) first when a release changes privileged code. Then update this interface:

```bash
omarchy plugin update io.github.peterholko.parent-browsing
omarchy restart shell
```

A plugin update cannot upgrade its root-owned backend. Package-based UI copies use the backend release's `./plugins install browsing --upgrade` instead of the Git updater.

Version 0.1.1 fixes the bar button opening an invisible window. For existing Git installs with the v0.1.0 backend, the two commands above are sufficient for this fix.

## Disable and remove

Disable the backend with `omarchy parent browsing off --user "$(id -un)"`. From the backend source directory, run `./remove browsing` to restore owned integration files and remove the backend package. Remove this interface with:

```bash
omarchy plugin remove io.github.peterholko.parent-browsing
```

Logs and settings are retained. Removing only the shell plugin leaves the backend running. See the [complete removal procedure](https://github.com/peterholko/omarchy-parent-addons#disable-and-remove).

## Source

This interface is generated from the [shared source](https://github.com/peterholko/omarchy-parent-addons/tree/v0.1.1/ui) by `packaging/export.py`. Changes should be made there and exported to this repository. Both exported plugin roots pass PR9750's plugin validator. Portable Qt tests use inert Quickshell transport stubs; real Linux runtime validation remains pending.

MIT. Extracted from [Omarchy Kids](https://github.com/peterholko/omarchy-kids) with standalone PR9750 adapters. [SOURCE.json](SOURCE.json) records the original revision and provenance; [LICENSE](LICENSE) retains the original license notice.
