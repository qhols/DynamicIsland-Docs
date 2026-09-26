---
icon: code
description: Полный скрипт, который использует всё сразу. Почитать или потыкать.
---

# Пример скрипта

Демо-скрипт SDK притворяется тремя приложениями (`SDK Demo`, `SDK Demo Runes`, `SDK Demo Courier`), чтобы можно было попробовать две и три активности сразу. Положи его рядом с `dynamic_island.lua` в папку скриптов и открой **General > Dynamic Island > SDK Demo**.

{% file src="../.gitbook/assets/di_sdk_demo.lua" %}

Что внутри:

| Группа | Кнопки |
| --- | --- |
| Notifications | обычное, с кнопками, переключатель, time-sensitive, тихое, с картинкой предмета, очень длинный текст, три приложения сразу |
| Gallery | перебор цветов, глифов и звуков, проиграть звук, вывести информацию об API в консоль |
| Live Activities | таймер стака, таймер руны от второго приложения, доставка от третьего, две и три сразу, финальный экран, тест устаревания, закончить все |
| Chaos | мусорные аргументы, падающие коллбэки, зависший коллбэк, сломанная очередь, подменённая глобалка, спам |

Группа Chaos нужна, чтобы показать, как островок переживает сломанный скрипт. Всё, что она делает, пишется в консоль.

<details>

<summary>Самое главное</summary>

```lua
local APP = "SDK Demo"

DynamicIslandQueue = DynamicIslandQueue or {}
table.insert(DynamicIslandQueue, { app = APP, title = "SDK demo is loaded", icon = "check", level = "passive" })

local function Island()
    local di = DynamicIsland
    if type(di) == "table" and di.api then return di end
    return nil
end

local function Notify(opts)
    local di = Island()
    if not di then return end
    opts.app = APP
    local id, err = di.Notify(opts)
    if not id then Log.Write("[SDK Demo] notify failed: " .. tostring(err)) end
end
```

</details>
