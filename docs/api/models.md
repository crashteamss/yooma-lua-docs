# models

Своя модель для твоего персонажа. Её видишь только ты (в третьем лице, в превью, на своём клиенте), другие игроки видят обычную модель. Модели берутся из файлов игры.

## Куда класть модели

Скомпилированные модели CS2 (`.vmdl_c` вместе с материалами `.vmat_c` и текстурами `.vtex_c`) кладутся в папку игры:

```
...\Counter-Strike Global Offensive\game\csgo\characters\models\   ← основная папка
...\Counter-Strike Global Offensive\game\csgo\agents\models\       ← для моделей, которые ссылаются на agents/models
```

Структуру папок из архива модели нужно сохранить: материалы ищутся по путям, зашитым в саму модель.

{% hint style="warning" %}
Старые модели, собранные под прошлую систему анимаций CS2 (они ссылаются на `player_ct.vanmgrph`), загружаются, но стоят в T-позе. Бери модели, обновлённые под текущую CS2.
{% endhint %}

## models.list

```lua
local list = models.list()
```

Массив найденных моделей: `{id, path}`. `id` — то, что передаётся в `models.set`. Модели рук (`*_arm`) в список не входят.

```lua
for i, m in ipairs(models.list()) do
    print(i, m.id)
end
```

## models.set

```lua
local path = models.set(id)
```

Ставит модель. `id` должен оканчиваться на `.vmdl_c` и существовать, иначе ошибка `invalid model id` / `model file not found`.

## models.clear / models.current / models.status

```lua
models.clear()                 -- вернуть обычную модель
local path = models.current()  -- текущая модель скрипта или nil
local s = models.status()      -- "idle", "ready" или "error"
```

## Пример: выбор модели из списка

```lua
local list = models.list()
local names = {"default"}
for _, m in ipairs(list) do names[#names + 1] = m.id end

local pick = ui.create("combo", "model", "Model", 1, names)
local last = 1

events.on("frame_stage", function()
    local i = ui.get(pick)
    if i == last then return end
    last = i
    if i == 1 then models.clear() else models.set(list[i - 1].id) end
end)
```

При выключении скрипта модель автоматически сбрасывается.
