# AI-Маркетолог · Handoff Package для Vlad Yasko

Пакет результатов работы над GEO-стратегией и блогом под курс **AI-Маркетолог**. Подготовлен Anna (Anettiya), 30 мая 2026.

**Что внутри:** семантическое ядро, content briefs, архитектура блога, GEO-аудит твоего сайта, стратегия доминирования в AI-поиске, готовый каркас Next.js блога.

**Живой блог:** https://ai-marketolog-blog.vercel.app
**Репо блога:** https://github.com/anettimer-create/ai-marketolog-blog

---

## С чего начать (если открываешь впервые)

**Если ты Vlad** и просто хочешь понять что тут — читай в порядке:

1. [00-START-HERE.md](./00-START-HERE.md) ← коротко что сделано и куда смотреть
2. [geo-audit-aimarketolog-site.md](./geo-audit-aimarketolog-site.md) ← **СРОЧНО:** твой сайт сейчас не виден AI. Здесь fix-лист на 1-2 часа
3. [strategy-geo-domination.md](./strategy-geo-domination.md) ← план как дойти до «ChatGPT в прямом эфире цитирует Vlad»

**Если ты Claude Code** и тебя запустили в этом репо — читай [CLAUDE.md](./CLAUDE.md), там bootstrap-инструкция.

---

## Структура

```
ai-marketolog-handoff/
├── README.md                           ← вы здесь
├── 00-START-HERE.md                    карта документа для Vlad'а
├── CLAUDE.md                           bootstrap для Claude Code
│
├── CONTEXT.md                          кто такой Vlad, статус совместного проекта
├── STRATEGY.md                         план совместного продукта Anna + Vlad
├── QUESTIONS-TO-VLAD.md                24 вопроса на 3 встречи
├── RESEARCH-2026.md                    опорная база GEO 2026 (4 deep research'а)
│
├── geo-audit-aimarketolog-site.md      ★ КРИТИЧНО: аудит сайта Vlad'а
├── strategy-geo-domination.md          стратегия 9-12 месяцев
├── vlad-geo-realtor-package-REFERENCE.md  заметки про репо Vlad'а
│
├── semantic-core/                      164 запроса, 12 кластеров
│   ├── analysis.md                     главный отчёт по семантике
│   ├── clusters.json                   12 тематических столпов
│   ├── cluster_map.md                  Hub & Spoke карта
│   ├── semantic_core.csv               полный CSV для Excel/Sheets
│   ├── input.json                      параметры прогона
│   ├── raw/all_queries_normalized.json
│   └── content_briefs/                 5 готовых детальных брифов под статьи
│       ├── 01_pillar_claude-code-marketing.md
│       ├── 02_pillar_3-ai-sotrudnika.md
│       ├── 03_pillar_vibe-coding-landing.md
│       ├── 04_pillar_ai-vs-marketolog.md
│       └── 05_spoke_numerolog-za-60-min.md
│
└── architecture/                       архитектура блога
    ├── README.md                       page hierarchy, navigation, internal linking
    ├── templates.md                    6 шаблонов страниц
    ├── tech-geo-spec.md                robots.txt, llms.txt, JSON-LD, hreflang
    └── folder-structure.md             Next.js folder structure
```

---

## Краткая сводка результатов

**Семантика:** 164 запроса в 12 тематических кластерах. 5 пилотных статей задеплоены/в работе. Полный отчёт со стратегией приоритизации в `semantic-core/analysis.md`.

**Архитектура:** Next.js 16 App Router + Tailwind v4 + Vercel. Hub & Spoke, 12 pillar-страниц под кластеры, MDX-content. JSON-LD на каждой странице (Article + Person + Organization + FAQPage + BreadcrumbList).

**GEO 2026:** Island Test + перевёрнутая пирамида + 134-167 слов на пассаж + мультимодальность. Полная база в `RESEARCH-2026.md`.

**Дифференциация vs конкурентов:** Евгения Одуд (прямой), Яндекс Практикум, Нетология, Skillbox, Setters, MAED, ВШЭ. Подробно в `strategy-geo-domination.md`.

---

## Лицензия

MIT — можешь использовать, копировать, адаптировать. Я (Anna) автор работы по аналитике и архитектуре, рада если поможет.

## Контакт

Anna · Anettiya · [Instagram @anettiya](https://www.instagram.com/anettiya/)
