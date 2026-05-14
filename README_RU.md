# Aspid.Claude.Plugins

[![Releases](https://img.shields.io/github/v/release/VPDPersonal/Aspid.Claude.Plugins?label=Release&labelColor=254d2c&color=4fa35d&include_prereleases&sort=semver)](https://github.com/VPDPersonal/Aspid.Claude.Plugins/releases)
[![License](https://img.shields.io/github/license/VPDPersonal/Aspid.Claude.Plugins?label=License&labelColor=254d2c&color=4fa35d)](LICENSE)

[English version](README.md)

Маркетплейс **Aspid** для плагинов [Claude Code](https://docs.claude.com/en/docs/claude-code).

Общий маркетплейс, объединяющий два типа плагинов:

- **Универсальные плагины для Claude Code** — скиллы, команды, агенты и хуки, облегчающие повседневную работу в Claude Code независимо от проекта.
- **Плагины для пакетов Aspid** — плагины-компаньоны для конкретных пакетов семейства **Aspid** (например, `Aspid.FastTools`), которые учат Claude Code правилам и API каждого пакета.

Плагины лежат в `plugins/<имя>/`, манифест маркетплейса — в `.claude-plugin/marketplace.json`.

## Установка

Добавьте репозиторий как маркетплейс в Claude Code:

```sh
/plugin marketplace add VPDPersonal/Aspid.Claude.Plugins
```

Установите конкретный плагин:

```sh
/plugin install aspid-fasttools@aspid-claude-plugins
```

Или откройте UI маркетплейса:

```sh
/plugin
```

## Плагины

В маркетплейсе живут как универсальные плагины для Claude Code, так и плагины для конкретных пакетов Aspid. Список будет пополняться.

### Плагины для пакетов Aspid

| Плагин | Целевой пакет | Что делает |
|---|---|---|
| [`aspid-fasttools`](plugins/aspid-fasttools/README_RU.md) | [Aspid.FastTools](https://github.com/VPDPersonal/Aspid.FastTools) | Скиллы Claude Code для пакета Aspid.FastTools: создание `IId`-структур и полей с `[UniqueId]` (`aspid-id-struct`), вставка `this.Marker()` для профайлинга (`aspid-profiler-marker`) и сборка editor- или runtime-UI на fluent-расширениях `VisualElement` (`aspid-visual-element-fluent`). |

### Универсальные плагины

_Пока нет — скоро появятся._

## Структура репозитория

```
Aspid.Claude.Plugins/
├── .claude-plugin/
│   └── marketplace.json     # манифест маркетплейса (имя, описание, список плагинов)
├── plugins/
│   ├── aspid-fasttools/     # плагин для пакета Aspid.FastTools
│   │   ├── .claude-plugin/
│   │   │   └── plugin.json  # метаданные плагина
│   │   ├── skills/          # aspid-id-struct, aspid-profiler-marker, aspid-visual-element-fluent
│   │   ├── CHANGELOG.md
│   │   ├── README.md
│   │   └── README_RU.md
│   └── <other-plugin>/      # плагин для другого пакета Aspid или универсальный плагин для Claude Code
├── LICENSE
├── README.md
└── README_RU.md
```

## Как добавить новый плагин

1. Создайте `plugins/<имя>/.claude-plugin/plugin.json` с полями `name`, `description`, `author`.
2. Добавьте нужное: `skills/<skill>/SKILL.md`, `commands/<cmd>.md`, `agents/<agent>.md`, `hooks/`.
3. Положите `README.md` в директорию плагина.
4. Зарегистрируйте плагин в `.claude-plugin/marketplace.json` в массиве `plugins` — для локальных плагинов укажите `"source": "./plugins/<имя>"`.

## Лицензия

MIT — см. [LICENSE](LICENSE).
