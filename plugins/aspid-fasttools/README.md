# Aspid.FastTools.Claude — Claude Code plugin

[![Releases](https://img.shields.io/github/v/release/VPDPersonal/Aspid.FastTools.Claude?label=Release&labelColor=254d2c&color=4fa35d&include_prereleases&sort=semver)](https://github.com/VPDPersonal/Aspid.FastTools.Claude/releases)
[![License](https://img.shields.io/github/license/VPDPersonal/Aspid.FastTools.Claude?label=License&labelColor=254d2c&color=4fa35d)](LICENSE)

[Русская версия](README_RU.md)

> **Alpha (`0.1.0`)** — skill triggers, descriptions and structure may change between
> minor versions as real-world usage feedback comes in. Pin a specific commit or tag
> if you need stability.

A Claude Code plugin that helps Claude Code work correctly with the
[**Aspid.FastTools**](https://github.com/VPDPersonal/Aspid.FastTools) Unity package
(`com.aspid.fasttools`) inside your own Unity projects.

The plugin ships **3 skills** that activate automatically when you ask Claude Code
to do common tasks:

| Skill | Triggers on requests like |
|---|---|
| `aspid-id-struct` | "create an id struct", "add `[UniqueId]` to X", "make X a project-wide id" |
| `aspid-profiler-marker` | "profile this method", "add `this.Marker()`", "instrument with ProfilerMarker" |
| `aspid-visual-element-fluent` | "build editor UI with UIToolkit", "use fluent `VisualElement`", "style this `VisualElement`" |

## Requirements

- Unity 2022.3+
- The Aspid.FastTools package installed in your project (`com.aspid.fasttools`)

## Installation

This plugin is published through the [`aspid-fasttools-claude`](https://github.com/VPDPersonal/Aspid.FastTools.Claude) marketplace:

```
/plugin marketplace add VPDPersonal/Aspid.FastTools.Claude
/plugin install aspid-fasttools@aspid-fasttools-claude
```

## What's inside

```
plugins/aspid-fasttools/
├── .claude-plugin/plugin.json
├── README.md
├── README_RU.md
└── skills/
    ├── aspid-id-struct/SKILL.md
    ├── aspid-profiler-marker/SKILL.md
    └── aspid-visual-element-fluent/SKILL.md
```

## Versioning

This plugin tracks the public API of `com.aspid.fasttools` but is versioned
independently. A `0.x` plugin is compatible with all `com.aspid.fasttools` versions
that expose the same public types (`IId`, `[UniqueId]`, `IdRegistry`, `SerializableType`,
`EnumValues<TValue>`, `this.Marker()`, fluent `VisualElement` extensions).

## License

MIT
