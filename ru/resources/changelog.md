---
icon: clock-rotate-left
description: Что менялось в SDK.
---

# Список изменений

## API 2

* `DynamicIsland.Widget.Register`: свой виджет в редакторе виджетов рядом со встроенными. У объекта есть `Set`, `Update`, `Remove` и `IsOnIsland`.
* `DynamicIsland.Has("widgets")`.
* `DynamicIsland.FaceID`: анимация Face ID на островке, зелёная при `ok`, красная с тряской при `fail`. `DynamicIsland.Has("faceId")`.

## API 1

Первая версия.

* `DynamicIsland.Notify` с уровнями, звуками, иконками, цветами, плашкой справа, длинным текстом и до 2 кнопок.
* `DynamicIsland.Activity.Start` с таймером, прогрессом, текстом справа, `onTap`, `onEnd` и `staleAfter`.
* `End` с финальным содержимым и `after`.
* `DynamicIslandQueue` для скриптов, которые грузятся раньше островка.
* `DynamicIsland.PlaySound`, `Has`, `IsAllowed` и `Glyphs`.
* Запрос разрешения для каждого скрипта, список скриптов и «Разрешить в фокусе» в настройках островка.
