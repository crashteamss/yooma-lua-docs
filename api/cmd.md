# cmd

Доступ к команде, которая уходит на сервер в текущем тике. **Работает только в командных событиях**: `create_move`, `pre_rage`, `post_rage`, `post_move`, `finalize_command`. В других событиях вызов даёт ошибку `cmd is only available in command callbacks`.

Маски кнопок — в глобальной таблице [`buttons`](globals.md#buttons).

## cmd.get

```lua
local c = cmd.get()
```

| Поле | Описание |
| --- | --- |
| `number` | номер команды |
| `buttons` | зажатые кнопки (маска) |
| `buttons_changed` | кнопки, состояние которых изменилось в этом тике |
| `buttons_scroll` | кнопки, нажатые колесом |
| `forward` | движение вперёд/назад, от −1 до 1 |
| `side` | движение вбок, от −1 до 1 (**плюс — вправо**) |
| `up` | вверх/вниз |
| `angles` | углы взгляда команды (vector) |

## cmd.set_button

```lua
cmd.set_button(mask, pressed)
```

Зажимает (`true`) или отпускает (`false`) кнопки в маске.

```lua
cmd.set_button(buttons.jump, false)              -- не прыгать
cmd.set_button(buttons.duck | buttons.use, true)
```

## cmd.set_button_state

```lua
cmd.set_button_state(mask, held, changed, scroll)
```

Полный контроль: задаёт бит в `buttons`, `buttons_changed` и `buttons_scroll` одновременно.

## cmd.set_movement

```lua
cmd.set_movement(forward, side, up)   -- up можно не указывать
```

Значения от −1 до 1. `side` положительный = вправо.

## cmd.set_angles

```lua
cmd.set_angles(angles)
```

Меняет углы команды, то есть куда «смотрит» сервер, не трогая твой экран. Движение поворачивается вместе с углами, так что ты продолжаешь идти туда же. Команды, в которых есть выстрел, не меняются: рейджбот ставит их сам.

## Subtick-шаги

CS2 делит тик на шаги («субтики»): каждое нажатие и движение имеет точное время внутри тика.

### cmd.add_subtick

```lua
local ok = cmd.add_subtick(when, forward_delta, side_delta, button, pressed)
```

| Параметр | Описание |
| --- | --- |
| `when` | момент внутри тика, строго между 0 и 1 |
| `forward_delta`, `side_delta` | изменение движения в этот момент (−2…2), по умолчанию 0 |
| `button` | маска кнопки (0 — без кнопки) |
| `pressed` | нажата ли кнопка в этот момент |

Возвращает `false`, если `when` вне диапазона или шагов уже 64.

### cmd.clear_subticks

```lua
cmd.clear_subticks()
```

Удаляет все subtick-шаги команды.

## Заглушки совместимости

`cmd.set_shot_angles`, `cmd.set_attack_start`, `cmd.set_history_ticks` есть, но ничего не делают.
