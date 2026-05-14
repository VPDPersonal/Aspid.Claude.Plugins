# Aspid.Claude.Plugins

[![Releases](https://img.shields.io/github/v/release/VPDPersonal/Aspid.Claude.Plugins?label=Release&labelColor=254d2c&color=4fa35d&include_prereleases&sort=semver)](https://github.com/VPDPersonal/Aspid.Claude.Plugins/releases)
[![License](https://img.shields.io/github/license/VPDPersonal/Aspid.Claude.Plugins?label=License&labelColor=254d2c&color=4fa35d)](LICENSE)

[Русская версия](README_RU.md)

Personal [Claude Code](https://docs.claude.com/en/docs/claude-code) marketplace by Vladislav Panin.

A shared marketplace for Claude Code plugins across the **Aspid** package family and adjacent workflows — not tied to any single package. Each Aspid package (or general-purpose tooling) can ship its own plugin here. Plugins live in `plugins/<name>/`; the marketplace manifest is at `.claude-plugin/marketplace.json`.

## Installation

Add this repository as a marketplace in Claude Code:

```sh
/plugin marketplace add VPDPersonal/Aspid.Claude.Plugins
```

Then install a specific plugin:

```sh
/plugin install aspid-fasttools@aspid-claude-plugins
```

Or browse the marketplace UI:

```sh
/plugin
```

## Plugins

The marketplace hosts plugins for any Aspid package (and related general-purpose Claude Code tooling). The list grows as new packages get their own plugin.

| Plugin | Target package | What it does |
|---|---|---|
| [`aspid-fasttools`](plugins/aspid-fasttools/README.md) | [Aspid.FastTools](https://github.com/VPDPersonal/Aspid.FastTools) | Claude Code skills for the Aspid.FastTools Unity package: scaffolding `IId`/`[UniqueId]` structs (`aspid-id-struct`), inserting `this.Marker()` profiler call sites (`aspid-profiler-marker`), and building editor UI with the fluent `VisualElement` extensions (`aspid-visual-element-fluent`). |

## Repository layout

```
Aspid.Claude.Plugins/
├── .claude-plugin/
│   └── marketplace.json     # marketplace manifest (name, description, plugin list)
├── plugins/
│   ├── aspid-fasttools/     # plugin for the Aspid.FastTools package
│   │   ├── .claude-plugin/
│   │   │   └── plugin.json  # plugin metadata
│   │   ├── skills/          # aspid-id-struct, aspid-profiler-marker, aspid-visual-element-fluent
│   │   ├── README.md
│   │   └── README_RU.md
│   └── <other-plugin>/      # plugins for other Aspid packages or general tooling
├── LICENSE
└── README.md
```

## Adding a new plugin

1. Create `plugins/<name>/.claude-plugin/plugin.json` with `name`, `description`, `author`.
2. Add `skills/<skill>/SKILL.md`, `commands/<cmd>.md`, `agents/<agent>.md`, or `hooks/` as needed.
3. Add a `README.md` in the plugin directory.
4. Register the plugin in `.claude-plugin/marketplace.json` under the `plugins` array — set `"source": "./plugins/<name>"` for in-repo plugins.

## License

MIT — see [LICENSE](LICENSE).
