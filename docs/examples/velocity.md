# Индикатор скорости

Число скорости внизу экрана и график скорости за последние ~2 секунды.

```lua
local enabled = ui.create("checkbox", "vel_on", "Velocity indicator", true)
local color   = ui.create("color", "vel_col", "Color", {150, 190, 255, 255})

local history, max_points = {}, 120

events.on("paint", function()
    if not ui.get(enabled) then return end

    local me = entity.local_player()
    if not me or not me.alive then history = {} return end

    local v = me.velocity
    local speed = math.sqrt(v.x * v.x + v.y * v.y)

    table.insert(history, speed)
    if #history > max_points then table.remove(history, 1) end

    local w, h = client.screen_size()
    local c = ui.get(color)

    -- число
    local text = tostring(math.floor(speed + 0.5))
    local tw = render.measure_text(text)
    render.text(w / 2 - tw / 2, h - 140, text, c)

    -- график
    local gx, gy, gw, gh = w / 2 - 120, h - 110, 240, 40
    local points = {}
    for i, s in ipairs(history) do
        points[#points + 1] = { gx + (i - 1) / (max_points - 1) * gw, gy + gh - math.min(s / 400, 1) * gh }
    end
    if #points > 1 then
        render.poly_line(points, {c[1], c[2], c[3], 180}, 1.5)
    end
end)
```
