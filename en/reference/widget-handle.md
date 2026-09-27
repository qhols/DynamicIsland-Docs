---
icon: sliders
description: The object Widget.Register returns. Change the text, the look, or remove the widget.
---

# WidgetHandle

Returned by [`Widget.Register`](widget.md). Both `w:Set("x")` and `w.Set("x")` work.

## Set

`w:Set(text):` **`boolean`**

| Name | Type | Description |
| --- | --- | --- |
| **text** | **`string`** | New text, up to 16 characters. Numbers work too. |

Changes what the island shows. Cheap, call it whenever the value changes. Returns `false` after `Remove`.

<details>

<summary>Example</summary>

```lua
local function OnUpdate()
    local left = math.max(0, nextRune - GameRules.GetGameTime())
    rune:Set(string.format("%d:%02d", math.floor(left / 60), math.floor(left % 60)))
end
```

</details>

## Update

`w:Update(fields):` **`boolean`**

| Name | Type | Description |
| --- | --- | --- |
| **fields** | **`table`** | Any of `text`, `title`, `icon`, `tint`. |

Changes several things at once. Only the fields you pass change.

<details>

<summary>Example</summary>

```lua
rune:Update({ icon = "bolt", tint = "purple", text = "Now" })
```

</details>

## Remove

`w:Remove():` **`nil`**

Takes the widget out of the editor and the island. The island still remembers its place, so registering it again later puts it back where it was.

## IsOnIsland

`w:IsOnIsland():` **`boolean`**

`true` while the user has your widget on the island. Handy to skip work nobody sees.
