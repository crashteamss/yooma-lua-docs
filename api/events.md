# events

Подписка на события. Список событий и потоков: [События и потоки](../concepts/events.md).

## events.on

```lua
local handle = events.on(name, callback)
```

| Параметр | Тип | Описание |
| --- | --- | --- |
| `name` | string | имя события (`"paint"`, `"create_move"`, `"game_event"`, …) |
| `callback` | function | обработчик |

Возвращает `handle` (integer), с ним можно отписаться. На одно событие можно повесить сколько угодно обработчиков, они вызываются в порядке подписки.

```lua
events.on("frame_stage", function(e)
    -- e.stage — номер стадии кадра
end)
```

## events.off

```lua
events.off(handle)
```

Снимает обработчик, который вернул `events.on`.

```lua
local h
h = events.on("paint", function()
    render.text(20, 20, "показываюсь один раз", {255, 255, 255, 255})
    events.off(h)
end)
```
