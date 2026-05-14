# Aspid.FastTools.Claude

[![Releases](https://img.shields.io/github/v/release/VPDPersonal/Aspid.FastTools.Claude?label=Release&labelColor=254d2c&color=4fa35d&include_prereleases&sort=semver)](https://github.com/VPDPersonal/Aspid.FastTools.Claude/releases)
[![License](https://img.shields.io/github/license/VPDPersonal/Aspid.FastTools.Claude?label=License&labelColor=254d2c&color=4fa35d)](LICENSE)

[English version](README.md)

Личный маркетплейс плагинов для [Claude Code](https://docs.claude.com/en/docs/claude-code) от Vladislav Panin.

Набор плагинов вокруг Unity-пакета [**Aspid.FastTools**](https://github.com/VPDPersonal/Aspid.FastTools) и связанных Unity-воркфлоу. Плагины лежат в `plugins/<имя>/`, манифест маркетплейса — в `.claude-plugin/marketplace.json`.

## Установка

Добавьте репозиторий как маркетплейс в Claude Code:

```sh
/plugin marketplace add VPDPersonal/Aspid.FastTools.Claude
```

Установите конкретный плагин:

```sh
/plugin install aspid-fasttools@aspid-fasttools-claude
```

Или откройте UI маркетплейса:

```sh
/plugin
```

## Плагины

| Плагин | Что делает |
|---|---|
| [`aspid-fasttools`](plugins/aspid-fasttools/README_RU.md) | Скиллы Claude Code для пакета Aspid.FastTools: создание `IId`/`[UniqueId]`-структур (`aspid-id-struct`), вставка `this.Marker()` для профайлинга (`aspid-profiler-marker`) и сборка редактор-UI на fluent-расширениях `VisualElement` (`aspid-visual-element-fluent`). |

## Структура репозитория

```
Aspid.FastTools.Claude/
├── .claude-plugin/
│   └── marketplace.json     # манифест маркетплейса (имя, описание, список плагинов)
├── plugins/
│   └── aspid-fasttools/
│       ├── .claude-plugin/
│       │   └── plugin.json  # метаданные плагина
│       ├── skills/          # aspid-id-struct, aspid-profiler-marker, aspid-visual-element-fluent
│       ├── README.md
│       └── README_RU.md
├── LICENSE
└── README.md
```

## Как добавить новый плагин

1. Создайте `plugins/<имя>/.claude-plugin/plugin.json` с полями `name`, `description`, `author`.
2. Добавьте нужное: `skills/<skill>/SKILL.md`, `commands/<cmd>.md`, `agents/<agent>.md`, `hooks/`.
3. Положите `README.md` в директорию плагина.
4. Зарегистрируйте плагин в `.claude-plugin/marketplace.json` в массиве `plugins` — для локальных плагинов укажите `"source": "./plugins/<имя>"`.

## Лицензия

MIT — см. [LICENSE](LICENSE).
