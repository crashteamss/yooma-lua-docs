# События и потоки

Скрипт подписывается на события через [`events.on`](../api/events.md). Обработчики вызываются в двух разных потоках, и от этого зависит, какие функции в них доступны.

## Список событий

| Событие | Когда | Поток | Аргумент |
| --- | --- | --- | --- |
| `paint` | каждый кадр | рендер | — |
| `create_move` | каждый тик, до рейджбота | игровой | — |
| `pre_rage` | перед рейджботом | игровой | — |
| `post_rage` | после рейджбота | игровой | — |
| `post_move` | после стрейфера и движения | игровой | — |
| `finalize_command` | последний шаг перед отправкой команды | игровой | — |
| `frame_stage` | стадии кадра движка | игровой | `{stage = n}` |
| `game_event` | игровые события | игровой | см. ниже |
| `shutdown` | скрипт выключается или выгружается | — | — |

Порядок внутри одного тика: `create_move` → `pre_rage` → рейджбот → `post_rage` → движение (стрейфер) → `post_move` → `finalize_command`.

## Что где работает

| Функции | `paint` | командные события¹ | `frame_stage`, `game_event` |
| --- | --- | --- | --- |
| `render.*` (рисование) | да | нет | нет |
| `cmd.*` | нет | да | нет |
| `trace.line`, `combat.damage` | нет² | да | нет² |
| `combat.records`, `entity.*`, `ui.*`, `settings.*`, `storage.*` | да | да | да |

¹ `create_move`, `pre_rage`, `post_rage`, `post_move`, `finalize_command`.
² Трейсы движка безопасны только на тике команды. Вне его `trace.line` вернёт «чистую» линию (`fraction = 1`), а `combat.damage` вернёт `nil`. Посчитай нужное в `create_move` и сохрани в переменную, а рисуй это в `paint`.

{% hint style="warning" %}
Рисовать можно **только** в `paint`. Вызов `render.text` и других функций рисования в другом событии приводит к ошибке `render drawing is only available in paint`.
{% endhint %}

## game_event

Сейчас скрипту приходят 4 игровых события. Аргумент — таблица:

| Поле | Есть у | Описание |
| --- | --- | --- |
| `name` | всех | `player_hurt`, `player_death`, `bullet_impact` или `round_start` |
| `userid` | у всех, кроме `round_start` | id игрока, с которым случилось событие |
| `attacker` | `player_hurt`, `player_death` | id атакующего |
| `position` | `bullet_impact` | точка попадания пули (vector) |
| `damage`, `dmg_health` | `player_hurt` | нанесённый урон |
| `health` | `player_hurt` | здоровье после попадания |
| `hitgroup` | `player_hurt` | куда попали (см. [хитгруппы](types.md#хитгруппы)) |

```lua
events.on("game_event", function(e)
    if e.name == "player_hurt" and e.attacker == 0 then
        local victim = entity.get(e.userid)
        print(("hit %s for %d (%d left)"):format(victim and victim.name or "?", e.damage, e.health))
    end
end)
```
