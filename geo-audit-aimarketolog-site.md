# GEO-аудит: new.aimarketolog.site

**Дата:** 2026-05-30
**URL:** https://new.aimarketolog.site/
**Запрос для теста:** «лучший курс по AI Маркетингу в постсоветском пространстве»
**Результат теста:** сайт НЕ выдан ChatGPT

---

## TL;DR

Сайт **не цитируется AI и плохо индексируется** по 8 техническим причинам. Контент сильный (есть FAQ, Person, цены, кейсы, 379 KB HTML, SSR работает), но техническая обёртка убивает всю работу.

**Главная причина:** в HTML стоит `<meta name="robots" content="noindex"/>` (первая строка), которая запрещает индексацию. Ниже идёт второй тэг `<meta name="robots" content="index, follow"/>`, но поисковики применяют ПЕРВОЕ и САМОЕ ОГРАНИЧИВАЮЩЕЕ правило. Сайт фактически закрыт от индексации.

---

## 8 критических ошибок (приоритет fix)

### 1. NOINDEX в meta robots ★★★★★ (block all)

```html
<meta name="robots" content="noindex"/>  ← это первое, поисковик применяет
...
<meta name="robots" content="index, follow"/>  ← это второе, игнорируется
```

**Что происходит:** Next.js рендерит `notFound()` компонент в дереве и MetadataBoundary одновременно. Сначала пишется noindex (от 404), потом нормальный мета-тег. Поисковик читает сверху вниз и применяет noindex.

**Доказательство:** заголовок страницы дублируется — первым идёт `<title>404: This page could not be found.</title>`, потом нормальный title. AI-краулер думает что попал на 404 страницу.

**Fix:** убрать рендеринг notFound() на главной. В Next.js App Router скорее всего где-то в layout или middleware вызывается notFound() ошибочно. Файл `app/not-found.tsx` рендерится только для 404, не должен попадать в HEAD главной.

### 2. robots.txt НЕТ ★★★★★

GET `/robots.txt` → возвращает HTML страницу 404, а не plain text. Это значит:
- AI-боты (GPTBot, PerplexityBot, ClaudeBot) видят непонятный HTML
- Не могут парсить правила доступа
- Часть ботов трактует как «доступ запрещён»

**Fix:** создать реальный `/robots.txt` (в Next.js это `app/robots.ts` или `public/robots.txt`):

```
User-agent: *
Allow: /

User-agent: GPTBot
Allow: /

User-agent: ChatGPT-User
Allow: /

User-agent: OAI-SearchBot
Allow: /

User-agent: PerplexityBot
Allow: /

User-agent: Perplexity-User
Allow: /

User-agent: ClaudeBot
Allow: /

User-agent: anthropic-ai
Allow: /

User-agent: Google-Extended
Allow: /

User-agent: Applebot-Extended
Allow: /

User-agent: Meta-ExternalAgent
Allow: /

User-agent: Bingbot
Allow: /

Sitemap: https://new.aimarketolog.site/sitemap.xml
```

### 3. sitemap.xml НЕТ ★★★★★

GET `/sitemap.xml` → 404. Без sitemap поисковики не знают какие страницы есть и как часто обновляются.

**Fix:** `app/sitemap.ts` в Next.js — генерируется автоматически.

### 4. llms.txt НЕТ ★★★★

Спецификация Answer.AI (Jeremy Howard) для AI-агентов. Не критично, но рекомендовано: даёт AI чистую структурированную сводку контента сайта.

**Fix:** `public/llms.txt`:

```
# AI-Маркетолог

> Практический воркшоп Vlad Yasko: 9 недель, собственная ИИ-команда из 26+ AI-ассистентов

## Курс

- [Программа AI-Маркетолог 2.0](https://new.aimarketolog.site/): 9 недель, 8 модулей, старт 18.04.2026
- Стек: Claude Code, Deep Research, NotebookLM, Vercel
- Тарифы: $397-$3408
- Гарантия: 14 дней, 100% возврат
- Автор: Vlad Yasko, 14 лет в маркетинге, кейс запуска $120К
```

