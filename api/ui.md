# ui

Свои элементы в меню и отдельные окна. Элементы скрипта показываются во вкладке **lua** под именем скрипта (или в окне, если [привязать их к окну](#ui.attach_window)).

## ui.create

```lua
local id = ui.create(kind, id, label, ...)
```

Создаёт элемент и возвращает его **id** (ту же строку `id`). Если элемент с таким id уже есть, просто возвращает id. `label` можно не указывать, тогда подписью будет id.

| `kind` | Дополнительные аргументы | Значение `ui.get` |
| --- | --- | --- |
| `"checkbox"` | `default` (boolean), `bind` (таблица `{key = vk, mode = n}`) | boolean |
| `"slider"` | `default`, `min` (0), `max` (100) | number¹ |
| `"combo"` | `default` (номер, с 1), `options` (таблица строк, обязательна) | integer (с 1) |
| `"color"` | `default` (цвет) | цвет |
| `"text"` | `default` (строка) | string |

¹ Если `default`, `min` и `max` — целые числа, слайдер целочисленный и `ui.get` возвращает integer.

Режимы бинда (`mode`) для чекбокса: `0` — переключение по нажатию, `1` — включено, пока клавиша зажата, `2` — выключено, пока клавиша зажата.

```lua
local on    = ui.create("checkbox", "on", "Enabled", true, {key = 0x05, mode = 0})
local alpha = ui.create("slider", "alpha", "Alpha", 200, 0, 255)
local style = ui.create("combo", "style", "Style", 1, {"Line", "Box", "Circle"})
local col   = ui.create("color", "col", "Color", {255, 120, 60, 255})
local text  = ui.create("text", "text", "Caption", "yooma")
```

## ui.get / ui.set

```lua
local value = ui.get(id)
ui.set(id, value)
```

Читает и меняет значение элемента. Для неизвестного id `ui.get` возвращает `nil`, а `ui.set` ничего не делает. Значение слайдера ограничивается его диапазоном, номер в combo — числом вариантов.

## ui.is_menu_opened

```lua
local open = ui.is_menu_opened()   -- boolean
```

## ui.mouse_state

```lua
local m = ui.mouse_state()
```

| Поле | Описание |
| --- | --- |
| `x`, `y` | позиция курсора |
| `down` | зажата ЛКМ |
| `right_down` | зажата ПКМ |
| `clicked` | ЛКМ нажата в этом кадре |

## ui.get_menu_rect

```lua
local r = ui.get_menu_rect()   -- {x, y, w, h}
```

Положение и размер окна меню.

## Окна

Скрипт может создать до **8** своих окон и разместить в них свои элементы.

### ui.create_window

```lua
local win = ui.create_window(id, title, options)
```

| Параметр | Описание |
| --- | --- |
| `id` | уникальный id окна (1–80 символов) |
| `title` | заголовок (по умолчанию id) |
| `options` | таблица, необязательна: `width`, `height`, `key` (клавиша открытия), `open` (boolean) |

Возвращает id окна. Окно **без клавиши** (`key = 0`) показывается вместе с меню.

### ui.attach_window

```lua
ui.attach_window(control_id, win)
```

Переносит элемент из вкладки lua в окно.

### ui.window_bind

```lua
ui.window_bind(win, vk)   -- vk = nil или 0, чтобы окно снова следовало за меню
```

### ui.set_window_open

```lua
ui.set_window_open(win, true)
```

### ui.get_window

```lua
local w = ui.get_window(win)
```

Поля: `id`, `title`, `x`, `y`, `width`, `height`, `key`, `open`, `follows_menu`.

## Заглушки совместимости

`ui.set_menu_font`, `ui.get_menu_font`, `ui.set_language_font`, `ui.register_language`, `ui.unregister_language`, `ui.set_language`, `ui.get_language`, `ui.translate`, `ui.get_translation_keys`, `ui.get_languages` есть, чтобы скрипты от других клиентов не падали, но ничего не делают.
