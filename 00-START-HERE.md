# 00 · Привет Vlad, с чего начать

Это краткая навигация по пакету. Если торопишься — читай только этот файл.

## За 5 минут — что Anna сделала

1. **Прогнала твой сайт через GEO-аудит** → нашла 8 ошибок которые не дают AI его цитировать. Главная — `noindex` мета-тег в HTML. Без фикса сайт не появится ни в ChatGPT, ни в Perplexity, ни в Gemini.
   → [geo-audit-aimarketolog-site.md](./geo-audit-aimarketolog-site.md)
   → **Срочно отдай разработчику или сделай сам по чек-листу — это 1-2 часа работы**

2. **Собрала семантическое ядро** 164 запроса, разбила на 12 тематических кластеров по твоим 9 модулям курса. Это план статей на 6-12 месяцев.
   → [semantic-core/analysis.md](./semantic-core/analysis.md)

3. **Написала 5 готовых детальных брифов** под пилотные статьи. Каждый бриф = почти outline статьи с H2-структурой, GEO-вопросами под Perplexity, схема markup, ToV-маркерами.
   → [semantic-core/content_briefs/](./semantic-core/content_briefs/)

4. **Спроектировала и собрала блог** под твой курс — Next.js + Vercel, GEO-оптимизированный по правилам 2026.
   → Живой: https://ai-marketolog-blog.vercel.app
   → Код: https://github.com/anettimer-create/ai-marketolog-blog
   → Архитектура: [architecture/README.md](./architecture/README.md)

5. **Расписала стратегию** как за 9-12 месяцев добиться чтобы ChatGPT цитировал тебя как эксперта по AI-маркетингу на русском.
   → [strategy-geo-domination.md](./strategy-geo-domination.md)

---

## За 30 минут — что делать тебе сейчас

**Шаг 1 (1-2 часа):** Открой `geo-audit-aimarketolog-site.md`. Это диагноз твоего сайта new.aimarketolog.site с готовыми сниппетами кода для fix. Передай разработчику или сделай сам:
- Убрать ошибочный `noindex` мета-тег на главной
- Создать `/robots.txt`, `/sitemap.xml`, `/llms.txt`
- Добавить JSON-LD Schema (Course + Person + Organization + FAQPage)

Без этих 5 правок весь остальной GEO-труд не приносит результата.

**Шаг 2 (опционально):** Открой `strategy-geo-domination.md` и посмотри 3-горизонтный план. Решишь хочешь ли двигаться по нему.

**Шаг 3 (опционально):** Развернуть блог у себя если хочешь сам редактировать:
```bash
git clone https://github.com/anettimer-create/ai-marketolog-blog.git
cd ai-marketolog-blog
npm install
npm run dev
```
Открыть в Claude Code: `claude .` в папке репо. Claude увидит контекст и сможет продолжить работу.

---

## Что в работе у Anna дальше

Если ты согласишься на стратегию, Anna может:
- Дописать остальные 4 pillar-статьи по уже готовым брифам (волна 2)
- Сделать перепосты на vc.ru/Habr/Reddit с upmention твоего курса (бэклинки)
- Подключить мониторинг цитирования через Perplexity API ($50 на год)
- Через 3-6 месяцев — гостевые посты, подкаст-выступления
- Через 6-12 месяцев — Wikipedia-страница

Формат сотрудничества обсуждается ([STRATEGY.md](./STRATEGY.md), [QUESTIONS-TO-VLAD.md](./QUESTIONS-TO-VLAD.md)).

---

## Что Anna уже знает про тебя

Из открытых источников: Instagram, Threads, YouTube. Изучила транскрипции 9 твоих видео — извлекла ToV-маркеры, кейсы с цифрами, реальные темы. Весь контент блога написан **твоим голосом** (через скил `stories-yasko` который уже есть у Anna).

Контекст подробно в [CONTEXT.md](./CONTEXT.md).

---

## Если что-то непонятно

Anna в Instagram @anettiya. Или попроси Claude Code в этом репо объяснить любой файл — он прочитает и переведёт на простой язык.
