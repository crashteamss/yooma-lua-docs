# client

Общие функции: время, экран, клавиши, лог.

## client.log

```lua
client.log(...)
```

Пишет аргументы через пробел в лог вкладки **lua** с префиксом `[имя_скрипта]`. Глобальный `print` делает то же самое.

## client.time

```lua
local t = client.time()   -- number, секунды
```

Время в секундах с монотонного таймера. Удобно для анимаций и задержек.

## client.frametime

```lua
local dt = client.frametime()   -- number, секунды
```

Длительность последнего кадра. Также доступно как `render.frame_time()`.

## client.screen_size

```lua
local w, h = client.screen_size()
```

Размер экрана в пикселях. Также доступно как `render.screen_size()`.

## client.key_down

```lua
local held = client.key_down(vk)   -- boolean
```

Зажата ли клавиша прямо сейчас. `vk` — виртуальный код клавиши Windows (см. [Клавиши](../concepts/types.md#клавиши)).

```lua
if client.key_down(0x12) then -- Alt
    print("alt held")
end
```

## client.unload

```lua
client.unload()
```

Сразу прерывает текущий вызов и выключает скрипт. Код после `client.unload()` не выполняется.

## client.stats

```lua
local s = client.stats()
```

Статистика текущего вызова скрипта:

| Поле | Описание |
| --- | --- |
| `phase` | `"paint"` или `"callback"` |
| `instructions` | сколько инструкций уже выполнено в этом вызове |
| `memory_bytes` | память Lua-состояния скрипта |
| `api_calls`, `native_work`, `time_ms` | служебные счётчики (для совместимости) |

## client.log_level

```lua
client.log_level("info")
```

Оставлено для совместимости, ничего не меняет.
