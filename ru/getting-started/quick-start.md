---
icon: rocket
description: Найти островок, отправить первое уведомление и запустить первую активность.
---

# Быстрый старт

## Как найти островок

Все скрипты Umbrella работают в одном общем Lua, поэтому островок это глобальная таблица `DynamicIsland`. Она есть, только если у пользователя включён скрипт островка, поэтому всегда проверяй:

```lua
local function Island()
    local di = DynamicIsland
    if type(di) == "table" and di.api then return di end
    return nil
end
```

Вызывай `Island()` каждый раз, когда нужно, а не сохраняй результат один раз. Umbrella перезагружает все скрипты разом, например после матча, и сохранённая ссылка будет указывать на старую копию.

## Когда вызывать

Umbrella грузит скрипты по алфавиту, поэтому твой может загрузиться раньше островка. Есть два надёжных варианта:

* вызывать островок из `OnScriptsLoaded` или любого коллбэка позже, к этому моменту загружены все скрипты
* при загрузке положить уведомление в [очередь](../reference/queue.md), островок заберёт его, как только стартует

```lua
DynamicIslandQueue = DynamicIslandQueue or {}
table.insert(DynamicIslandQueue, { app = "My Script", title = "Загружен", level = "passive" })
```

## Первое уведомление

```lua
local function OnScriptsLoaded()
    local di = Island()
    if not di then return end
    di.Notify({
        app = "Auto Stacker",
        title = "Стак древних через 0:10",
        icon = "stack",
        tint = "green"
    })
end

return { OnScriptsLoaded = OnScriptsLoaded }
```

<figure><picture><source srcset="../.gitbook/assets/notify-dark.png" media="(prefers-color-scheme: dark)"><img src="../.gitbook/assets/notify-light.png" alt="Уведомление от скрипта" width="296"></picture></figure>

`app` это название твоего скрипта. Оно показывается над заголовком, по нему группируются уведомления в центре уведомлений, и именно его пользователь разрешает или запрещает. Используй одно и то же название во всём скрипте.

## Первая живая активность

```lua
local timer

local function StartTimer()
    local di = Island()
    if not di then return end
    timer = di.Activity.Start({
        app = "Auto Stacker",
        icon = "clock",
        tint = "orange",
        title = "Стак древних",
        subtitle = "Тяни в 0:53 с левой стороны",
        timer = 42
    })
end
```

<figure><picture><source srcset="../.gitbook/assets/activity-compact-dark.png" media="(prefers-color-scheme: dark)"><img src="../.gitbook/assets/activity-compact-light.png" alt="Живая активность с таймером и полосой" width="266"></picture></figure>

Таймер островок отсчитывает сам. Когда всё, вызови `timer:End()`.

## Проверка результата

Если вызов не прошёл, он возвращает `nil` и причину:

```lua
local id, err = di.Notify({ app = "Auto Stacker", title = "Тест" })
if not id then Log.Write("островок отказал: " .. tostring(err)) end
```

Список причин есть в [Типах](../reference/types.md).

{% hint style="success" %}
Готовый скрипт, который использует всё сразу, лежит на странице [Пример скрипта](../resources/example-script.md). Положи его рядом с `dynamic_island.lua` и открой General > Dynamic Island > SDK Demo.
{% endhint %}
