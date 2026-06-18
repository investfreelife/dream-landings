# dream-landings — Storage парсера «Мечта»

⚠️ **Это НЕ сайт.** Это **сырьё парсера** для проекта Мечта.

## Зачем

Чтобы не убивать Supabase Storage квоту, мы храним **тяжёлые файлы** (фото, raw HTML, json'ы) **в этом репо** на GitHub. Бесплатно, навсегда, через `raw.githubusercontent.com` доступно агентам.

## Что куда

| | Что | Куда |
|---|---|---|
| **Сюда** (этот репо) | Сырьё парсера: фото лидов webp, data.json, reviews.json, services.json | `<slug>/photos/NN.webp`, `<slug>/data.json` |
| **`investfreelife.github.io`** | Готовые лендинги для показа клиентам (продаём) | отдельный репо |
| **Supabase CRM** | Только записи: URL фото, путь до сырья, метаданные | без файлов |

## Структура

```
<lead_slug>/
  ├── data.json           ← name, niche, phone, address, hours, features, rating, ...
  ├── reviews.json        ← {rating, count, sample[50]}
  ├── services.json       ← [{name, price, unit, source}, ...]
  ├── photos.json         ← [{idx, yandex_url, raw_url, priority, deleted}]
  └── photos/
      ├── 01.webp         ← оптимизированное фото (≤200 KB)
      ├── 02.webp
      └── ...
```

## Workflow

```
1. ПАРСЕР Я.Карт скачивает → сжимает в webp → грузит сюда
2. Регистрирует URL в CRM (Supabase) dream_lead_photos.url = raw.githubusercontent.com/.../01.webp
3. Sergey в CRM:
   - помечает мусорные фото 🗑 deleted=true
   - помечает приоритетные ⭐ priority=true
4. АГЕНТ-КОДЕР читает CRM → берёт ТОЛЬКО priority=true + deleted=false
5. Собирает лендинг → пушит в investfreelife.github.io
6. Регистрирует финальный URL в CRM dream_landings.entry_url
```

## Доступ агентам

Чтение (любой агент):
```
https://raw.githubusercontent.com/investfreelife/dream-landings/main/<slug>/photos/01.webp
https://raw.githubusercontent.com/investfreelife/dream-landings/main/<slug>/data.json
```

Запись (агент-парсер):
```bash
git clone https://github.com/investfreelife/dream-landings.git
cp /tmp/parsed/<slug>/* dream-landings/<slug>/
cd dream-landings && git add <slug>/ && git commit -m "<slug>: parsed by agent" && git push
```

## Правила (соблюдают парсер и Sergey)

1. **Webp обязательно** для фото. Не jpg/png. 1200px по большей стороне, quality 78.
2. **≤200 KB на файл**. Если больше — пересжатие.
3. **Не пиши в чужой `<slug>/`**. Один лид = одна папка.
4. **Не клади сюда HTML лендингов** — это для `investfreelife.github.io`.
5. **Не клади приватные данные** (внутренние ID, заметки, секреты).
6. **Удалить можно из CRM** — pomeчaeшь `deleted=true`, физическое удаление пакетом раз в неделю.

## Связано

- CRM: https://metallportal-crm2.vercel.app/dream
- Tenant ID: `11111111-2222-3333-4444-555555555555`
- Готовые сайты: https://investfreelife.github.io/
- Контракт парсера: `~/Documents/Claude/Projects/Мечта/app/queue/SPEC/CRM_DATA_CONTRACT.md`
- Гайд агента-кодера: `~/Documents/Claude/Projects/Мечта/app/queue/SPEC/LANDING_FACTORY_AGENT_GUIDE.md`
