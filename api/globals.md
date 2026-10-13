# Глобальные значения

## print

```lua
print(...)
```

Пишет в лог вкладки lua (то же, что [`client.log`](client.md#client.log)).

## buttons

Маски кнопок для [`cmd`](cmd.md). Комбинируются через `|`, проверяются через `&`.

| Поле | Значение |
| --- | --- |
| `buttons.attack` | 1 |
| `buttons.jump` | 2 |
| `buttons.duck` | 4 |
| `buttons.forward` | 8 |
| `buttons.back` | 16 |
| `buttons.use` | 32 |
| `buttons.moveleft` | 512 |
| `buttons.moveright` | 1024 |
| `buttons.attack2` | 2048 |
| `buttons.reload` | 8192 |
| `buttons.speed` | 65536 (Shift — шаг) |
| `buttons.score` | 8589934592 (Tab) |
| `buttons.zoom` | 17179869184 |
| `buttons.inspect` | 34359738368 |

```lua
local c = cmd.get()
if c.buttons & buttons.attack ~= 0 then
    print("shooting")
end
```

## api

| Поле | Описание |
| --- | --- |
| `api.version` | версия API (сейчас `2`) |
| `api.lua` | версия Lua (`"Lua 5.4.7"`) |
| `api.script` | имя текущего скрипта |
| `api.protect(fn)` | безопасный вызов, см. ниже |
| `api.budget()` | остаток лимита инструкций |

### api.protect

```lua
local ok, err, result1, result2 = api.protect(fn)   -- дальше идут все результаты fn
```

Вызывает `fn` без аргументов. Если всё хорошо: `true, nil, ...результаты fn`. Если ошибка: `false, текст_ошибки`. Если закончился [лимит инструкций](../getting-started/sandbox.md#лимит-инструкций): `false, "budget_exceeded"`, и при этом скрипт **не выключается**.

```lua
local ok, err = api.protect(function()
    -- тяжёлый расчёт
end)
if not ok then print("failed: " .. tostring(err)) end
```

### api.budget

```lua
local b = api.budget()
```

`instructions_remaining` — сколько инструкций ещё можно выполнить в этом вызове. Остальные поля (`time_remaining_ms`, `native_work_remaining`, `protect_depth`, …) — для совместимости.
