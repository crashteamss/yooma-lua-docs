# render

Рисование поверх игры. **Все функции рисования работают только внутри события `paint`.** Координаты — пиксели экрана, начало в левом верхнем углу.

## Текст

### render.text

```lua
render.text(x, y, text, color, font)
```

`font` — хэндл из [`render.setup_font`](#render.setup_font). Если не указан, используется шрифт меню (SF Pro 13px).

### render.measure_text

```lua
local w, h = render.measure_text(text, font)
```

```lua
local label = "yooma"
local w, h = render.measure_text(label)
render.text(sw / 2 - w / 2, 40, label, {255, 255, 255, 255})
```

## Фигуры

### render.line

```lua
render.line(x1, y1, x2, y2, color, thickness)   -- thickness = 1
```

### render.rect

```lua
render.rect(x, y, w, h, color, filled)
```

Рамка. Если `filled == true` — залитый прямоугольник.

### render.rect_filled_fade

```lua
render.rect_filled_fade(x, y, w, h, top_left, top_right, bottom_right, bottom_left)
```

Градиент: у каждого угла свой цвет.

### render.circle

```lua
render.circle(x, y, radius, color, filled)
```

Окружность. Если `filled == true` — залитый круг.

### render.circle_fade

```lua
render.circle_fade(x, y, radius, inner_color, outer_color)
```

Залитый круг с мягким градиентом от центра (`inner_color`) к краю (`outer_color`).

### render.arc

```lua
render.arc(x, y, radius, start_rad, end_rad, color, thickness, segments)
-- thickness = 1, segments = 64 (2–256)
```

Дуга от угла `start_rad` до `end_rad`. Углы в **радианах** (0 — вправо, по часовой стрелке): `math.rad(90)` = вниз.

### render.polygon / render.concave_polygon

```lua
render.polygon(points, color)
```

Залитый многоугольник. `points` — массив точек вида `{ {x, y}, {x, y}, ... }` (именно массивы из двух чисел).

### render.poly_line

```lua
render.poly_line(points, color, thickness, closed)   -- thickness = 1
```

Ломаная по точкам. `closed == true` замыкает её.

## Мир → экран

### render.world_to_screen

```lua
local x, y, on_screen = render.world_to_screen(position)
```

Переводит точку мира в координаты экрана. Возвращает `nil`, если точка позади камеры. `on_screen` — попадает ли точка в границы экрана.

```lua
for _, p in ipairs(entity.players(true)) do
    local x, y = render.world_to_screen(p.origin)
    if x then
        render.text(x, y, p.name, {255, 255, 255, 255})
    end
end
```

### render.circle_3d / render.circle_filled_3d / render.circle_fade_3d

```lua
render.circle_3d(center, radius, color, normal)
render.circle_filled_3d(center, radius, color, normal)
render.circle_fade_3d(center, radius, inner_color, outer_color, normal)
```

Окружность в мире (например, под ногами игрока). `normal` — вектор плоскости, по умолчанию «вверх».

## Текстуры {#textures}

### render.setup_texture

```lua
local tex, err = render.setup_texture(file)
```

Загружает картинку (png, jpg, svg) по имени файла. Файл ищется в `resources/`, `skeetles/` и в корне папки скриптов. Повторный вызов с тем же файлом возвращает тот же хэндл. При ошибке: `nil, "resource_not_found"` или `nil, "unsupported_image"`.

### render.texture

```lua
render.texture(tex, x, y, w, h, color, rounding)   -- color = белый, rounding = 0
```

### render.texture_rotated

```lua
render.texture_rotated(tex, center_x, center_y, w, h, angle_rad, color)   -- angle_rad = 0
```

Рисует текстуру, повёрнутую вокруг её центра (`center_x`, `center_y`). Угол в **радианах**.

### render.texture_size

```lua
local w, h = render.texture_size(tex)
```

### Текстуры из пикселей

```lua
local tex = render.setup_texture_rgba(bytes, width, height)   -- bytes: строка RGBA, 4 байта на пиксель
local tex = render.create_texture_rgba(width, height)         -- пустая текстура
render.update_texture_pixels(tex, bytes, format, pitch)       -- format = "rgba8"
```

Форматы `update_texture_pixels`: `"rgba8"`, `"bgra8"`, `"bgrx8"`, `"rgb565"`, `"rgb555"`. `pitch` — байт в строке (по умолчанию ширина × размер пикселя). Так можно рисовать своё видео, мини-игры, графики.

## Шрифты {#fonts}

### render.setup_font

```lua
local font, err = render.setup_font(file, size)   -- size = 13
```

Загружает ttf/otf из `fonts/`, `resources/` или корня папки скриптов. При ошибке: `nil, "resource_not_found"` / `nil, "unsupported_font"`.

```lua
local big = render.setup_font("Montserrat-Bold.ttf", 24)

events.on("paint", function()
    render.text(30, 30, "yooma", {255, 255, 255, 255}, big)
end)
```

## Прочее

| Функция | Описание |
| --- | --- |
| `render.screen_size()` | `w, h` — то же, что `client.screen_size` |
| `render.frame_time()` | длительность кадра |
| `render.frame_count()` | номер кадра |
| `render.push_clip_rect`, `render.pop_clip_rect`, `render.builtin_font`, `render.setup_texture_from_memory` | заглушки совместимости, ничего не делают |
