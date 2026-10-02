---
icon: cube
description: Глобальная таблица, через которую любой скрипт общается с островком.
---

# DynamicIsland

Глобальная таблица, только для чтения. Запись в неё вызывает ошибку, а если другой скрипт её подменит, островок вернёт её на следующем кадре.

## api

`DynamicIsland.api:` **`integer`**

Версия SDK. Сейчас `2`. Растёт только когда что-то добавляется, старый код продолжает работать.

## version

`DynamicIsland.version:` **`string`**

Версия скрипта Dynamic Island, например `"2.3.0"`.

## Notify

`DynamicIsland.Notify(options):` **`integer`** | **`nil`**, **`string`**

Показывает уведомление. Подробно в [Notify](notify.md).

## Activity.Start

`DynamicIsland.Activity.Start(options):` [**`ActivityHandle`**](activity-handle.md) | **`nil`**, **`string`**

Запускает живую активность. Подробно в [Activity](activity.md).

## Widget.Register

`DynamicIsland.Widget.Register(options):` [**`WidgetHandle`**](widget-handle.md) | **`nil`**, **`string`**

Добавляет виджет, который пользователь может поставить в островок. Подробно в [Widget](widget.md).

## PlaySound

`DynamicIsland.PlaySound(name, [volume]):` **`boolean`**

| Имя | Тип | Описание |
| --- | --- | --- |
| **name** | [**`Sound`**](types.md) | Один из звуков островка. |
| **volume `[?]`** | **`number`** | От 0.05 до 1. `(по умолчанию: 0.5)` |

Проигрывает системный звук островка. Учитывает настройки звука пользователя: если он выключил звуки, ничего не играет. Возвращает `false` для неизвестного имени или если все скрипты вместе просят больше 25 звуков в секунду.

<details>

<summary>Пример</summary>

```lua
local di = DynamicIsland
if type(di) == "table" and di.api then
    di.PlaySound("wheel_notch", 0.8)
end
```

</details>

## IsAllowed

`DynamicIsland.IsAllowed(app):` **`boolean`**

| Имя | Тип | Описание |
| --- | --- | --- |
| **app** | **`string`** | Название твоего скрипта. |

Возвращает `true`, когда пользователь разрешил этот скрипт. `false`, пока он не ответил или если отказал.

## Has

`DynamicIsland.Has(feature):` **`boolean`**

| Имя | Тип | Описание |
| --- | --- | --- |
| **feature** | **`string`** | Название возможности. |

Говорит, поддерживает ли эта версия островка нужную возможность. Пригодится, когда в новом SDK что-то добавится, а скрипт должен работать и на старых версиях.

Возможности в API 1: `notify`, `activity`, `queue`, `levels`, `sounds`, `body`, `actions`, `trailing`, `onEnd`, `staleAfter`, `endAfter`, `playSound`, `focus`. Добавлено в API 2: `widgets`, `faceId`.

<details>

<summary>Пример</summary>

```lua
local di = DynamicIsland
local opts = { app = "Auto Stacker", title = "Древние готовы" }
if di.Has("actions") then
    opts.actions = { { title = "Напомнить", fn = RemindLater } }
end
di.Notify(opts)
```

</details>

## Glyphs

`DynamicIsland.Glyphs():` **`string[]`**

Возвращает названия всех иконок островка по алфавиту. Те, что лучше всего смотрятся в уведомлениях, показаны на странице [Типы](types.md).
