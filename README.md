# Browsing history

Omarchy Quattro plugin for a child installation configured by [PR9750](https://github.com/omacom/omarchy/pull/9750). It uses the existing OS parent password through polkit. No password is stored in the plugin.

This repository contains an on-demand panel, with no bar or system tray widget. It requires the separately installed `omarchy-parent-addons-browsing` and `omarchy-parent-addons-core` packages. The [current backend release](https://github.com/peterholko/omarchy-parent-addons/releases/tag/v0.1.3) includes the `omarchy parent browsing ui` command and matching package-based UI copies. The backend does not depend on the Omarchy Kids distribution or replace Omarchy. Linux installation and authentication checks are still pending; see the [validation record](https://github.com/peterholko/omarchy-parent-addons/blob/v0.1.3/VALIDATION.md).

## Install

First follow the [backend build and installation instructions](https://github.com/peterholko/omarchy-parent-addons#build-and-install-on-a-pr9750-child-laptop) as the normal desktop user. Select `./install browsing` for this feature, or `./install dns browsing` for both. If you already installed the other feature, use `./install --upgrade browsing` from the matching backend release. The installer requests the existing parent password. No ISO or reinstall is needed.

Then install this interface, still as the normal desktop user:

```bash
omarchy plugin add https://github.com/peterholko/omarchy-parent-browsing.git --enable
omarchy parent browsing ui
```

Accept Omarchy's plugin confirmation. Enabling this panel does not place anything in the bar. If you already have a package-based copy with this ID, remove that user interface with `omarchy plugin remove io.github.peterholko.parent-browsing` before adding the Git copy. This leaves the backend and its settings in place.

## Enable and use

Open the panel as the desktop user with `omarchy parent browsing ui`, without sudo. Opening it does not prompt for a password or enable filtering or collection. Use the explicit controls inside its panel. Private reports and settings changes require parent authentication; reports clear when the window closes or after two minutes. DNS filtering changes this laptop's resolver, firewall and supported browser policies. Browsing logs are opt-in; tell the child when collection is enabled.

The first collection includes existing history, followed by collection every minute. Supported history databases are Chromium, Chrome, Brave, Edge and Firefox. The collector records URLs and titles; it does not capture every network request or establish time spent watching a video.

The [backend README](https://github.com/peterholko/omarchy-parent-addons#enable-features-explicitly) documents terminal commands, browser support, policy limitations and the manual `apply` step after installing or reinstalling browsers.

## Upgrade

Follow the [backend upgrade steps](https://github.com/peterholko/omarchy-parent-addons#upgrade) for the new `ui` command, then update this interface:

```bash
omarchy plugin update io.github.peterholko.parent-browsing
omarchy restart shell
```

After the shell has reappeared, move an existing bar entry to a panel registration by disabling and re-enabling the updated plugin once:

```bash
omarchy plugin disable io.github.peterholko.parent-browsing &&
omarchy plugin enable io.github.peterholko.parent-browsing
```

This removes the old bar button and keeps the panel available to commands. DNS filtering and history collection continue with their existing settings. A plugin update cannot upgrade its root-owned backend. Package-based UI copies use the backend release's `./plugins install browsing --upgrade` instead of the Git updater, followed by the same shell restart and disable/enable steps.

The lower-level command `omarchy-shell shell summon io.github.peterholko.parent-browsing` also opens this panel and works with older backend packages. The DNS domain-blocking fix from v0.1.2 still requires a backend upgrade; updating only this interface cannot change DNS behavior.

## Disable and remove

Disable the backend with `omarchy parent browsing off --user "$(id -un)"`. From the backend source directory, run `./remove browsing` to restore owned integration files and remove the backend package. Remove this interface with:

```bash
omarchy plugin remove io.github.peterholko.parent-browsing
```

Logs and settings are retained. Removing only the shell plugin leaves the backend running. See the [complete removal procedure](https://github.com/peterholko/omarchy-parent-addons#disable-and-remove).

## Source

This interface is generated from the [shared source](https://github.com/peterholko/omarchy-parent-addons/tree/v0.1.3/ui) by `packaging/export.py`. Changes should be made there and exported to this repository. Both exported plugin roots pass PR9750's plugin validator. Portable Qt tests use inert Quickshell transport stubs; real Linux runtime validation remains pending.

MIT. Extracted from [Omarchy Kids](https://github.com/peterholko/omarchy-kids) with standalone PR9750 adapters. [SOURCE.json](SOURCE.json) records the original revision and provenance; [LICENSE](LICENSE) retains the original license notice.