### 5. Schema.org / JSON-LD = 0 ★★★★★

В HTML НИ ОДНОЙ structured data разметки. AI-краулеры извлекают факты из неструктурированного текста — ошибаются с цифрами, ценами, датами.

**Что должно быть:**

```json
{
  "@context": "https://schema.org",
  "@type": "Course",
  "name": "AI-Маркетолог 2.0",
  "description": "Практический воркшоп: собственная ИИ-команда за 9 недель",
  "provider": {
    "@type": "Organization",
    "name": "AI-Маркетолог",
    "url": "https://new.aimarketolog.site"
  },
  "instructor": {
    "@type": "Person",
    "name": "Vlad Yasko",
    "alternateName": "Влад Ясько",
    "url": "https://t.me/vlad_yasko_ai",
    "sameAs": [
      "https://www.instagram.com/vlad_yasko/",
      "https://www.threads.com/@vlad_yasko"
    ],
    "jobTitle": "Маркетолог, преподаватель AI-маркетинга",
    "description": "14 лет в маркетинге, кейс запуска $120K"
  },
  "courseMode": "online",
  "educationalLevel": "intermediate",
  "inLanguage": "ru",
  "startDate": "2026-04-18",
  "timeRequired": "PT9W",
  "offers": [
    {"@type": "Offer", "price": "397", "priceCurrency": "USD", "name": "Базовый"},
    {"@type": "Offer", "price": "3408", "priceCurrency": "USD", "name": "VIP"}
  ],
  "hasCourseInstance": {
    "@type": "CourseInstance",
    "courseMode": "online",
    "courseWorkload": "PT9W"
  }
}
```

Плюс отдельные блоки `FAQPage`, `BreadcrumbList`, `Organization`, `AggregateRating` (если есть отзывы с рейтингом).

### 6. Subdomain `new.` снижает авторитет ★★★

`new.aimarketolog.site` — это поддомен, поисковики и AI трактуют его как **отдельный сайт без истории**. Авторитет основного домена не наследуется.

**Fix:** решить — переехать на основной `aimarketolog.site` с 301 редиректом, или принять что это временный лендинг.

### 7. Дубль title и FOUC через notModule ★★★

`<script src="..." noModule=""></script>` — для старых браузеров. AI-краулер видит его и может трактовать как fallback контент.

И дублируется `<title>` (404 + основной). Поисковик берёт первый.

### 8. Социальные ссылки автора недостаточно прописаны ★★

