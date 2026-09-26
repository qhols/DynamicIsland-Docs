---
icon: code
description: A full script that uses every feature, to read or to test with.
---

# Example Script

The SDK demo script acts as three apps (`SDK Demo`, `SDK Demo Runes`, `SDK Demo Courier`) so you can try two and three activities at once. Put it next to `dynamic_island.lua` in your scripts folder and open **General > Dynamic Island > SDK Demo**.

{% file src="../.gitbook/assets/di_sdk_demo.lua" %}

What's inside:

| Group | Buttons |
| --- | --- |
| Notifications | a plain one, one with actions, a toggle, time-sensitive, passive, one with an item picture, very long text, three apps at once |
| Gallery | cycle through tints, glyphs and sounds, play a sound, print API info to the console |
| Live Activities | a stack timer, a rune timer from a second app, a delivery from a third app, two and three at once, a final screen, a stale test, end all |
| Chaos | garbage arguments, crashing callbacks, a frozen callback, a broken queue, a replaced global, spam |

The Chaos group is there to show the island surviving a broken script. Everything it does is written to the console.

<details>

<summary>The most important part</summary>

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
