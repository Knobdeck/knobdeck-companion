# KnobDeck companions

Small add-ons that run *inside* other apps so [KnobDeck](https://knobdeck.harshalkudale.com) can
control them through a real interface instead of keyboard shortcuts. A KnobDeck plugin from
[knobdeck-plugins](https://github.com/Knobdeck/knobdeck-plugins) talks to each one: the editor
companions, the Blender add-on and the Ableton Live Remote Script link to KnobDeck's companion hub (the editor companions, for older
KnobDeck versions, also expose a local action server), and the browser extension connects out to
the browser plugin's local server.

Every companion lives in its own repo with its own CI, versions and one-click store releases. This
repo holds what they share: the protocol, the privacy statement and the compatibility test driver.

Companion metadata (display name, store links, minimum companion version per plugin) lives in
[knobdeck-registry](https://github.com/Knobdeck/knobdeck-registry) under `catalog/companions/<id>.toml`
and is served as `/v2/companions.json`. Each companion's release workflow tells the catalog to rebuild.

| Companion | Repo | Runs in | KnobDeck plugin | Stores |
|---|---|---|---|---|
| KnobDeck VS Code Companion | [knobdeck-vscode-companion](https://github.com/Knobdeck/knobdeck-vscode-companion) | VS Code, VSCodium, Cursor, Windsurf | `vscode` | Visual Studio Marketplace, Open VSX |
| KnobDeck Companion for JetBrains IDEs | [knobdeck-intellij-companion](https://github.com/Knobdeck/knobdeck-intellij-companion) | IntelliJ IDEA, Android Studio, PyCharm, WebStorm and other JetBrains IDEs (2024.3+) | `intellij` | JetBrains Marketplace |
| KnobDeck Browser Companion | [knobdeck-browser-companion](https://github.com/Knobdeck/knobdeck-browser-companion) | Chrome, Edge, Brave and other Chromium browsers; Firefox, Zen | `browser-plugin` | Chrome Web Store (also for Edge, Brave), addons.mozilla.org |
| KnobDeck add-on for Blender | [knobdeck-blender-companion](https://github.com/Knobdeck/knobdeck-blender-companion) | Blender 4.2 and newer | `blender` | extensions.blender.org |
| KnobDeck Remote Script for Ableton Live | [knobdeck-ableton-companion](https://github.com/Knobdeck/knobdeck-ableton-companion) | Ableton Live 11 and newer (Windows, macOS) | `ableton` | none (the plugin's setup installs it from the GitHub release) |

- [PROTOCOL.md](PROTOCOL.md): how the plugins talk to the companions. Ids are a contract: add,
  never rename.
- [PRIVACY.md](PRIVACY.md): what the companions do with data (nothing leaves your computer).
- [`compat/`](compat): drives the real Rust plugins against a companion, the way the KnobDeck host
  does. Each companion's CI runs a copy of it (`test/compat/` or `test-support/compat/`); change it
  here first, then copy it over.
  The Blender add-on and the Ableton Live Remote Script are Python and have their own equivalent
  (`tests/e2e_plugin.py` / `tests/test_e2e_plugin.py`, with a fake companion hub).

## Building, testing and releasing

See each repo's `CONTRIBUTING.md`. They all release the same way: bump the version, merge to
`master`, then **Actions → Release → Run workflow**. That runs CI, creates the tag `v<version>` and a
GitHub release with the packages, and publishes to the stores.
