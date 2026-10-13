# chams

Свои материалы для игроков, рук и оружия. Чамсы из скрипта **приоритетнее** настроек меню: пока скрипт задаёт слой, меню для этой цели не используется.

## Цели

```lua
local list = chams.targets()
```

| Цель | Что красит |
| --- | --- |
| `enemy` | противники, видимые |
| `enemy_invisible` | противники за стенами |
| `team` | союзники, видимые |
| `team_invisible` | союзники за стенами |
| `local` | ты (от третьего лица) |
| `arms` | твои руки |
| `weapon` | твоё оружие в руках |

`attachments` есть в списке для совместимости, но в этой сборке ничего не делает.

## Материалы

Каждая функция возвращает **хэндл** материала. На скрипт — до 8 материалов.

### chams.builtin_material

```lua
local mat = chams.builtin_material(name)   -- "flat", "glow", "bloom"
```

Встроенные материалы чита.

### chams.create_material

```lua
local mat = chams.create_material(brightness, tint)
```

Сплошной цвет (шейдер `solidcolor`). `brightness` от 0.1 до 8 (по умолчанию 1), `tint` — цвет, который «зашивается» в материал. Создаётся сразу в двух вариантах: обычный и сквозь стены.

### chams.load_material

```lua
local mat = chams.load_material(path, ignorez_path)
```

Загружает готовый материал игры или мода по пути `.vmat` (например `"materials/dev/glow.vmat"`). `ignorez_path` — необязательный материал для слоя сквозь стены. При ошибке: `nil, "material_not_found_or_unavailable"`.

## chams.set

```lua
chams.set(target, material, color)   -- включить
chams.set(target, nil)               -- вернуть управление меню
```

Для `enemy_invisible` и `team_invisible` нужен материал со вторым (сквозь стены) вариантом: `builtin_material` и `create_material` его имеют, у `load_material` укажи `ignorez_path`.

```lua
local glow = chams.builtin_material("glow")
local flat = chams.create_material(1.0, {255, 255, 255, 255})

chams.set("enemy",           glow, {255, 80, 120, 255})
chams.set("enemy_invisible", flat, {120, 80, 255, 160})

events.on("shutdown", function()
    chams.set("enemy", nil)
    chams.set("enemy_invisible", nil)
end)
```

{% hint style="info" %}
При выключении скрипта его чамсы снимаются автоматически. `shutdown` в примере нужен только для наглядности.
{% endhint %}
