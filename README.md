# geosite-builder
Автоматическая сборка `geosite.dat` для V2Ray/Xray и `geosite.db` для Sing-box/Mihomo на основе актуальных списков доменов.
## 📦 Возможности
- **Автоматическая сборка** — ежедневное обновление в 02:00 UTC
- **Мультиформатность** — поддержка V2Ray/Xray (`.dat`) и Sing-box/Mihomo (`.db`)
- **Источники данных**:
  - 🇷🇺 Российские домены из [kirilllavrov/whitelists](https://github.com/kirilllavrov/whitelists)
  - ✅ Белый список доменов из [kirilllavrov/whitelists](https://github.com/kirilllavrov/whitelists) (`release/whitelist.txt`)
  - 🌐 Международные списки из [v2fly/domain-list-community](https://github.com/v2fly/domain-list-community)
  - 🚫 Рекламные блокировки из [Hagezi DNS blocklists](https://github.com/hagezi/dns-blocklists)
- **Категоризация** — автоматическая генерация категорий для удобной фильтрации
- **GitHub Releases** — готовые файлы в каждом релизе
- **CDN** — прямая загрузка через jsDelivr
- **Зеркало** — копии файлов в Yandex Object Storage (раздача из РФ)
## 📥 Установка
### V2Ray/Xray
```bash
https://cdn.jsdelivr.net/gh/kirilllavrov/geosite-builder@release/geosite.dat
```
или
```bash
https://raw.githubusercontent.com/kirilllavrov/geosite-builder/release/geosite.dat
```
или (зеркало для РФ)
```bash
https://storage.yandexcloud.net/flyz-cryp-olxn-mojw/geofiles/geosite.dat
```
### Sing-box/Mihomo
```bash
https://cdn.jsdelivr.net/gh/kirilllavrov/geosite-builder@release/geosite.db
```
или
```bash
https://raw.githubusercontent.com/kirilllavrov/geosite-builder/release/geosite.db
```
или (зеркало для РФ)
```bash
https://storage.yandexcloud.net/flyz-cryp-olxn-mojw/geofiles/geosite.db
```
> ℹ️ Целостность `geosite.dat` проверяйте по `geosite.dat.sha256sum` —
> `https://storage.yandexcloud.net/flyz-cryp-olxn-mojw/geofiles/geosite.dat.sha256sum`
> или `https://raw.githubusercontent.com/kirilllavrov/geosite-builder/release/geosite.dat.sha256sum`.
## 🗂 Структура данных
```
data/
├── ru/           # Российские домены
├── whitelist/    # Белый список доменов
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
| **Sync Whitelist** | Обновление белого списка доменов | Ежедневно в 00:15 UTC |
| **Sync Ads Lists** | Обновление рекламных блоков (Hagezi) | Ежедневно в 00:00 UTC |
| **Sync Lists** | Загрузка мобильных, стриминговых и других списков | Ежедневно в 00:10 UTC |
| **Generate Categories** | Автогенерация файлов категорий | По триггеру |
| **Build Geosite** | Сборка финальных файлов `.dat` и `.db` | Ежедневно в 02:00 UTC |

> ℹ️ **Build Geosite** дополнительно выгружает `geosite.dat`, `geosite.db` и `geosite.dat.sha256sum`
> в бакет Yandex Object Storage (`geofiles/`), когда файлы изменились. Для этого нужны секреты
> `YC_S3_KEY_ID` и `YC_S3_SECRET` (статический ключ сервисного аккаунта с ролью `storage.uploader`).
> Ручной запуск с `force_upload` выгружает файлы в бакет даже без изменений.
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
- `whitelist` — белый список доменов
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