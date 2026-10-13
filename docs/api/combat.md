# combat

Данные оружия, бэктрек и расчёт урона — то же, что использует рейджбот.

## combat.context

```lua
local c = combat.context()
```

| Поле | Описание |
| --- | --- |
| `valid` | есть ли оружие и данные |
| `weapon_type` | тип оружия (числом) |
| `item_id` | номер предмета |
| `eye` | позиция глаз (откуда летит пуля) |
| `inaccuracy`, `spread` | текущий разброс |
| `range` | дальность оружия |
| `scoped` | в прицеле ли |
| `tick`, `time` | текущий тик и время клиента |

## combat.weapon_state

```lua
local w = combat.weapon_state()
```

| Поле | Описание |
| --- | --- |
| `item_id`, `ammo`, `reloading`, `is_revolver` | оружие |
| `tick_base`, `client_tick` | тики |
| `next_primary_tick`, `next_secondary_tick` | когда можно снова стрелять |
| `can_start_primary` | можно ли начать атаку |
| `ready_tick`, `postponed_primary_ready` | состояние взвода (револьвер) |

## combat.can_shoot

```lua
local ok = combat.can_shoot(ignore_next_attack)
```

Можно ли выстрелить прямо сейчас: есть патроны, нет перезарядки и (если `ignore_next_attack` не `true`) прошло время до следующего выстрела.

## combat.aim_punch

```lua
local punch = combat.aim_punch()   -- vector, градусы
```

Текущая отдача (насколько ствол ушёл от прицела).

## combat.records

```lua
local list = combat.records(id)
```

Записи лагкомпенсации (бэктрек) противника, **от новых к старым**. У каждой записи: `tick`, `time`, `origin`, `velocity`, `extrapolated`.

## combat.damage

```lua
local r = combat.damage(id, point)
```

Сколько урона нанесёт пуля из твоих глаз в точку `point` по игроку `id`, с учётом стен, пробития, брони и хитгруппы. Работает только в командных событиях, иначе возвращает `nil`.

| Поле | Описание |
| --- | --- |
| `damage` | урон |
| `hitbox`, `hitgroup` | куда попадёт пуля |
| `penetrated` | прошла ли пуля через стену |

```lua
events.on("create_move", function()
    for _, p in ipairs(entity.players(true)) do
        local head = p.alive and entity.bone(p.id, 6)
        local r = head and combat.damage(p.id, head)
        if r and r.damage >= p.health then
            print(p.name .. " can be killed: " .. r.damage)
        end
    end
end)
```

## Заглушки совместимости

`combat.hitboxes`, `combat.hitchance`, `combat.spread_correction`, `combat.spread_sample` возвращают `nil`.
