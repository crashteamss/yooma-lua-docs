# movement

Данные движения и отключение встроенных бхопа/стрейфера.

## movement.context

```lua
local m = movement.context()
```

| Поле | Описание |
| --- | --- |
| `on_ground` | стоишь ли на земле |
| `flags` | флаги игрока |
| `move_type` | тип движения (9 — лестница, 8 — noclip) |
| `origin` | позиция |
| `velocity` | скорость в начале тика |
| `predicted_velocity` | скорость после предсказания этого тика |
| `stamina` | выносливость (0–100) |
| `surface_friction` | трение поверхности |
| `forward`, `side` | движение текущей команды |
| `buttons` | кнопки текущей команды |

## movement.claim

```lua
movement.claim("bhop")    -- или "strafe"
```

Отключает встроенный бхоп (`"bhop"`) или автострейф (`"strafe"`) **на этот тик**, чтобы скрипт управлял ими сам. Вызывай каждый тик в `create_move`, пока хочешь держать управление.

```lua
events.on("create_move", function()
    if not ui.get(my_bhop) then return end
    movement.claim("bhop")

    local m = movement.context()
    local c = cmd.get()
    if c.buttons & buttons.jump ~= 0 and not m.on_ground then
        cmd.set_button(buttons.jump, false)
    end
end)
```
