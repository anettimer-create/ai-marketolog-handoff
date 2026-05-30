# CLAUDE.md · Bootstrap для Claude Code в этом репо

Если ты Claude Code и тебя запустили в этой папке — прочитай этот файл первым.

## Что это за репо

Это **handoff-пакет** результатов работы Anna (Anettiya) над GEO-стратегией и блогом под курс AI-Маркетолог Vlad Yasko. Vlad может скачать его и продолжить работу с тобой как с помощником.

## Главные документы (читай по приоритету)

1. **[README.md](./README.md)** — структура всего пакета
2. **[00-START-HERE.md](./00-START-HERE.md)** — карта для Vlad'а
3. **[CONTEXT.md](./CONTEXT.md)** — кто Vlad, кто Anna, статус
4. **[STRATEGY.md](./STRATEGY.md)** — план совместного продукта
5. **[geo-audit-aimarketolog-site.md](./geo-audit-aimarketolog-site.md)** — критично, основа всей работы
6. **[strategy-geo-domination.md](./strategy-geo-domination.md)** — KPI и план 9-12 месяцев
7. **[semantic-core/analysis.md](./semantic-core/analysis.md)** — семядро и приоритизация
8. **[architecture/README.md](./architecture/README.md)** — архитектура блога

## Что Vlad может попросить тебя сделать

### Сценарий A — починить его сайт

Vlad: «прогони мой сайт new.aimarketolog.site через geo-audit заново после моих правок»
→ Используй `geo-audit` скилл если есть, или вручную проверь по чек-листу из `geo-audit-aimarketolog-site.md`

### Сценарий B — написать новую статью

Vlad: «напиши мне статью про [тема]»
→ Найди соответствующий brief в `semantic-core/content_briefs/`
→ Если есть — пиши по нему (с ToV Vlad'а из stories-yasko если стоит)
→ Если нет — сначала проверь cluster в `semantic-core/cluster_map.md`, потом пиши с нуля

### Сценарий C — мониторинг цитирования

Vlad: «проверь цитируют ли меня в Perplexity по запросу X»
→ Используй `geo-perplexity-research` скилл если есть
→ Требует PERPLEXITY_API_KEY в env

### Сценарий D — развернуть блог локально

Vlad: «как мне поднять этот блог у себя»
→ Клонируй https://github.com/anettimer-create/ai-marketolog-blog
→ `npm install` → `npm run dev` → localhost:3000
→ Деплой: связать с Vercel через GitHub-app (см. опыт Anna в коммитах блог-репо)

## Что важно знать

- **Vlad — реальный человек**, не клиент Anna. Они в стратегическом партнёрстве (Anna проходит у Vlad'а менторинг, Vlad может стать партнёром по продукту).
- **Не используй слова «гуру», «коуч»** в публичных текстах про Vlad'а — это против его позиционирования.
- **ToV Vlad'а специфический:** «свой пацан, который взломал систему». Маркеры: движ, залетай, машина, на коленке, гиперценность, стакать, завод в кармане. Подробно в `stories-yasko/references/tov_yasko.md` (если у Vlad'а стоит этот скил).
- **Без em-dash** в публичных текстах (триггерит как AI-tell).
- **Сайт Vlad'а:** `https://new.aimarketolog.site` (НЕ `aimarketolog.site` — это поддомен потому что основная версия в разработке).

## Что НЕ делать

- Не переписывать архитектуру блога без согласования с Vlad'ом — она спроектирована под GEO 2026 и менять её = терять преимущества
- Не использовать чистый CSR (без SSR) — AI-краулеры не выполняют JS
- Не убирать llms.txt / robots.txt — они критичны для AI-цитирования
- Не публиковать материалы с пометкой «личное» из vlad-sources (этих файлов нет в публичном пакете, но если Vlad сам загрузит — соблюдай конфиденциальность)

## Скиллы Anna которые опираются на метод Vlad'а

Если Vlad запустит Claude Code на своём компьютере и установит мои скиллы:
- `carousel-style-vlad` — визуальный стандарт каруселей Vlad'а
- `stories-yasko` — Tone of Voice + структура сторис
- `client-dna-v2` — ДНК клиента по методу Vlad'а
- `hormozi-offer` — упаковка офферов
- `hunt-ladder` — лестница Ханта
- `geo-audit` / `geo-perplexity-research` — GEO-инструменты

Эти скиллы — открытые, можно установить из marketplace.

---

**Начинай работу с того что прочитай README.md и спроси Vlad'а какой сценарий он хочет.**
