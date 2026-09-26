---
icon: inbox
description: Send notifications before the island has loaded.
---

# DynamicIslandQueue

`DynamicIslandQueue:` **`table[]`**

A global list anyone can add to. Every frame the island takes the notifications out of it and shows them. Use it when your script runs before the island is loaded, for example right at the top of your file.

```lua
DynamicIslandQueue = DynamicIslandQueue or {}
table.insert(DynamicIslandQueue, {
    app = "My Script",
    title = "Loaded and ready",
    icon = "check",
    level = "passive"
})
```

Each item is the same table you would pass to [`Notify`](notify.md).

## Good to know

* Queued notifications follow the same [limits](../guides/limits-and-safety.md) as `Notify`.
* The island takes up to 20 items per frame.
* Always write `DynamicIslandQueue = DynamicIslandQueue or {}` first, another script may have created the list already.
* If a script puts something that isn't a list there, the island resets it and writes about it in the console.

{% hint style="info" %}
Once the island is loaded, calling `DynamicIsland.Notify` directly is simpler and gives you the result right away.
{% endhint %}
