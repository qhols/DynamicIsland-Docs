---
icon: sliders
description: Объект, который возвращает Widget.Register. Меняет текст, вид или убирает виджет.
---

# WidgetHandle

Возвращается из [`Widget.Register`](widget.md). Работает и `w:Set("x")`, и `w.Set("x")`.

## Set

`w:Set(text):` **`boolean`**

| Имя | Тип | Описание |
| --- | --- | --- |
| **text** | **`string`** | Новый текст, до 16 символов. Числа тоже можно. |

Меняет то, что показывает островок. Стоит дешево, вызывай каждый раз, когда значение меняется. После `Remove` возвращает `false`.

<details>

<summary>Пример</summary>

```lua
local function OnUpdate()
    local left = math.max(0, nextRune - GameRules.GetGameTime())
    rune:Set(string.format("%d:%02d", math.floor(left / 60), math.floor(left % 60)))
end
```

</details>

## Update

`w:Update(fields):` **`boolean`**

| Имя | Тип | Описание |
| --- | --- | --- |
| **fields** | **`table`** | Любые из `text`, `title`, `icon`, `tint`. |

Меняет несколько вещей сразу. Меняются только переданные поля.

<details>

<summary>Пример</summary>

```lua
rune:Update({ icon = "bolt", tint = "purple", text = "Now" })
```

</details>

## Remove

`w:Remove():` **`nil`**

Убирает виджет из редактора и с островка. Островок помнит его место, так что если зарегистрировать виджет снова, он встанет туда же.

## IsOnIsland

`w:IsOnIsland():` **`boolean`**

`true`, пока пользователь держит твой виджет на островке. Удобно, чтобы не считать то, что никто не видит.
