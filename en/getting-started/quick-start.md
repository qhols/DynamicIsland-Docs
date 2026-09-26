---
icon: rocket
description: Find the island, send your first notification and start your first activity.
---

# Quick Start

## Finding the island

All Umbrella scripts run in one shared Lua state, so the island is a global called `DynamicIsland`. It only exists when the user has the island script turned on, so always check for it:

```lua
local function Island()
    local di = DynamicIsland
    if type(di) == "table" and di.api then return di end
    return nil
end
```

Call `Island()` every time you need it instead of saving the result once. Umbrella reloads every script together, for example after a match, and a saved reference would point to the old copy.

## When to call it

Umbrella loads scripts in alphabetical order by file name, so your script may load before the island. There are two safe ways:

* call the island from `OnScriptsLoaded` or any later callback, by then every script is loaded
* at load time, put your notification in [the queue](../reference/queue.md), the island picks it up as soon as it starts

```lua
DynamicIslandQueue = DynamicIslandQueue or {}
table.insert(DynamicIslandQueue, { app = "My Script", title = "Loaded", level = "passive" })
```

## Your first notification

```lua
local function OnScriptsLoaded()
    local di = Island()
    if not di then return end
    di.Notify({
        app = "Auto Stacker",
        title = "Stack the ancients in 0:10",
        icon = "stack",
        tint = "green"
    })
end

return { OnScriptsLoaded = OnScriptsLoaded }
```

<figure><picture><source srcset="../.gitbook/assets/notify-dark.png" media="(prefers-color-scheme: dark)"><img src="../.gitbook/assets/notify-light.png" alt="A notification from a script" width="296"></picture></figure>

`app` is your script's name. It's shown above the title, it groups your notifications in the notification center, and it's what the user allows or denies. Use the same name everywhere in your script.

## Your first live activity

```lua
local timer

local function StartTimer()
    local di = Island()
    if not di then return end
    timer = di.Activity.Start({
        app = "Auto Stacker",
        icon = "clock",
        tint = "orange",
        title = "Ancients stack",
        subtitle = "Pull at 0:53 from the left side",
        timer = 42
    })
end
```

<figure><picture><source srcset="../.gitbook/assets/activity-compact-dark.png" media="(prefers-color-scheme: dark)"><img src="../.gitbook/assets/activity-compact-light.png" alt="A live activity with a timer and a progress bar" width="266"></picture></figure>

The island counts the timer down on its own. When you are done, call `timer:End()`.

## Checking the result

Every call returns `nil` and a reason when it does not go through:

```lua
local id, err = di.Notify({ app = "Auto Stacker", title = "Test" })
if not id then Log.Write("island said no: " .. tostring(err)) end
```

The reasons are listed in [Types](../reference/types.md#error-reasons).

{% hint style="success" %}
A full working script that uses every feature is on the [Example Script](../resources/example-script.md) page. Drop it next to `dynamic_island.lua` and open General > Dynamic Island > SDK Demo.
{% endhint %}
