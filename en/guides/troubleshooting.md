---
icon: bug
description: Nothing shows up? Start here.
---

# Troubleshooting

## Nothing shows up

1. **Is the island loaded?** `DynamicIsland` is `nil` when the user doesn't have it or turned it off. Check `type(DynamicIsland) == "table"`.
2. **Did you call it too early?** At load time the island may not exist yet. Use `OnScriptsLoaded` or [the queue](../reference/queue.md).
3. **Look at the return value.** `local id, err = DynamicIsland.Notify(...)` and print `err`. The reasons are in [Types](../reference/types.md#error-reasons).
4. **Was it allowed?** `DynamicIsland.IsAllowed("Your App")`. If the user said no, they can turn your script on in Alerts > Scripts.
5. **Is Focus on?** The moon next to the island means Focus mode. While it's on, notifications skip the banner and go straight to the notification center, unless they are `time-sensitive` and the user allows urgent alerts, or the user turned on Allow in Focus for your script.

## The console

The island writes to the Umbrella console with the prefix `[Dynamic Island]` when it rejects something important: a missing app name, rate limits, a mute, callback errors. Each message is written once, so the console doesn't fill up.

## The island's debug log

When you need to see exactly what the island did, turn on **General > Dynamic Island > Enable Island > More > Debug log**. A red dot appears next to the island, and everything goes to `scripts/dynamic_island_debug.log`: every notification your script sent, what the island decided, errors with a stack trace. Click the red dot to open the file.

## Common mistakes

| Symptom | Cause |
| --- | --- |
| Works once, then `"rate limited"` | Sending from `OnFrame` every frame. Send only when something changes. |
| The activity disappears after a while | `staleAfter` without `Update`, or the user swiped it away. Check `onEnd`. |
| The old activity vanished when you started a new one | One activity per script. Update the existing one instead. |
| The icon is a bell | Unknown glyph name, or the image path didn't load. |
| `IsActive()` is `false` right after `End({ after = 3 })` | That's expected: the activity is over, it's only showing its final content. |
