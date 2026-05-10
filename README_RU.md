# Aspid.FastTools.Claude — плагин для Claude Code

[![Releases](https://img.shields.io/github/v/release/VPDPersonal/Aspid.FastTools.Claude?label=Release&labelColor=254d2c&color=4fa35d&include_prereleases&sort=semver)](https://github.com/VPDPersonal/Aspid.FastTools.Claude/releases)
[![License](https://img.shields.io/github/license/VPDPersonal/Aspid.FastTools.Claude?label=License&labelColor=254d2c&color=4fa35d)](LICENSE)

[English version](README.md)

> **Альфа (`0.1.0`)** — триггеры, описания и структура скиллов могут меняться между
> минорными версиями по мере накопления фидбека от реального использования. Если нужна
> стабильность — фиксируйте конкретный коммит или тег.

Плагин для Claude Code, который помогает агенту корректно работать с Unity-пакетом
[**Aspid.FastTools**](https://github.com/VPDPersonal/Aspid.FastTools)
(`com.aspid.fasttools`) внутри ваших собственных Unity-проектов.

В составе плагина — **3 скилла**, которые срабатывают автоматически на типовые запросы:

| Скилл | Срабатывает на запросы вида |
|---|---|
| `aspid-id-struct` | «создай id struct», «добавь `[UniqueId]` к X», «сделай X идентификатором проекта» |
| `aspid-profiler-marker` | «профайлинг этого метода», «добавь `this.Marker()`», «инструментируй ProfilerMarker'ом» |
| `aspid-visual-element-fluent` | «собери редактор-UI на UIToolkit», «используй fluent `VisualElement`», «застилизуй этот `VisualElement`» |

## Требования

- Unity 2022.3+
- Установленный пакет Aspid.FastTools (`com.aspid.fasttools`) в проекте

## Установка

```
/plugin install github:VPDPersonal/Aspid.FastTools.Claude
```

## Что внутри

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

## Версионирование

Плагин отслеживает публичный API пакета `com.aspid.fasttools`, но версионируется
независимо. Версия `0.x` совместима со всеми версиями `com.aspid.fasttools`, которые
экспортируют те же публичные типы (`IId`, `[UniqueId]`, `IdRegistry`, `SerializableType`,
`EnumValues<TValue>`, `this.Marker()`, fluent-расширения `VisualElement`).

## Лицензия

MIT
