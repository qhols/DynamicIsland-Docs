---
icon: book-open
description: Ready to copy patterns for common cases.
---

# Recipes

All recipes use this helper:

```lua
local function Island()
    local di = DynamicIsland
    if type(di) == "table" and di.api then return di end
    return nil
end
```

## Countdown with a bar

A timer that also fills a bar, and a final screen when it's done.

```lua
local act, endsAt, length

local function Start(seconds)
    local di = Island()
    if not di then return end
    endsAt, length = os.clock() + seconds, seconds
    act = di.Activity.Start({
        app = "Auto Stacker", icon = "clock", tint = "orange",
        title = "Ancients stack", timer = seconds, progress = 0
    })
end

local function OnFrame()
    if not (act and act:IsActive()) then return end
    local left = endsAt - os.clock()
    act:Update({ progress = 1 - math.max(0, left) / length })
    if left <= 0 then
        act:End({ title = "Stacked", trailing = "Done", progress = 1, after = 3 })
    end
end

return { OnFrame = OnFrame }
```

## Toggle feedback

A quick On / Off confirmation. A new one from the same script replaces the previous one, so pressing fast feels instant.

```lua
local function Toast(name, on)
    local di = Island()
    if not di then return end
    di.Notify({
        app = "Action Dial", title = name,
        icon = on and "check" or "close", tint = on and "green" or "red",
        trailing = on and "On" or "Off", duration = 2.5
    })
end
```

## Progress with a percentage

```lua
act:Update({ progress = p, trailing = string.format("%d%%", math.floor(p * 100)) })
```

## Ask before doing something

```lua
Island().Notify({
    app = "Auto Stacker",
    title = "Stack the ancients?",
    body = "The next pull is at 0:53.",
    actions = {
        { title = "Stack", fn = StartStacking },
        { title = "Skip", destructive = true }
    }
})
```

An action without `fn` just closes the notification.

## Clean up when the island ends your activity

```lua
act = Island().Activity.Start({
    app = "Rune Timer", title = "Power rune", timer = 120,
    onEnd = function(reason)
        act = nil
        if reason == "dismissed" then skipUntilNextRune = true end
    end
})
```

## A quiet log for later

Passive notifications don't pop up, they wait in the notification center.

```lua
Island().Notify({ app = "Farm Helper", title = "Farmed 1200 gold in 5 minutes", icon = "gold", level = "passive" })
```
