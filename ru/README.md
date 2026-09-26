---
icon: sparkles
description: Уведомления и живые активности в Dynamic Island из твоего скрипта для Umbrella.
---

# Обзор

Dynamic Island это скрипт для Umbrella, который превращает верх экрана Dota 2 в островок как на iPhone: музыка, оповещения, таймеры, драки. SDK даёт пользоваться им **любому другому скрипту**. Ты передаёшь заголовок, иконку и цвет, а островок рисует всё остальное, поэтому любой скрипт выглядит частью одной системы.

<figure><picture><source srcset=".gitbook/assets/notify-dark.png" media="(prefers-color-scheme: dark)"><img src=".gitbook/assets/notify-light.png" alt="Уведомление от скрипта" width="296"></picture></figure>

## Что можно делать

* **Уведомления.** Баннер с названием твоего скрипта, заголовком, длинным текстом и до двух кнопок. Тихие сразу уходят в центр уведомлений.
* **Живые активности.** То, что длится: таймер, полоса прогресса, счётчик. Они висят в островке и раскрываются при наведении.
* **Звуки.** Системные звуки островка с учётом настроек звука у пользователя.

<figure><picture><source srcset=".gitbook/assets/activity-expanded-dark.png" media="(prefers-color-scheme: dark)"><img src=".gitbook/assets/activity-expanded-light.png" alt="Раскрытая живая активность" width="376"></picture></figure>

## Пример за 30 секунд

```lua
local di = DynamicIsland
if type(di) == "table" and di.api then
    di.Notify({ app = "My Script", title = "Привет из моего скрипта", icon = "bell", tint = "blue" })
end
```

Это вся интеграция. Когда твой скрипт в первый раз что-то пришлёт, островок спросит пользователя, можно ли, так же как iPhone спрашивает про приложение.

## Куда дальше

{% content-ref url="getting-started/quick-start.md" %}
[quick-start.md](getting-started/quick-start.md)
{% endcontent-ref %}

{% content-ref url="reference/notify.md" %}
[notify.md](reference/notify.md)
{% endcontent-ref %}

{% content-ref url="reference/activity.md" %}
[activity.md](reference/activity.md)
{% endcontent-ref %}

{% hint style="info" %}
SDK работает, только когда у пользователя установлен и включён Dynamic Island. Твой скрипт должен работать и без него, смотри [Быстрый старт](getting-started/quick-start.md).
{% endhint %}
