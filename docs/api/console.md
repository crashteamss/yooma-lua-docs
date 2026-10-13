# console

Команды консоли CS2 и сообщения в чат. Команды выполняются не сразу, а через очередь в игровом потоке (до 4 за кадр).

## console.exec

```lua
local ok, err = console.exec(command)
```

Выполняет команду консоли, как если бы ты ввёл её сам. Строка до 1024 символов. При ошибке: `false, "invalid_command"`.

```lua
console.exec("cl_crosshairsize 2; cl_crosshairgap -1")
```

## console.say

```lua
local ok, err = console.say(text, team_only)
```

Сообщение в чат (`team_only == true` — в командный). Текст до 200 символов, без кавычек `"`, `;`, `\` и переводов строк. При ошибке: `false, "invalid_text"`.

## console.print

```lua
console.print(text)
```

Пишет текст в лог вкладки lua.

## console.clear_pending

```lua
console.clear_pending()
```

Удаляет ещё не выполненные команды из очереди.
