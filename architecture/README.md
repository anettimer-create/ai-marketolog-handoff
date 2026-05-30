# Архитектура блога AI-Маркетолог 2.0

**Дата:** 2026-05-30
**Стек:** Next.js 14 App Router + Tailwind + Vercel
**Палитра:** чёрный `#000000` + жёлтый `#F5C518` (Vlad-стиль) + синий `#9333ea` Claude точечно
**ToV:** Vlad Yasko (`stories-yasko/references/tov_yasko.md`)
**Цель:** холодный органический + AI-трафик → курс Vlad'а

---

## Принципы

1. **Минимум страниц, максимум содержания.** На MVP — 4 типа страниц: главная, статья, лендинг курса, about автора. Без CMS, без админки, без личного кабинета.
2. **SSR не SSG не CSR.** Обновляем контент часто, AI-краулеры должны видеть полный HTML, JS не выполняется.
3. **Глубина 2 уровня.** Hub & Spoke. Pillar = pillar URL, spoke = подпапка под pillar. Глубже не уходим.
4. **Schema на каждой странице.** Article + Person + Organization + BreadcrumbList + FAQPage + HowTo. JSON-LD в `<head>`.
5. **Один CTA = курс Vlad'а.** Все статьи ведут в одно место: `/kurs` → внешняя ссылка на `aimarketolog.site`.
6. **Без em-dash, без «воронка/лиды/прогрев», без эмодзи в каждом предложении.** ToV-маркеры Vlad'а в контенте.

---

## Page Hierarchy

```
Homepage (/)
├── Blog index (/blog)
│   ├── Pillar (/blog/claude-code-marketing)           [C01]
│   │   ├── Spoke (/blog/claude-code-marketing/skills)
│   │   ├── Spoke (/blog/claude-code-marketing/projects)
│   │   └── Spoke (/blog/claude-code-marketing/install-windows)
│   ├── Pillar (/blog/3-ai-sotrudnika)                 [C02]
│   │   ├── Spoke (/blog/3-ai-sotrudnika/assistent)
│   │   ├── Spoke (/blog/3-ai-sotrudnika/operationka)
│   │   └── Spoke (/blog/3-ai-sotrudnika/avtonomnyy-agent)
│   ├── Pillar (/blog/vibe-coding-landing)             [C03]
│   │   ├── Spoke (/blog/vibe-coding-landing/numerolog-za-60-min) [C05]
│   │   └── Spoke (/blog/vibe-coding-landing/deploy-vercel)
│   ├── Pillar (/blog/deep-research-ai)                [C04]
│   ├── Pillar (/blog/upakovka-produkta-ai)            [C05]
│   ├── Pillar (/blog/content-za-chas)                 [C06]
│   ├── Pillar (/blog/ai-prodazi-telegram)             [C07]
│   ├── Pillar (/blog/avtomatizatsiya-marketinga)      [C08]
│   ├── Pillar (/blog/soloeksperт-ai)                  [C09]
│   ├── Pillar (/blog/ai-vs-marketolog)                [C10]  ★ MVP-волна 1
│   └── Pillar (/blog/ai-stack-2026)                   [C11]
├── /kurs                                              лендинг + редирект на aimarketolog.site
├── /vlad                                              about автора + Entity для AI
├── /llms.txt                                          для AI-агентов
├── /robots.txt                                        с Allow для AI-ботов
└── /sitemap.xml                                       автогенерация
```

**Глубина:** 3 уровня (homepage → pillar → spoke). Соответствует «3-Click Rule».

---

## Visual Sitemap

```mermaid
graph TD
    HOME[/ Homepage]:::primary
    BLOG[/blog Index]:::primary
    KURS[/kurs CTA]:::cta
    VLAD[/vlad About]:::primary

    HOME --> BLOG
    HOME --> KURS
    HOME --> VLAD

    BLOG --> P01[Claude Code Marketing]
    BLOG --> P02[3 AI-сотрудника]
    BLOG --> P03[Vibe Coding]
    BLOG --> P10[AI vs Маркетолог]
    BLOG --> PETC[... 8 других pillar]

    P03 --> S05[Numerolog 60min]

    P01 -.->|CTA| KURS
    P02 -.->|CTA| KURS
    P03 -.->|CTA| KURS
    P10 -.->|CTA| KURS
    S05 -.->|CTA| KURS

    KURS -->|external link| AIMK[aimarketolog.site]:::external

    classDef primary fill:#000,color:#fff,stroke:#F5C518,stroke-width:2px
    classDef cta fill:#F5C518,color:#000,stroke:#000
    classDef external fill:#9333ea,color:#fff
```

---

