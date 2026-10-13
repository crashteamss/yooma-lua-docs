# settings

Чтение и изменение настроек самого чита. Каждая настройка адресуется парой **категория + имя**, как в конфиге: например `"ragebot - sniper"`, `"hit chance"`.

## settings.list

```lua
local all = settings.list(filter)   -- filter необязателен
```

Список **переключателей** (галочек) чита, у которых в категории встречается подстрока `filter`. У каждой записи: `category`, `name`, `type` (`"boolean"`), `count`, `writable`. Числовые настройки (слайдеры) в этот список не попадают, но читаются и меняются через `settings.get` / `settings.set` по тому же имени, что в конфиге.

```lua
for _, s in ipairs(settings.list("ragebot")) do
    print(s.category, "/", s.name, s.type)
end
```

## settings.get / settings.set

```lua
local v = settings.get(category, name)
settings.set(category, name, value)
```

`get` возвращает `nil` для неизвестной настройки. `set` меняет значение **навсегда** (как если бы ты сам поменял его в меню).

## settings.override

```lua
settings.override(category, name, value)   -- временно подменить
settings.override(category, name, nil)     -- вернуть как было
```

Временная подмена: настройка меняется, пока скрипт её держит. При выключении скрипта все подмены откатываются сами. Неизвестные имена молча игнорируются, так что скрипт под другую версию не упадёт.

```lua
local low_hc = ui.create("checkbox", "low_hc", "Low hitchance on key", false, {key = 0x05, mode = 1})

events.on("create_move", function()
    if ui.get(low_hc) then
        settings.override("ragebot - sniper", "hit chance", 40)
    else
        settings.override("ragebot - sniper", "hit chance", nil)
    end
end)
```

## Бинды

### settings.bind

```lua
local b = settings.bind(category, name, key, mode)   -- key и mode необязательны
```

Читает (и, если переданы `key`/`mode`, меняет) бинд настройки. Возвращает `{key, mode, active}`. Режимы как у [`ui.create`](ui.md#ui.create).

### settings.binds

```lua
local list = settings.binds()
```

Все настройки с биндом: `category`, `name`, `key`, `mode`, `active`, `value`. Удобно для своего списка кейбиндов.

## Чамсы через settings

Для совместимости со скриптами других клиентов категории `"chams enemy invisible"` и `"chams team invisible"` автоматически отображаются на второй слой (сквозь стены) категорий `"chams enemy"` / `"chams team"`.
