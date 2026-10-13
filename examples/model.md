# Своя модель игрока

Комбо со всеми установленными моделями. Модели сначала нужно положить в папку игры, см. [models](../api/models.md#куда-класть-модели).

```lua
local list = models.list()

local names = { "default" }
for _, m in ipairs(list) do
    -- короткое имя: последняя папка + файл
    names[#names + 1] = m.id:match("([^/]+/[^/]+)%.vmdl_c$") or m.id
end

local pick = ui.create("combo", "model_pick", "Player model", 1, names)
local current = 1

events.on("frame_stage", function()
    local i = ui.get(pick)
    if i == current then return end
    current = i

    if i == 1 then
        models.clear()
        print("model: default")
    else
        local ok, err = pcall(models.set, list[i - 1].id)
        print(ok and ("model: " .. names[i]) or ("model error: " .. tostring(err)))
    end
end)
```

Модель сама сбросится на обычную, когда скрипт выключат.
