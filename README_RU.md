# Aspid.Claude.Plugins

[![Releases](https://img.shields.io/github/v/release/VPDPersonal/Aspid.Claude.Plugins?label=Release&labelColor=254d2c&color=4fa35d&include_prereleases&sort=semver)](https://github.com/VPDPersonal/Aspid.Claude.Plugins/releases)
[![License](https://img.shields.io/github/license/VPDPersonal/Aspid.Claude.Plugins?label=License&labelColor=254d2c&color=4fa35d)](LICENSE)

[English version](README.md)

Личный маркетплейс плагинов для [Claude Code](https://docs.claude.com/en/docs/claude-code) от Vladislav Panin.

Общий маркетплейс плагинов Claude Code для семейства пакетов **Aspid** и смежных воркфлоу — не привязан к какому-то одному пакету. Каждый пакет Aspid (или универсальная утилита) может поставлять сюда свой плагин. Плагины лежат в `plugins/<имя>/`, манифест маркетплейса — в `.claude-plugin/marketplace.json`.

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

В маркетплейсе живут плагины для любых пакетов Aspid (и смежного универсального тулинга для Claude Code). Список будет расти по мере появления плагинов для новых пакетов.

| Плагин | Целевой пакет | Что делает |
|---|---|---|
| [`aspid-fasttools`](plugins/aspid-fasttools/README_RU.md) | [Aspid.FastTools](https://github.com/VPDPersonal/Aspid.FastTools) | Скиллы Claude Code для пакета Aspid.FastTools: создание `IId`/`[UniqueId]`-структур (`aspid-id-struct`), вставка `this.Marker()` для профайлинга (`aspid-profiler-marker`) и сборка редактор-UI на fluent-расширениях `VisualElement` (`aspid-visual-element-fluent`). |

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
│   │   ├── README.md
│   │   └── README_RU.md
│   └── <other-plugin>/      # плагины для других пакетов Aspid или универсального тулинга
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
