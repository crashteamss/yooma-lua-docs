# storage

Сохранение данных между запусками игры. У каждого скрипта своё хранилище: файл `storage\<имя_скрипта>.json` в папке скриптов.

## storage.set

```lua
storage.set(key, value)
storage.set(key, nil)   -- удалить
```

`value` — число, строка, boolean или таблица из них (вложенные таблицы тоже можно). Функции и userdata сохранить нельзя. Общий размер хранилища — до **64 KiB**.

## storage.get

```lua
local value = storage.get(key)   -- nil, если нет
```

## Когда сохраняется на диск

Хранилище пишется в файл при выключении скрипта (и при выгрузке чита). Если игра упадёт, последние изменения могут потеряться.

```lua
local kills = storage.get("kills") or 0

events.on("game_event", function(e)
    if e.name == "player_death" and e.attacker == 0 then
        kills = kills + 1
        storage.set("kills", kills)
    end
end)

events.on("paint", function()
    render.text(20, 200, "kills total: " .. kills, {255, 255, 255, 255})
end)
```
