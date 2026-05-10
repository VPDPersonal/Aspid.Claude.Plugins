# Aspid.FastTools.Claude — плагин для Claude Code

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

Плагин **не включает** скиллы для внутренних конвенций пакета (USS BEM грамматика,
кастомная палитра `AspidStyles`) — они нужны контрибьюторам самого пакета, а не его
потребителям.

## Требования

- Unity 2022.3+
- Установленный пакет Aspid.FastTools (`com.aspid.fasttools`) в проекте

## Установка

### Вариант A — через marketplace плагинов Claude Code

```
/plugin install aspid-fasttools
```

### Вариант B — напрямую из Git

```
/plugin install github:VPDPersonal/Aspid.FastTools.Claude
```

### Вариант C — git submodule (для контрибьюторов)

```bash
git submodule add https://github.com/VPDPersonal/Aspid.FastTools.Claude.git \
    .claude/plugins/aspid-fasttools
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
