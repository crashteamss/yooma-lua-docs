# math

Вся стандартная библиотека `math` Lua 5.4 плюс функции для углов и векторов. Углы — в градусах, векторы — таблицы `{x, y, z}`.

## math.calc_angle

```lua
local ang = math.calc_angle(from, to)
```

Угол, под которым нужно смотреть из точки `from`, чтобы попасть в `to`.

## math.normalize_angle

```lua
local a = math.normalize_angle(deg)   -- в диапазон [-180, 180]
```

## math.angle_vectors

```lua
local forward, right, up = math.angle_vectors(angles)
```

Направляющие векторы для углов.

## math.vector_angles

```lua
local ang = math.vector_angles(direction)
```

Обратная операция: углы по вектору направления.

## math.calc_fov

```lua
local fov = math.calc_fov(view_angles, from, to)
```

Сколько градусов между текущим взглядом `view_angles` и направлением из `from` в `to`. Так обычно выбирают «ближайшую к прицелу» цель.

```lua
events.on("create_move", function()
    local eye = combat.context().eye
    local view = cmd.get().angles
    local best, best_fov = nil, 180
    for _, p in ipairs(entity.players(true)) do
        local head = p.alive and entity.bone(p.id, 6)
        if head then
            local fov = math.calc_fov(view, eye, head)
            if fov < best_fov then best, best_fov = p, fov end
        end
    end
end)
```
