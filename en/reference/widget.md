---
icon: grip
description: Put your own value into the island next to the clock, like WidgetKit.
---

# Widget

## Widget.Register

`DynamicIsland.Widget.Register(options):` [**`WidgetHandle`**](widget-handle.md) | **`nil`**, **`string`**

| Name | Type | Description |
| --- | --- | --- |
| **options** | **`table`** | Fields below. |

Adds your widget to the widget editor. It shows up in the island only after the user adds it there, the same way as the built-in clock or FPS. Returns a handle to change or remove it, or `nil` and a [reason](types.md#error-reasons).

### Options

`app` and `id` are required.

| Name | Type | Description |
| --- | --- | --- |
| **app** | **`string`** | Your script's name, same as in [Notify](notify.md). |
| **id** | **`string`** | Stays the same between reloads. The island remembers where the user put your widget by it. Letters and digits count. |
| **title `[?]`** | **`string`** | Name in the widget editor, up to 24 characters. `(default: app)` |
| **text `[?]`** | **`string`** | What the island shows, up to 16 characters. |
| **icon `[?]`** | [**`Icon`**](types.md#icons) | Glyph next to the text and in the editor. Only glyphs, image paths show a bell. `(default: "bell")` |
| **tint `[?]`** | [**`Tint`**](types.md#tints) | Color of the widget in the editor and its default color when the user picks a custom color. |

<details>

<summary>Example</summary>

```lua
local rune = DynamicIsland.Widget.Register({
    app = "Rune Timer",
    id = "next_rune",
    title = "Next Rune",
    icon = "clock",
    tint = "orange",
    text = "1:42"
})
```

</details>

## How it works

* The island never calls your code while drawing. It shows the last text you passed to [`Set`](widget-handle.md#set), so a slow script can't slow the island down. Update the text from your own `OnUpdate` or `OnFrame` when the value changes.
* Widgets don't ask for permission. Nothing appears until the user adds your widget in the editor.
* The user can move your widget, turn its icon off, make it bold and recolor it, like any other widget.
* If the user turns your script off in Alerts > Scripts, its widgets leave the island and come back on their old place when it's turned on again.
* If your script isn't loaded yet, the island keeps your widget's place and shows the rest as usual.
* Registering the same `app` and `id` again replaces the old widget and keeps its place.

## Rules

* 4 widgets per script, 12 across all scripts.
* Use `DynamicIsland.Has("widgets")` if your script should also run on older versions of the island.
