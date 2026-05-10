# Aspid.FastTools.Claude — Claude Code plugin

[Русская версия](README_RU.md)

> **Alpha (`0.1.0`)** — skill triggers, descriptions and structure may change between
> minor versions as real-world usage feedback comes in. Pin a specific commit or tag
> if you need stability.

A Claude Code plugin that helps Claude Code work correctly with the
[**Aspid.FastTools**](https://github.com/VladislavPanin/Aspid.FastTools) Unity package
(`com.aspid.fasttools`) inside your own Unity projects.

The plugin ships **3 skills** that activate automatically when you ask Claude Code
to do common tasks:

| Skill | Triggers on requests like |
|---|---|
| `aspid-id-struct` | "create an id struct", "add `[UniqueId]` to X", "make X a project-wide id" |
| `aspid-profiler-marker` | "profile this method", "add `this.Marker()`", "instrument with ProfilerMarker" |
| `aspid-visual-element-fluent` | "build editor UI with UIToolkit", "use fluent `VisualElement`", "style this `VisualElement`" |

The plugin **does not** include skills for internal-only conventions (USS BEM grammar,
custom `AspidStyles` palette) — those are for contributors of the package itself,
not consumers.

## Requirements

- Unity 2022.3+
- The Aspid.FastTools package installed in your project (`com.aspid.fasttools`)

## Installation

### Option A — Claude Code plugin marketplace

```
/plugin install aspid-fasttools
```

### Option B — Direct from Git

```
/plugin install github:VladislavPanin/Aspid.FastTools.Claude
```

### Option C — Git submodule (for contributors)

```bash
git submodule add https://github.com/VladislavPanin/Aspid.FastTools.Claude.git \
    .claude/plugins/aspid-fasttools
```

## What's inside

```
Aspid.FastTools.Claude/
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
