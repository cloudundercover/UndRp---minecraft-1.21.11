# UndRp

Paper/Bukkit plugin for **Minecraft 1.21.11** that exposes a player's server resource-pack status through **PlaceholderAPI**.

Плагин для Paper/Bukkit под **Minecraft 1.21.11**, показывающий статус серверного ресурс-пака игрока через **PlaceholderAPI**.

---

## English

### What it does
UndRp listens for Minecraft's `PlayerResourcePackStatusEvent` - the event fired when the server sends a player a resource pack and the client reports back whether it was applied, declined, failed to download, etc. The plugin remembers the latest status per player and exposes it as a configurable PlaceholderAPI placeholder, so you can show "resource pack installed / not installed" style text anywhere placeholders are supported (scoreboards, tab list, chat, GUIs, other plugins).

### How it works
- **`UndRp` (main class)** - on enable, loads `config.yml`, registers the event listener, and (if PlaceholderAPI is present) registers the placeholder expansion.
- **`ResourcePackListener`** - listens to `PlayerResourcePackStatusEvent` and stores the player's latest status; clears stored data when the player disconnects. Optional debug logging on status changes.
- **`PlayerDataManager`** - thread-safe in-memory map (`UUID → status`) of resource-pack statuses. A pack counts as "applied" if the status is `SUCCESSFULLY_LOADED` or `DOWNLOADED`.
- **`ConfigManager`** - reads any number of modules named `rp-info-<N>` from `config.yml`, each with an `rp` text (pack applied) and `no-rp` text (pack not applied).
- **`UndRpExpansion`** - the PlaceholderAPI expansion. Placeholder identifier is `undrp`; for each module `rp-info-<N>` it exposes `%undrp_info_<N>%`, which resolves to the module's `rp` or `no-rp` text depending on the requesting player's current resource-pack status.

### Configuration (`config.yml`)
```yaml
debug: false

rp-info-1:
  rp: "Resource pack installed"
  no-rp: "Resource pack not installed"
```
Add more modules (`rp-info-2`, `rp-info-3`, ...) to get more independent placeholders - useful if you want different wording/formatting in different UI contexts.

### Requirements
- Minecraft 1.21 (built against Paper API `1.21.11-R0.1-SNAPSHOT`)
- [PlaceholderAPI](https://www.spigotmc.org/resources/placeholderapi.6245/) (hard dependency)
- Java 21

### Placeholder
```
%undrp_info_<N>%
```
where `<N>` matches a `rp-info-<N>` block in `config.yml`.

---

## Русский

### Что делает плагин
UndRp отслеживает игровое событие `PlayerResourcePackStatusEvent` - оно срабатывает, когда сервер отправляет игроку ресурс-пак, а клиент сообщает результат (успешно загружен, отклонён, ошибка загрузки и т.д.). Плагин запоминает последний статус для каждого игрока и предоставляет его в виде настраиваемого плейсхолдера PlaceholderAPI, чтобы показывать текст вида "ресурс-пак установлен / не установлен" где угодно, где поддерживаются плейсхолдеры (скорборды, таб-лист, чат, GUI других плагинов).

### Как это устроено
- **`UndRp` (главный класс)** - при включении загружает `config.yml`, регистрирует слушатель событий и (если найден PlaceholderAPI) регистрирует экспансию плейсхолдеров.
- **`ResourcePackListener`** - слушает `PlayerResourcePackStatusEvent`, сохраняет последний статус игрока; очищает данные при выходе игрока с сервера. Есть опциональный debug-лог при изменении статуса.
- **`PlayerDataManager`** - потокобезопасная мапа в памяти (`UUID → статус`). Пак считается "установленным", если статус `SUCCESSFULLY_LOADED` или `DOWNLOADED`.
- **`ConfigManager`** - читает из `config.yml` произвольное число модулей `rp-info-<N>`, у каждого есть текст `rp` (пак установлен) и `no-rp` (пак не установлен).
- **`UndRpExpansion`** - сама экспансия PlaceholderAPI. Идентификатор - `undrp`; для каждого модуля `rp-info-<N>` регистрируется плейсхолдер `%undrp_info_<N>%`, который возвращает текст `rp` или `no-rp` в зависимости от текущего статуса ресурс-пака у игрока, запросившего плейсхолдер.

### Конфигурация (`config.yml`)
```yaml
debug: false

rp-info-1:
  rp: "Ресурс пак установлен"
  no-rp: "Ресурс пак не установлен"
```
Можно добавлять новые модули (`rp-info-2`, `rp-info-3` и т.д.), чтобы получить несколько независимых плейсхолдеров - например, с разным текстом/форматированием для разных мест интерфейса.

### Требования
- Minecraft 1.21 (собрано под Paper API `1.21.11-R0.1-SNAPSHOT`)
- [PlaceholderAPI](https://www.spigotmc.org/resources/placeholderapi.6245/) (обязательная зависимость)
- Java 21

### Плейсхолдер
```
%undrp_info_<N>%
```
где `<N>` - номер соответствующего блока `rp-info-<N>` в `config.yml`.
