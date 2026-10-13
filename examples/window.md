# Окно с настройками

Своё окно, которое открывается и закрывается клавишей `Home`, с перенесёнными в него элементами.

```lua
local win = ui.create_window("my_panel", "My panel", {
    width  = 260,
    height = 180,
    key    = 0x24,   -- Home
    open   = false,
})

local enabled = ui.create("checkbox", "panel_on", "Enabled", true)
local radius  = ui.create("slider", "panel_r", "Radius", 60, 10, 200)
local color   = ui.create("color", "panel_col", "Color", {255, 255, 255, 120})

for _, id in ipairs({ enabled, radius, color }) do
    ui.attach_window(id, win)
end

events.on("paint", function()
    if not ui.get(enabled) then return end
    local w, h = client.screen_size()
    render.circle(w / 2, h / 2, ui.get(radius), ui.get(color))
end)
```

* `key = 0` (или без `key`) — окно показывается вместе с меню.
* Позицию и размер окна можно узнать через `ui.get_window(win)`.
