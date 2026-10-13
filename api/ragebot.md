# ragebot

Модуль для совместимости со скриптами других клиентов. Управлять рейджботом напрямую из Lua пока нельзя: меняй его настройки через [`settings.override`](settings.md#settings.override).

## ragebot.pattern

```lua
local points = ragebot.pattern(kind, count, rings, phase)   -- rings = 4, phase = 0
```

Генерирует набор 2D-точек в единичном круге: `{ {x, y}, ... }`, до 128 штук. `kind = "spiral"` даёт равномерную спираль (золотой угол), иначе точки раскладываются по `rings` кольцам. Удобно для своих визуализаций разброса или мультипоинтов.

## Заглушки

| Функция | Поведение |
| --- | --- |
| `ragebot.hitchance()` | возвращает `0` |
| `ragebot.multipoints()` | возвращает пустую таблицу |
| `ragebot.claim_command()` | ничего не делает |
