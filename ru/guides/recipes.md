---
icon: book-open
description: Готовые куски кода для частых случаев.
---

# Рецепты

Во всех рецептах используется этот помощник:

```lua
local function Island()
    local di = DynamicIsland
    if type(di) == "table" and di.api then return di end
    return nil
end
```

## Отсчёт с полосой

Таймер, который заодно заполняет полосу, и финальный экран в конце.

```lua
local act, endsAt, length

local function Start(seconds)
    local di = Island()
    if not di then return end
    endsAt, length = os.clock() + seconds, seconds
    act = di.Activity.Start({
        app = "Auto Stacker", icon = "clock", tint = "orange",
        title = "Стак древних", timer = seconds, progress = 0
    })
end

local function OnFrame()
    if not (act and act:IsActive()) then return end
    local left = endsAt - os.clock()
    act:Update({ progress = 1 - math.max(0, left) / length })
    if left <= 0 then
        act:End({ title = "Застакано", trailing = "Готово", progress = 1, after = 3 })
    end
end

return { OnFrame = OnFrame }
```

## Отклик на переключатель

Быстрое подтверждение «Вкл» или «Выкл». Новое от того же скрипта заменяет предыдущее, поэтому даже быстрые нажатия выглядят мгновенно.

```lua
local function Toast(name, on)
    local di = Island()
    if not di then return end
    di.Notify({
        app = "Action Dial", title = name,
        icon = on and "check" or "close", tint = on and "green" or "red",
        trailing = on and "Вкл" or "Выкл", duration = 2.5
    })
end
```

## Прогресс с процентами

```lua
act:Update({ progress = p, trailing = string.format("%d%%", math.floor(p * 100)) })
```

## Спросить, прежде чем делать

```lua
Island().Notify({
    app = "Auto Stacker",
    title = "Стакать древних?",
    body = "Следующий пулл в 0:53.",
    actions = {
        { title = "Стакать", fn = StartStacking },
        { title = "Пропустить", destructive = true }
    }
})
```

Кнопка без `fn` просто закрывает уведомление.

## Прибраться, когда островок закончил активность

```lua
act = Island().Activity.Start({
    app = "Rune Timer", title = "Руна силы", timer = 120,
    onEnd = function(reason)
        act = nil
        if reason == "dismissed" then skipUntilNextRune = true end
    end
})
```

## Тихий лог на потом

Тихие уведомления не всплывают, они ждут в центре уведомлений.

```lua
Island().Notify({ app = "Farm Helper", title = "Нафармлено 1200 золота за 5 минут", icon = "gold", level = "passive" })
```
