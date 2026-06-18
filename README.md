# dream-landings — лендинг-фабрика «Мечта»

Авто-генерируемые лендинги для проекта **Мечта** (CRM `metallportal-crm2`).
Публикуются на **GitHub Pages**: `https://investfreelife.github.io/dream-landings/<slug>/<variant>-<version>/`

## Структура

```
<lead_slug>/
└── <variant>-<version>/
    ├── index.html
    ├── services.html
    ├── gallery.html
    ├── reviews.html
    ├── contacts.html
    └── assets/
        ├── styles.css
        └── photos/*.webp
```

## Кто пушит

- **Cowork-Claude / Hands-агент** — после генерации лендинга
- **GitHub Actions** — авто-валидация и оптимизация (см. `.github/workflows/`)

## Контракт

Полный гайд агенту: `~/Documents/Claude/Projects/Мечта/app/queue/SPEC/LANDING_FACTORY_AGENT_GUIDE.md`

## Правила

1. **Не клади** ничего больше 200 KB на файл (webp обязательно для фото)
2. **Относительные пути** в HTML — лендинг переезжает на свой домен без правок
3. **HTML lint** проходит без ошибок (CI откажет иначе)
4. **Один вариант** = один префикс `<slug>/<variant>-<version>/`. Не пересекаемся.
5. После push в `main` — Pages деплоится автоматом (~30 сек).

## Жизненный цикл

```
парсер парсит лида → CRM (Supabase)
   ↓
агент-кодер берёт данные → /tmp/ собирает HTML+фото
   ↓
git commit + push в этот репо → GitHub Pages
   ↓
POST /api/dream/landings/register → URL запись в CRM
```
