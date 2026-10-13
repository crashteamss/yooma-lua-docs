# Свои чамсы

Переключаемые чамсы на противников: выбор материала, отдельный цвет для видимых и тех, что за стеной.

```lua
local on        = ui.create("checkbox", "ch_on", "Script chams", true)
local style     = ui.create("combo", "ch_style", "Material", 1, {"Flat", "Glow", "Bloom"})
local vis_col   = ui.create("color", "ch_vis", "Visible", {255, 90, 140, 255})
local invis_col = ui.create("color", "ch_inv", "Behind walls", {120, 90, 255, 170})

local materials = {
    chams.builtin_material("flat"),
    chams.builtin_material("glow"),
    chams.builtin_material("bloom"),
}

local applied = {}

local function apply()
    local key = table.concat({ tostring(ui.get(on)), ui.get(style),
        table.concat(ui.get(vis_col), ","), table.concat(ui.get(invis_col), ",") }, "|")
    if applied.key == key then return end
    applied.key = key

    if not ui.get(on) then
        chams.set("enemy", nil)
        chams.set("enemy_invisible", nil)
        return
    end

    local mat = materials[ui.get(style)]
    chams.set("enemy", mat, ui.get(vis_col))
    chams.set("enemy_invisible", mat, ui.get(invis_col))
end

-- применяем только при изменении настроек, не каждый кадр
events.on("frame_stage", apply)
```

{% hint style="info" %}
Пока скрипт держит цель (`enemy`, `enemy_invisible`), чамсы из меню для неё не работают. `chams.set(target, nil)` возвращает управление меню.
{% endhint %}