## URL Map (полная таблица)

| Страница | URL | Шаблон | Уровень | Приоритет | Кластер |
|---|---|---|---|---|---|
| Главная | `/` | `home` | L0 | High | все |
| Блог-индекс | `/blog` | `blog-index` | L1 | High | все |
| Pillar Claude Code | `/blog/claude-code-marketing` | `pillar` | L2 | High | C01 |
| Pillar 3 AI-сотрудника | `/blog/3-ai-sotrudnika` | `pillar` | L2 | High | C02 |
| Pillar Vibe Coding | `/blog/vibe-coding-landing` | `pillar` | L2 | High | C03 |
| Pillar AI vs Маркетолог | `/blog/ai-vs-marketolog` | `pillar` | L2 | High ★ | C10 |
| Spoke Numerolog | `/blog/vibe-coding-landing/numerolog-za-60-min` | `article` | L3 | High | C05 |
| Pillar Deep Research | `/blog/deep-research-ai` | `pillar` | L2 | Medium | C04 |
| Pillar Упаковка | `/blog/upakovka-produkta-ai` | `pillar` | L2 | Medium | C05 |
| Pillar Контент | `/blog/content-za-chas` | `pillar` | L2 | Medium | C06 |
| Pillar AI-продажи | `/blog/ai-prodazi-telegram` | `pillar` | L2 | Medium | C07 |
| Pillar Автоматизация | `/blog/avtomatizatsiya-marketinga` | `pillar` | L2 | Low (волна 3) | C08 |
| Pillar Соло-эксперт | `/blog/soloeksperт-ai` | `pillar` | L2 | High | C09 |
| Pillar AI-стек 2026 | `/blog/ai-stack-2026` | `pillar` | L2 | High | C11 |
| Курс | `/kurs` | `kurs` | L1 | High | C12 |
| Vlad | `/vlad` | `about` | L1 | Medium | C12 |
| robots.txt | `/robots.txt` | route | — | Critical | — |
| llms.txt | `/llms.txt` | static | — | High | — |
| sitemap.xml | `/sitemap.xml` | route | — | Critical | — |
| OG image | `/opengraph-image` | route | — | Medium | — |

