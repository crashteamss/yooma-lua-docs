# Хит-логи с сохранением

Список последних попаданий на экране и общая статистика урона, которая сохраняется между запусками игры.

```lua
local stats = storage.get("stats") or { hits = 0, damage = 0, headshots = 0 }
local feed = {}

local hitgroups = { [1] = "head", [2] = "chest", [3] = "stomach", [4] = "left arm",
                    [5] = "right arm", [6] = "left leg", [7] = "right leg", [8] = "neck" }

events.on("game_event", function(e)
    if e.name ~= "player_hurt" or e.attacker ~= 0 then return end

    local victim = entity.get(e.userid)
    local part = hitgroups[e.hitgroup] or "body"

    stats.hits = stats.hits + 1
    stats.damage = stats.damage + e.damage
    if e.hitgroup == 1 then stats.headshots = stats.headshots + 1 end
    storage.set("stats", stats)

    table.insert(feed, 1, {
        text = ("%s  -%d  %s  (%d hp)"):format(victim and victim.name or "?", e.damage, part, e.health),
        time = client.time(),
    })
    if #feed > 6 then table.remove(feed) end
end)

events.on("paint", function()
    local now = client.time()
    local y = 300

    for _, item in ipairs(feed) do
        local age = now - item.time
        if age < 5 then
            local alpha = math.floor(255 * math.min(1, (5 - age) / 0.5))
            render.text(20, y, item.text, {255, 255, 255, alpha})
            y = y + 16
        end
    end

    render.text(20, y + 8, ("hits %d  dmg %d  hs %d"):format(stats.hits, stats.damage, stats.headshots),
        {150, 190, 255, 255})
end)
```
