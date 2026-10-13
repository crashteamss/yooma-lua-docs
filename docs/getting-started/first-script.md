# Первый скрипт

Скрипт выполняется один раз при включении: в этот момент создаются элементы меню и подписки на события. Вся «живая» работа идёт внутри обработчиков событий.

```lua
-- 1. элементы меню (появятся во вкладке lua под именем скрипта)
local enabled = ui.create("checkbox", "enabled", "Enabled", true)
local size    = ui.create("slider", "size", "Crosshair size", 6, 2, 20)
local color   = ui.create("color", "color", "Color", {120, 180, 255, 255})

-- 2. рисование каждый кадр
events.on("paint", function()
    if not ui.get(enabled) then return end

    local w, h = client.screen_size()
    local cx, cy = w / 2, h / 2
    local s = ui.get(size)
    local c = ui.get(color)

    render.line(cx - s, cy, cx + s, cy, c, 1.5)
    render.line(cx, cy - s, cx, cy + s, c, 1.5)
end)

-- 3. логика каждого тика (game thread)
events.on("create_move", function()
    local me = entity.local_player()
    if me and me.alive and client.key_down(0x56) then -- клавиша V
        cmd.set_button(buttons.duck, true)
    end
end)

-- 4. уборка при выключении скрипта
events.on("shutdown", function()
    print("bye")
end)
```

## Что здесь важно

* **`ui.create`** возвращает **id** элемента (строку). Значение читается через `ui.get(id)`.
* **`paint`** вызывается каждый кадр, только в нём работают функции `render.*`.
* **`create_move`** вызывается каждый тик игры, только в командных событиях работает `cmd.*`.
* **`print`** пишет в лог вкладки lua (то же самое, что `client.log`).

Подробнее о том, какое событие когда приходит: [События и потоки](../concepts/events.md).