**Что MVP-волны 1 (деплой сегодня):**
- Главная `/`
- Блог-индекс `/blog`
- 1 готовая статья `/blog/ai-vs-marketolog` (полные 2500 слов по brief'у)
- Лендинг курса `/kurs`
- About `/vlad`
- robots.txt, llms.txt, sitemap.xml

Остальные 4 статьи добавляются волной 2 в следующие дни.

---

## Navigation Spec

### Header (4 элемента + CTA)

```
[ЛОГО AI-МАРКЕТОЛОГ]   Блог   О Vlad'е   |   [Залетай на курс] →
```

- **Лого** (левый край) → `/`
- **Блог** → `/blog`
- **О Vlad'е** → `/vlad`
- **CTA «Залетай на курс»** (правый край, жёлтая кнопка `#F5C518`, чёрный текст) → `/kurs`

Mobile: гамбургер + те же пункты + sticky CTA внизу экрана.

### Footer

3 колонки + копирайт:

| Контент | Vlad | Юр. |
|---|---|---|
| Все статьи (`/blog`) | О Vlad'е (`/vlad`) | Политика |
| Pillar Claude Code | Instagram | Условия |
| Pillar 3 AI-сотрудника | Threads | |
| Pillar Vibe Coding | Telegram | |
| Pillar AI vs Маркетолог | YouTube | |

Под футером: `© 2026 AI-Маркетолог 2.0 · Курс Vlad Yasko`

### Breadcrumbs (на всех страницах кроме главной)

Шаблон: `Главная > Блог > [Pillar] > [Spoke]`

Примеры:
- `Главная > Блог > Claude Code для маркетинга`
- `Главная > Блог > Vibe Coding > Нумеролог за 60 минут`
- `Главная > О Vlad'е`

Каждый сегмент кроме последнего — кликабельная ссылка. Реализуется как `Schema.org BreadcrumbList` + визуальная микро-навигация.

### Sidebar (только на pillar и article страницах)

Sticky `<aside>` слева на десктопе (≥1024px):
- **TOC** (table of contents) — авто-генерация из H2 заголовков
- **Прогресс-бар чтения** (тонкая полоска жёлтым `#F5C518` сверху TOC)
- **CTA-карточка** «Залетай на курс» в нижней части sidebar

Mobile: TOC сворачивается в выпадающий блок над контентом, CTA выезжает sticky bar снизу экрана.

---

## Internal Linking — Hub & Spoke План

Каждый pillar = hub, под ним 0-7 spoke-статей.

### Правила

1. **Spoke → Pillar** обязательно (в первом абзаце + в TOC + в breadcrumbs)
2. **Pillar → все Spoke** (через карточный список «Глубже по теме» в конце pillar)
3. **Spoke → Spoke** где тематически связано (через карточки «Связанные статьи» в конце)
4. **Pillar → Pillar** cross-link (минимум 2-3 на pillar, через текст в естественных местах)
5. **Любая статья → /kurs** через CTA-блок в конце (всегда) + 1-2 контекстных упоминания внутри текста
6. **Любая статья → /vlad** через мини-bio в конце с фото и одной строкой о Vlad'е

### Cross-cluster связи (примеры)

```
C01 Claude Code Marketing ↔ C02 3 AI-сотрудника
   (CC = инструмент, 3 сотрудника = использование инструмента)

C02 3 AI-сотрудника ↔ C07 AI-продажи Telegram
   (уровень 3 автономного агента = AI-продавец)

C03 Vibe Coding ↔ C05 Упаковка продукта
   (vibe coding это исполнение, упаковка это что упаковываем)

C03 Vibe Coding → /vibe-coding-landing/numerolog-za-60-min
   (pillar → spoke кейс)

C10 AI vs Маркетолог → C09 Соло-эксперт + C01 Claude Code
   (мотивация → план → инструмент)

C11 AI-стек 2026 → все остальные C01-C09
   (хаб инструментов → каждый кластер по теме инструмента)
```

### Карта приоритета внутренних ссылок

Самые ссылаемые страницы (получают больше всего входящих линков):

1. `/kurs` — каждая статья ссылается (CTA + 1-2 текстовых)
2. `/vlad` — каждая статья через author-bio
3. `/blog/claude-code-marketing` (C01) — базовый pillar, ссылаются почти все
4. `/blog/3-ai-sotrudnika` (C02) — следующий по важности
5. `/` — через лого

Pillar статьи получают по 3-5 входящих ссылок от spoke + cross-pillar.

### Карточки «Связанные статьи»

Внизу каждой статьи блок из 3 карточек (тёмный фон, жёлтые акценты):

```
┌──────────────┐ ┌──────────────┐ ┌──────────────┐
│ [Изображение]│ │ [Изображение]│ │ [Изображение]│
│ ТЕГ КЛАСТЕРА │ │ ТЕГ КЛАСТЕРА │ │ ТЕГ КЛАСТЕРА │
│ Заголовок    │ │ Заголовок    │ │ Заголовок    │
│ 2-3 строки   │ │ 2-3 строки   │ │ 2-3 строки   │
│ Читать →     │ │ Читать →     │ │ Читать →     │
└──────────────┘ └──────────────┘ └──────────────┘
```

Логика подбора (детерминированная, без ML):
1. Сначала 2 статьи из того же кластера (если есть)
2. Потом 1 cross-cluster по правилам выше
3. Если кластер пустой — 3 ближайших по приоритету

---

## Что НЕ делаем (out of scope MVP)

- ❌ Поиск по сайту (не нужен на 5-15 статях)
- ❌ Тэги (используем кластеры C01-C12 как тэги-аналоги)
- ❌ Комментарии (Vlad собирает аудиторию в Telegram, не на блоге)
- ❌ Подписка на email (без email-сервиса, без юридического сопровождения GDPR/152-ФЗ)
- ❌ Личный кабинет / авторизация
- ❌ Раздел кейсов отдельно (кейсы внутри статей)
- ❌ Раздел отзывов отдельно (отзывы на /kurs и /vlad)
- ❌ Категорийные индексы /blog/category/[slug] (используем pillar как индекс кластера)
- ❌ Дата в URL (`/blog/2026/05/post-title` ← плохо для SEO)
- ❌ Trailing slash variation (выбираем БЕЗ слеша, enforcer в next.config.js)
- ❌ Аналитика на старте (добавим GA4 + PostHog после деплоя отдельным шагом)

---

## Связанные файлы

- [templates.md](templates.md) — 5 шаблонов страниц (home, blog-index, pillar, article, kurs, about)
- [tech-geo-spec.md](tech-geo-spec.md) — robots, llms, sitemap, schema, hreflang
- [folder-structure.md](folder-structure.md) — структура Next.js App Router
- [../semantic-core/cluster_map.md](../semantic-core/cluster_map.md) — карта 12 кластеров
- [../semantic-core/content_briefs/](../semantic-core/content_briefs/) — 5 готовых briefs
- [../geo-audit-aimarketolog-site.md](../geo-audit-aimarketolog-site.md) — аудит сайта Vlad'а, ошибки которых избегаем