`rel="author"` указывает на Telegram. Хорошо. Но `sameAs` в Schema нет (см. fix #5). Entity Knowledge Graph не строится — AI не связывает «Vlad Yasko» с его соцсетями и Instagram.

---

## Что у сайта ХОРОШО

| Параметр | Статус | Заметка |
|---|---|---|
| SSR работает | ✓ | HTML 379KB, текст в исходнике, не CSR-only |
| H1 + H2 на месте | ✓ | 13 H2 заголовков |
| Meta description | ✓ | Качественная, 200 символов |
| Open Graph + Twitter cards | ✓ | Полный набор |
| Author tag | ✓ | Указан Влад Ясько |
| Canonical | ✓ | Указан |
| FAQ-блок | ✓ | 8 вопросов (но без Schema FAQPage) |
| Person с квалификацией | ✓ | 14 лет, $120K кейс |
| Конкретные цифры | ✓ | 2660 учеников, 12+ ниш, $120K, цены |
| Программа и даты | ✓ | 9 недель, старт 18.04.26, 4 тарифа |
| Отзывы | ✓ | 4 цитаты из чата |
| Keywords meta | ✓ (хотя устарел) | Правильные термины |
| Стек: Next.js | ✓ | Современный SSR |

---

## Почему ChatGPT НЕ выдал сайт на запрос «лучший курс по AI-Маркетингу в постсоветском пространстве»

**5 причин в порядке важности:**

### 1. NOINDEX блокирует индексацию

Без индексации Bing → нет в ChatGPT Search (ChatGPT использует Bing). Без индексации Google → нет в Gemini / AI Overviews. Это первопричина.

### 2. Сайт молодой, без бэклинков и истории

Топ-результаты ChatGPT по этому запросу = **Skillbox / Нетология / Synergy / Practicum / MAED**. Все они:
- Существуют 5-15 лет
- Имеют тысячи бэклинков с авторитетных доменов
- Имеют сотни статей вокруг темы
- Цитируются в Wikipedia, Reddit, vc.ru
- Имеют брендовые поисковые запросы с 10К+ показами

Молодой лендинг на поддомене за день не может их обойти. Это **войны авторитета**, которые выигрываются через PR, цитаты в СМИ, гостевые посты, ответы на Habr/vc.ru/Reddit.

### 3. Subdomain без brand-history

`new.aimarketolog.site` — для AI это **новый домен**. Авторитет домена строится 3-12 месяцев минимум.

### 4. Нет Schema.org → AI не извлекает факты

ChatGPT при формулировке ответа «лучший курс» извлекает: имя курса, преподавателя, длительность, цену, аудиторию. Без Schema приходится парсить текст — медленнее, с ошибками, AI пропускает.

### 5. Запрос «лучший курс» это **comparison intent**

ChatGPT отвечает на такие запросы списком 3-5 курсов. Чтобы попасть в этот список:
- Либо собственный сайт с авторитетом (топ-результаты Google по запросу) — не наш случай
- Либо упоминание в **подборках на vc.ru/Habr/sostav.ru** типа «топ-10 курсов по AI-маркетингу»  — лучший рычаг для молодого бренда

---

## Что делать прямо сейчас (приоритет fix)

**Сегодня (1-2 часа работы):**
1. Убрать `noindex` мета-тег — найти и починить рендер notFound() на главной
2. Создать `/robots.txt` (5 минут, файл выше)
3. Создать `/sitemap.xml` (5 минут, через `app/sitemap.ts`)
4. Создать `/llms.txt` (10 минут)
5. Добавить JSON-LD `Course` + `Person` + `Organization` + `FAQPage` в head главной (30-60 минут)

**На неделе:**
6. Решить с переездом на `aimarketolog.site` (без `new.`)
7. Подключить Google Search Console + Bing Webmaster Tools
8. Подать sitemap в GSC
9. Запросить URL inspection после fix
10. Подключить аналитику для мониторинга

**На месяц:**
11. Написать 1-2 публикации на vc.ru/Habr (через свой блог → перепост)
12. Запросить упоминание у тематических подборок («Топ курсов по AI 2026»)
13. Подключить мониторинг цитирования (через `geo-perplexity-research` или Otterly $25/мес)

---

## Прогноз после fix

| Срок | Ожидаемый результат |
|---|---|
| 1-7 дней | Сайт начнёт индексироваться Google/Bing |
| 2-4 недели | Первые цитирования в Perplexity (быстрее всех) |
| 4-8 недель | Цитирования в ChatGPT Search |
| 2-3 месяца | Появление в Gemini AI Overviews по узким запросам |
| 6-12 месяцев | Возможность бороться за «лучший курс по AI-маркетингу» при наличии бэклинков из vc.ru/Habr |

**Без фикса:** сайт не появится в AI вообще. Никогда.

---

## Что наш блог (который мы делаем сегодня) даст Vlad'у

Помимо собственного органического трафика, **наш блог = решение проблемы #5** выше:
- Статья «Лучшие курсы по AI-маркетингу на русском 2026» от нашего блога с упоминанием курса Vlad'а = шанс попасть в выборку ChatGPT
- Pillar `/blog/ai-vs-marketolog` с упоминанием курса = TOFU magnet
- Все 5 наших пилотных статей ведут CTA на курс Vlad'а

Наш блог = **внешний контент-якорь и бэклинк-фабрика** для основного лендинга.

---

## Файл подготовлен для передачи Vlad'у

Можно скинуть Vlad'у целиком, либо вытащить пункты 1-5 как «срочный fix-лист» и отдать его технарю/себе на 1-2 часа работы.
