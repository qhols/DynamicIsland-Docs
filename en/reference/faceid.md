---
icon: face-viewfinder
description: Play the Face ID animation on the island.
---

# FaceID

## FaceID

`DynamicIsland.FaceID(options):` **`boolean`**

| Name | Type | Description |
| --- | --- | --- |
| **options** | **`table`** | Fields below. |

The island turns into a rounded square, draws a face and scans it, like Face ID on iPhone. Returns `true` when it started, `false` if another one is still playing or the island is busy.

### Options

| Name | Type | Description |
| --- | --- | --- |
| **result** | **`string`** | `"ok"`: the face turns green. `"fail"`: it turns red and shakes. Default `"ok"`. |
| **scan** | **`number`** | How long it looks before the result, from 0.4 to 3 seconds. |

<details>

<summary>Example</summary>

```lua
local di = DynamicIsland
if di.Has("faceId") then
    di.FaceID({ result = "ok" })
end
```

</details>

## Good to know

* The whole animation takes about 2.5 seconds.
* At most one every 3 seconds.
* Check `DynamicIsland.Has("faceId")` first if your script also runs on older versions.
