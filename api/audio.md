# audio

Проигрывание своих звуков (mp3, wav). Файлы ищутся в `muzon/`, `sounds/` и в корне папки скриптов. У скрипта один «канал»: новый `audio.play` останавливает предыдущий звук.

## audio.play

```lua
local ok, err = audio.play(file, loop)
```

`loop == true` — играть по кругу (только mp3). При ошибке: `false, "audio_not_found"` или `false, "audio_open_failed"`.

## audio.stop

```lua
audio.stop()
```

## audio.volume

```lua
audio.volume(percent)   -- 0 … 100
```

Громкость в процентах для текущего и следующих звуков скрипта.

## audio.preload

```lua
local ok, err = audio.preload(file)
```

Проверяет, что файл есть: `true` или `false, "audio_not_found"`.

```lua
events.on("game_event", function(e)
    if e.name == "player_death" and e.attacker == 0 then
        audio.play("kill.wav")
    end
end)
```
