# entity

Информация об игроках. Все функции возвращают [таблицы игрока](../concepts/types.md#таблица-игрока) и работают в любом событии.

## entity.local_player

```lua
local me = entity.local_player()   -- таблица игрока или nil
```

Ты сам, с `id = 0`. Возвращает `nil`, если ты не в игре.

## entity.players

```lua
local list = entity.players(enemies_only)
```

Массив всех игроков. Если `enemies_only == true` — только противники. Без аргумента в список попадаешь и ты, со своим номером слота, а не с `0`. Отличить себя можно по `p.enemy == false` и совпадению ника, проще брать `entity.players(true)`. Мёртвые тоже входят в список, проверяй `p.alive`.

```lua
for _, p in ipairs(entity.players(true)) do
    if p.alive then
        print(p.name, p.health)
    end
end
```

## entity.get

```lua
local p = entity.get(id)   -- таблица игрока или nil
```

## entity.bone

```lua
local pos = entity.bone(id, bone_index)   -- vector или nil
```

Позиция кости в мире. `bone_index` от 0 до 127 (6 — голова у стандартных моделей).

## entity.bounds

```lua
local box = entity.bounds(id)   -- {x, y, w, h} или nil
```

2D-рамка игрока на экране (как у ESP-бокса). `nil`, если игрок не на экране.

```lua
events.on("paint", function()
    for _, p in ipairs(entity.players(true)) do
        local b = p.alive and entity.bounds(p.id)
        if b then
            render.rect(b.x, b.y, b.w, b.h, {255, 255, 255, 200})
        end
    end
end)
```

## entity.weapon

```lua
local w = entity.weapon(id)
```

Активное оружие игрока: `item_id` (номер предмета CS2), `ammo` (патроны в обойме), `reloading` (boolean).

## entity.items

Заглушка совместимости, возвращает пустую таблицу.
