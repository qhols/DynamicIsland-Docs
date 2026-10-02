---
icon: face-viewfinder
description: Показать на островке анимацию Face ID.
---

# FaceID

## FaceID

`DynamicIsland.FaceID(options):` **`boolean`**

| Name | Type | Description |
| --- | --- | --- |
| **options** | **`table`** | Поля ниже. |

Островок превращается в скруглённый квадрат, рисует лицо и сканирует его, как Face ID на iPhone. Возвращает `true`, если анимация началась, и `false`, если уже идёт другая или островок занят.

### Параметры

| Name | Type | Description |
| --- | --- | --- |
| **result** | **`string`** | `"ok"`: лицо становится зелёным. `"fail"`: краснеет и трясётся. По умолчанию `"ok"`. |
| **scan** | **`number`** | Сколько островок смотрит до результата, от 0.4 до 3 секунд. |

<details>

<summary>Пример</summary>

```lua
local di = DynamicIsland
if di.Has("faceId") then
    di.FaceID({ result = "ok" })
end
```

</details>

## Важно знать

* Вся анимация идёт около 2.5 секунды.
* Не чаще одного раза в 3 секунды.
* Если скрипт работает и на старых версиях, сначала проверьте `DynamicIsland.Has("faceId")`.
