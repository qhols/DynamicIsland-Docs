---
icon: gauge
description: How much a script can send, and how the island protects itself.
---

# Limits and Safety

## Limits

| What | Limit |
| --- | --- |
| Notifications | 3 in a row per script, then one every 2 seconds. |
| Spam | A script that keeps getting refused is muted until the next reload. It's written to the console. |
| Activities | 1 per script, 3 in total. |
| Script names | 24 different `app` names per session. |
| Waiting | 6 permission prompts and 6 script notifications in the queue at once. |
| Sounds | 25 per second across all scripts. |
| Images | 32 different paths per session across all scripts. |
| Callbacks | After 3 errors the island stops calling that script's callbacks. |

Each call that goes over a limit returns `nil` and a [reason](../reference/types.md#error-reasons), so you can see what happened.

## Callbacks

Your `onTap`, `onEnd` and action functions run in `pcall`. An error there never reaches the island, it's written to the console with your script's name.

{% hint style="warning" %}
Umbrella has no debug hooks, so the island can't stop a callback that never returns. A `while true do end` inside `onTap` freezes the game like it would anywhere else. Keep callbacks short.
{% endhint %}

## What the island protects itself from

All Umbrella scripts share one Lua state, so the island assumes another script can be broken:

* Bad arguments are rejected, including tables with trapped metamethods, NaN and infinite numbers, and huge strings.
* If `DynamicIsland` or `DynamicIslandQueue` gets overwritten, the island puts it back and writes about it in the console.
* The island keeps its own copies of `string`, `table`, `math`, `os` and `io`, so a script that replaces `math.floor` doesn't take it down.
* Long text is trimmed by characters, so Cyrillic and emoji are never cut in half.

## Being a good citizen

* Pick one `app` name and keep it.
* Use `passive` for things the user doesn't need to see right now.
* End your activities when they're no longer relevant, or set `staleAfter`.
* Don't send a notification every frame, send it when something changes.
