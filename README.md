# yooma Lua API

**yooma** умеет запускать пользовательские скрипты на **Lua 5.4**. Скрипт может рисовать поверх игры, добавлять свои элементы в меню, менять движение и команды, управлять чамсами и моделями игроков, читать данные рейджбота и реагировать на игровые события.

API совместимо с большинством скриптов из «skeet»-подобных паст: те же имена модулей, те же события, те же форматы цветов и векторов.

## Что умеет скрипт

| Задача | Модули |
| --- | --- |
| Рисовать HUD, индикаторы, ESP | [`render`](api/render.md), [`entity`](api/entity.md) |
| Свои галочки, слайдеры, окна в меню | [`ui`](api/ui.md) |
| Менять кнопки и движение каждого тика | [`cmd`](api/cmd.md), [`movement`](api/movement.md) |
| Данные оружия, бэктрек, автоволл | [`combat`](api/combat.md), [`trace`](api/trace.md) |
| Читать и переопределять настройки чита | [`settings`](api/settings.md) |
| Чамсы и модели игроков | [`chams`](api/chams.md), [`models`](api/models.md) |
| Хранить данные между запусками | [`storage`](api/storage.md) |
| Звуки, консоль, чат | [`audio`](api/audio.md), [`console`](api/console.md) |

## Быстрый пример

```lua
local enabled = ui.create("checkbox", "show_speed", "Show speed", true)

events.on("paint", function()
    if not ui.get(enabled) then return end

    local me = entity.local_player()
    if not me or not me.alive then return end

    local v = me.velocity
    local speed = math.floor(math.sqrt(v.x * v.x + v.y * v.y))
    local w, h = client.screen_size()

    render.text(w / 2 - 20, h - 120, tostring(speed), {255, 255, 255, 255})
end)
```

Дальше: [Установка скриптов](getting-started/installation.md) → [Первый скрипт](getting-started/first-script.md).
