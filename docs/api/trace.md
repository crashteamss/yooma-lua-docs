# trace

## trace.line

```lua
local tr = trace.line(start, finish, skip_id)
```

Луч от `start` до `finish` сквозь мир и игроков. `skip_id` — id игрока, которого луч пропускает (по умолчанию — ты сам).

| Поле | Описание |
| --- | --- |
| `fraction` | доля пути до препятствия: 1 — луч прошёл целиком |
| `end_pos` | где луч остановился |
| `normal` | нормаль поверхности в точке удара |
| `all_solid` | луч начался внутри препятствия |
| `entity` | id игрока, в которого попал луч (если попал в игрока) |

{% hint style="warning" %}
Работает только в командных событиях (`create_move` и т.п.). В остальных событиях всегда возвращает `fraction = 1`.
{% endhint %}

```lua
local visible = {}

events.on("create_move", function()
    local me = entity.local_player()
    local eye = combat.context().eye
    for _, p in ipairs(entity.players(true)) do
        local head = p.alive and entity.bone(p.id, 6)
        if head then
            local tr = trace.line(eye, head)
            visible[p.id] = tr.entity == p.id or tr.fraction > 0.97
        end
    end
end)
```
