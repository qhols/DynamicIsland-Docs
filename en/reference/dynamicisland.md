---
icon: cube
description: The global table every script uses to talk to the island.
---

# DynamicIsland

Global table, read only. Writing into it throws an error, and if another script replaces it, the island puts it back on the next frame.

## api

`DynamicIsland.api:` **`integer`**

Version of the SDK. Currently `2`. It only goes up when something is added, old code keeps working.

## version

`DynamicIsland.version:` **`string`**

Version of the Dynamic Island script, for example `"2.3.0"`.

## Notify

`DynamicIsland.Notify(options):` **`integer`** | **`nil`**, **`string`**

Shows a notification. See [Notify](notify.md).

## Activity.Start

`DynamicIsland.Activity.Start(options):` [**`ActivityHandle`**](activity-handle.md) | **`nil`**, **`string`**

Starts a live activity. See [Activity](activity.md).

## Widget.Register

`DynamicIsland.Widget.Register(options):` [**`WidgetHandle`**](widget-handle.md) | **`nil`**, **`string`**

Adds a widget the user can put into the island. See [Widget](widget.md).

## PlaySound

`DynamicIsland.PlaySound(name, [volume]):` **`boolean`**

| Name | Type | Description |
| --- | --- | --- |
| **name** | [**`Sound`**](types.md#sounds) | One of the island's sounds. |
| **volume `[?]`** | **`number`** | 0.05 to 1. `(default: 0.5)` |

Plays one of the island's own sounds. It respects the user's sound settings, so if they turned sounds off, nothing plays. Returns `false` for an unknown name or when more than 25 sounds per second are requested across all scripts.

<details>

<summary>Example</summary>

```lua
local di = DynamicIsland
if type(di) == "table" and di.api then
    di.PlaySound("wheel_notch", 0.8)
end
```

</details>

## IsAllowed

`DynamicIsland.IsAllowed(app):` **`boolean`**

| Name | Type | Description |
| --- | --- | --- |
| **app** | **`string`** | Your script's name. |

Returns `true` once the user has allowed this script. `false` while they haven't answered or said no.

## Has

`DynamicIsland.Has(feature):` **`boolean`**

| Name | Type | Description |
| --- | --- | --- |
| **feature** | **`string`** | Feature name. |

Tells you if this version of the island supports a feature. Useful when a newer SDK adds something and your script should still run on older ones.

Features in API 1: `notify`, `activity`, `queue`, `levels`, `sounds`, `body`, `actions`, `trailing`, `onEnd`, `staleAfter`, `endAfter`, `playSound`, `focus`. Added in API 2: `widgets`, `faceId`.

<details>

<summary>Example</summary>

```lua
local di = DynamicIsland
local opts = { app = "Auto Stacker", title = "Ancients are ready" }
if di.Has("actions") then
    opts.actions = { { title = "Remind me", fn = RemindLater } }
end
di.Notify(opts)
```

</details>

## Glyphs

`DynamicIsland.Glyphs():` **`string[]`**

Returns the names of every icon the island has, sorted. The ones that look best in notifications are shown on the [Types](types.md#icons) page.
