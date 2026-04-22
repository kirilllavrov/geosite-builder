# geosite-builder
Автоматическая сборка `geosite.dat` для V2Ray/Xray и `geosite.db` для Sing-box/Mihomo на основе актуальных списков доменов.
## 📦 Возможности
- **Автоматическая сборка** — ежедневное обновление в 02:00 UTC
- **Мультиформатность** — поддержка V2Ray/Xray (`.dat`) и Sing-box/Mihomo (`.db`)
- **Источники данных**:
  - 🇷🇺 Российские домены из [kirilllavrov/whitelists](https://github.com/kirilllavrov/whitelists)
  - 🌐 Международные списки из [v2fly/domain-list-community](https://github.com/v2fly/domain-list-community)
  - 🚫 Рекламные блокировки из [Hagezi DNS blocklists](https://github.com/hagezi/dns-blocklists)
- **Категоризация** — автоматическая генерация категорий для удобной фильтрации
- **GitHub Releases** — готовые файлы в каждом релизе
- **CDN** — прямая загрузка через jsDelivr
## 📥 Установка
### Через CDN (рекомендуется)
#### V2Ray/Xray с jsDelivr CDN
```bash
https://cdn.jsdelivr.net/gh/kirilllavrov/geosite-builder@release/geosite.dat
```
или
#### V2Ray/Xray с GitHub (прямой)
```bash
https://raw.githubusercontent.com/kirilllavrov/geosite-builder/release/geosite.dat
```
#### Sing-box/Mihomo с jsDelivr CDN
```bash
https://cdn.jsdelivr.net/gh/kirilllavrov/geosite-builder@release/geosite.db
```
или
##### Sing-box/Mihomo с GitHub (прямой)
```bash
https://raw.githubusercontent.com/kirilllavrov/geosite-builder/release/geosite.db
```
## 🗂 Структура данных
```
data/
├── ru/           # Российские домены
├── mobile/       # Мобильные сервисы (Apple, Huawei, Xiaomi и др.)
├── streaming/    # Стриминговые платформы (YouTube, Netflix)
├── browser/      # Браузеры и связанные сервисы
├── games/        # Игровые сервисы
├── ads/          # Рекламные блокировки
├── private.d/    # Приватные домены
└── category-*    # Автогенерируемые категории
```
## ⚙️ GitHub Actions Workflow
| Workflow | Описание | Расписание |
|----------|----------|------------|
| **Sync RU Lists** | Синхронизация российских доменов | Ежедневно в 00:05 UTC |
| **Sync Ads Lists** | Обновление рекламных блоков (Hagezi) | Ежедневно в 00:00 UTC |
| **Sync Lists** | Загрузка мобильных, стриминговых и других списков | Ежедневно в 00:10 UTC |
| **Generate Categories** | Автогенерация файлов категорий | По триггеру |
| **Build Geosite** | Сборка финальных файлов `.dat` и `.db` | Ежедневно в 02:00 UTC |
## 🚀 Использование
### V2Ray / Xray
```json
{
  "routing": {
    "rules": [
      {
        "type": "field",
        "domain": ["geosite:ru"],
        "outboundTag": "direct"
      },
      {
        "type": "field",
        "domain": ["geosite:category-ads"],
        "outboundTag": "block"
      }
    ]
  }
}
```
### Sing-box
```json
{
  "route": {
    "rules": [
      {
        "domain": ["geosite:ru"],
        "outbound": "direct"
      },
      {
        "domain": ["geosite:category-ads"],
        "outbound": "block"
      }
    ]
  }
}
```
### Mihomo (Clash.Meta)
```yaml
rules:
  - DOMAIN-SUFFIX,example.ru,DIRECT
  - GEOSITE,category-ads,REJECT
```
## 📊 Доступные категории
- `ru` — российские домены
- `mobile` — мобильные сервисы
- `streaming` — стриминговые платформы
- `browser` — браузеры
- `games` — игровые сервисы
- `ads` — рекламные домены
- `private` — приватные домены
Полный список доступен в директории `data/`.
## 📝 Лицензия
Данный проект использует данные из открытых источников:
- [v2fly/domain-list-community](https://github.com/v2fly/domain-list-community) (MIT)
- [kirilllavrov/whitelists](https://github.com/kirilllavrov/whitelists)
- [hagezi/dns-blocklists](https://github.com/hagezi/dns-blocklists)
## 🔗 Ссылки
- [V2Ray документация](https://www.v2ray.com/en/configuration/routing.html)
- [Sing-box документация](https://sing-box.sagernet.org/)
- [Mihomo документация](https://wiki.metacubex.one/)