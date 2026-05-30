# Tech/GEO Spec · AI-Маркетолог Blog

Полная техническая спецификация: что должно быть в коде на момент деплоя, чтобы блог цитировался AI и индексировался поисковиками.

**Особенно важно:** мы НЕ повторяем 8 ошибок сайта Vlad'а (см. `../geo-audit-aimarketolog-site.md`). Этот спек прямо перекрывает каждую ошибку.

---

## Rendering Strategy

**SSR (Server-Side Rendering)** через Next.js 14 App Router.

- **НЕ** SSG (статика): обновляем контент часто, инвалидация кэша лишний шаг
- **НЕ** CSR (клиент): AI-краулеры не выполняют JS
- **Да** SSR с `revalidate = 3600` (ISR-режим, час) для статей — балансирует свежесть и нагрузку

В каждом `page.tsx`:
```ts
export const dynamic = 'force-dynamic'  // на старте
// потом переводим на:
// export const revalidate = 3600
```

**Проверка после деплоя:** `curl https://blog/.../ | grep '<h1>'` должен вернуть текст. Если пусто = CSR-проблема, AI не увидит.

---

## robots.txt

Создаём через `app/robots.ts` (Next.js App Router) — генерируется автоматически на `/robots.txt`.

```ts
// app/robots.ts
import type { MetadataRoute } from 'next'

const BASE_URL = process.env.NEXT_PUBLIC_BASE_URL || 'https://ai-marketolog-blog.vercel.app'

export default function robots(): MetadataRoute.Robots {
  return {
    rules: [
      { userAgent: '*', allow: '/' },
      // OpenAI
      { userAgent: 'GPTBot', allow: '/' },
      { userAgent: 'ChatGPT-User', allow: '/' },
      { userAgent: 'OAI-SearchBot', allow: '/' },
      // Perplexity
      { userAgent: 'PerplexityBot', allow: '/' },
      { userAgent: 'Perplexity-User', allow: '/' },
      // Anthropic
      { userAgent: 'ClaudeBot', allow: '/' },
      { userAgent: 'anthropic-ai', allow: '/' },
      { userAgent: 'Claude-Web', allow: '/' },
      // Google
      { userAgent: 'Google-Extended', allow: '/' },
      { userAgent: 'Googlebot', allow: '/' },
      // Apple
      { userAgent: 'Applebot-Extended', allow: '/' },
      { userAgent: 'Applebot', allow: '/' },
      // Meta
      { userAgent: 'Meta-ExternalAgent', allow: '/' },
      { userAgent: 'FacebookExternalHit', allow: '/' },
      // Microsoft
      { userAgent: 'Bingbot', allow: '/' },
      // Cohere
      { userAgent: 'cohere-ai', allow: '/' },
      // DuckDuckGo
      { userAgent: 'DuckDuckBot', allow: '/' },
    ],
    sitemap: `${BASE_URL}/sitemap.xml`,
    host: BASE_URL,
  }
}
```

**Проверка после деплоя:** `curl https://blog/.../robots.txt` должен вернуть plain text. Если HTML или 404 = поправить.

---

## sitemap.xml

Через `app/sitemap.ts` — генерируется автоматически на `/sitemap.xml`.

```ts
// app/sitemap.ts
import type { MetadataRoute } from 'next'
import { getAllArticles, getAllPillars } from '@/lib/content'

const BASE_URL = process.env.NEXT_PUBLIC_BASE_URL || 'https://ai-marketolog-blog.vercel.app'

export default async function sitemap(): Promise<MetadataRoute.Sitemap> {
  const articles = await getAllArticles()

  const staticPages: MetadataRoute.Sitemap = [
    { url: BASE_URL, lastModified: new Date(), changeFrequency: 'weekly', priority: 1 },
    { url: `${BASE_URL}/blog`, lastModified: new Date(), changeFrequency: 'daily', priority: 0.9 },
    { url: `${BASE_URL}/kurs`, lastModified: new Date(), changeFrequency: 'weekly', priority: 0.95 },
    { url: `${BASE_URL}/vlad`, lastModified: new Date(), changeFrequency: 'monthly', priority: 0.8 },
  ]

  const articlePages: MetadataRoute.Sitemap = articles.map((a) => ({
    url: `${BASE_URL}/blog/${a.slug}`,
    lastModified: new Date(a.updatedAt || a.publishedAt),
    changeFrequency: a.pillar ? 'weekly' : 'monthly',
    priority: a.pillar ? 0.9 : 0.7,
  }))

  return [...staticPages, ...articlePages]
}
```

---

## llms.txt

Спецификация Answer.AI. Создаём в `public/llms.txt` как статический файл.

```markdown
# AI-Маркетолог 2.0 — блог под курс

> Блог про практический AI-маркетинг от Vlad Yasko, автора курса AI-Маркетолог 2.0. 12 тематических столпов, привязанных к 9 модулям курса.

## Основные страницы

- [Главная](https://blog/.../): обзор блога и всех тематических столпов
- [О Vlad'е](https://blog/.../vlad): автор курса, 14 лет в маркетинге, $120K кейс запуска
- [Курс AI-Маркетолог 2.0](https://blog/.../kurs): краткое описание, цены, ссылка на полную программу

## Тематические столпы (pillar pages)

- [Claude Code для маркетинга](https://blog/.../blog/claude-code-marketing): полный гид и маршрут переезда с ChatGPT за 30 дней
- [3 AI-сотрудника](https://blog/.../blog/3-ai-sotrudnika): ассистент → операционный помощник → автономный агент
- [Vibe Coding и лендинги](https://blog/.../blog/vibe-coding-landing): сайт без разработчика за 60 минут
- [AI vs профессия маркетолога](https://blog/.../blog/ai-vs-marketolog): как 80% задач уже автоматизируется
- [Deep Research для маркетолога](https://blog/.../blog/deep-research-ai): анализ конкурентов и аудитории через AI
- [Упаковка продукта через AI](https://blog/.../blog/upakovka-produkta-ai): ДНК + Лестница Ханта + Оффер Хормози
- [Контент за час](https://blog/.../blog/content-za-chas): 5 типов контента из одного чата
- [AI-продажи в Telegram](https://blog/.../blog/ai-prodazi-telegram): кейс AI-продавца $1500/день
- [Эксперт без команды](https://blog/.../blog/soloeksperт-ai): соло-маркетолог с AI-инфраструктурой
- [AI-стек 2026](https://blog/.../blog/ai-stack-2026): Claude, Gemini, Perplexity, NotebookLM, Telegram

## Об авторе

Vlad Yasko (Влад Ясько) — маркетолог с 14-летним опытом, преподаватель AI-маркетинга. Автор курса AI-Маркетолог 2.0 (9 недель, 8 модулей, 26+ AI-сотрудников в практике).

- Instagram: https://www.instagram.com/vlad_yasko/
- Threads: https://www.threads.com/@vlad_yasko
- Telegram: https://t.me/vlad_yasko_ai

## Контекст

Блог построен по принципам GEO 2026: Island Test (каждый абзац самодостаточен), перевёрнутая пирамида в H2 (прямой ответ в первых 100 словах), Schema.org разметка на каждой странице, мультимодальность (текст + изображения + диаграммы), Entity Knowledge Graph через Person markup автора.
```

---

## JSON-LD Schema

### Базовый набор для каждой страницы

Все страницы получают:
- `Organization` (один раз, через layout)
- `Person` (Vlad, один раз через layout с `@id` для ссылок)
- `BreadcrumbList` (на каждой странице кроме главной)

### По типам страниц

**`home`:**
```json
[
  {"@type": "WebSite", "name": "AI-Маркетолог 2.0", "url": "...", "potentialAction": {...}},
  {"@type": "Organization", "@id": "...#org", "name": "AI-Маркетолог 2.0", "founder": {"@id": "...#vlad"}},
  {"@type": "Person", "@id": "...#vlad", "name": "Vladyslav Yasko", "sameAs": [...]}
]
```

**`pillar` / `article`:**
```json
[
  {
    "@type": "Article",
    "headline": "...",
    "description": "...",
    "author": {"@id": "https://blog/.../#vlad"},
    "publisher": {"@id": "https://blog/.../#org"},
    "datePublished": "2026-05-30",
    "dateModified": "2026-05-30",
    "image": "...",
    "mainEntityOfPage": "...",
    "inLanguage": "ru-RU",
    "wordCount": 2500,
    "articleSection": "C01 Claude Code"
  },
  {"@type": "BreadcrumbList", "itemListElement": [...]},
  {"@type": "FAQPage", "mainEntity": [{"@type": "Question", "name": "...", "acceptedAnswer": {"@type": "Answer", "text": "..."}}]}
]
```

Если статья пошаговая (например, «Нумеролог за 60 минут»):
```json
{
  "@type": "HowTo",
  "name": "Запуск AI-нумеролога за 60 минут",
  "totalTime": "PT60M",
  "step": [
    {"@type": "HowToStep", "name": "Минута 0-10: формулировка оффера", "text": "..."},
    ...
  ]
}
```

**`kurs`:**
```json
[
  {
    "@type": "Course",
    "name": "AI-Маркетолог 2.0",
    "description": "Практический воркшоп: собственная ИИ-команда за 9 недель",
    "provider": {"@id": "...#org"},
    "instructor": {"@id": "...#vlad"},
    "courseMode": "online",
    "inLanguage": "ru",
    "startDate": "2026-04-18",
    "timeRequired": "PT9W",
    "offers": [
      {"@type": "Offer", "price": "397", "priceCurrency": "USD", "name": "Базовый"},
      {"@type": "Offer", "price": "1000", "priceCurrency": "USD", "name": "Стандарт"},
      {"@type": "Offer", "price": "3408", "priceCurrency": "USD", "name": "VIP"}
    ],
    "url": "https://aimarketolog.site"
  },
  {"@type": "FAQPage", ...},
  {"@type": "BreadcrumbList", ...}
]
```

**`about` (/vlad):**
```json
{
  "@type": "Person",
  "@id": "https://blog/.../#vlad",
  "name": "Vladyslav Yasko",
  "alternateName": ["Vlad Yasko", "Влад Ясько"],
  "jobTitle": "Маркетолог, преподаватель AI-маркетинга",
  "description": "14 лет в маркетинге, автор курса AI-Маркетолог 2.0, кейс запуска $120K",
  "url": "https://blog/.../vlad",
  "image": "https://blog/.../images/vlad.jpg",
  "sameAs": [
    "https://www.instagram.com/vlad_yasko/",
    "https://www.threads.com/@vlad_yasko",
    "https://t.me/vlad_yasko_ai",
    "https://aimarketolog.site"
  ],
  "knowsAbout": [
    "AI marketing",
    "Claude Code",
    "Deep Research",
    "Marketing automation",
    "Vibe coding",
    "Product packaging"
  ],
  "worksFor": {"@id": "https://blog/.../#org"}
}
```

Реализация в `lib/schema.ts` как функции-генераторы:
```ts
export function getPersonSchema() { ... }
export function getOrgSchema() { ... }
export function getArticleSchema(article) { ... }
export function getFaqSchema(faq) { ... }
export function getBreadcrumbSchema(items) { ... }
export function getCourseSchema(course) { ... }
export function getHowToSchema(steps) { ... }
```

Вставка в JSX:
```tsx
<script type="application/ld+json" dangerouslySetInnerHTML={{
  __html: JSON.stringify([article, breadcrumbs, faq])
}}/>
```

---

## Metadata (next.js)

Используем Next.js Metadata API в каждом `page.tsx`:

```tsx
// app/blog/[slug]/page.tsx
export async function generateMetadata({ params }): Promise<Metadata> {
  const article = await getArticleBySlug(params.slug)
  return {
    title: `${article.title} · AI-Маркетолог`,
    description: article.description,
    authors: [{ name: 'Vlad Yasko', url: `${BASE_URL}/vlad` }],
    keywords: article.keywords,
    openGraph: {
      title: article.title,
      description: article.description,
      url: `${BASE_URL}/blog/${article.slug}`,
      siteName: 'AI-Маркетолог 2.0',
      locale: 'ru_RU',
      type: 'article',
      publishedTime: article.publishedAt,
      modifiedTime: article.updatedAt,
      authors: ['Vlad Yasko'],
      images: [{ url: `${BASE_URL}/blog/${article.slug}/opengraph-image`, width: 1200, height: 630 }],
    },
    twitter: {
      card: 'summary_large_image',
      title: article.title,
      description: article.description,
      images: [`${BASE_URL}/blog/${article.slug}/opengraph-image`],
    },
    alternates: {
      canonical: `${BASE_URL}/blog/${article.slug}`,
      languages: {
        'ru-RU': `${BASE_URL}/blog/${article.slug}`,
        // готовим hreflang под будущие переводы:
        // 'uk-UA': `${BASE_URL}/uk/blog/${article.slug}`,
        // 'en-US': `${BASE_URL}/en/blog/${article.slug}`,
      },
    },
    robots: {
      index: true,
      follow: true,
      googleBot: { index: true, follow: true, 'max-image-preview': 'large', 'max-snippet': -1 },
    },
  }
}
```

**Критично:** `robots: { index: true, follow: true }` — явно, чтобы случайные `noindex` не появились. Не использовать `notFound()` где не нужно (это была ошибка сайта Vlad'а).

---

## OG Images (динамические)

`app/opengraph-image.tsx` (для главной) + `app/blog/[slug]/opengraph-image.tsx` (для статей).

Используем `ImageResponse` из `next/og`:
- Чёрный фон `#000000`
- Жёлтый акцент `#F5C518` для заголовка или линии
- Заголовок статьи 60-80pt
- Подпись «AI-Маркетолог · Vlad Yasko» 24pt
- 1200×630px

---

## Hreflang под будущие переводы

В layout добавляем:
```tsx
<link rel="alternate" hrefLang="ru-RU" href={`${BASE_URL}/blog/${slug}`} />
{/* потом раскомментируем */}
{/* <link rel="alternate" hrefLang="uk-UA" href={`${BASE_URL}/uk/blog/${slug}`} /> */}
{/* <link rel="alternate" hrefLang="en-US" href={`${BASE_URL}/en/blog/${slug}`} /> */}
<link rel="alternate" hrefLang="x-default" href={`${BASE_URL}/blog/${slug}`} />
```

И в `next.config.js` готовим i18n или сегменты `/uk`, `/en` под будущие переводы.

---

## Performance budget

- LCP < 2.5s
- CLS < 0.1
- TBT < 200ms
- HTML страницы < 200KB gzipped (важно для AI-краулеров — они часто timeout на больших страницах)
- Использовать `next/font` для шрифтов (Unbounded + IBM Plex Sans + JetBrains Mono)
- Использовать `next/image` для всех картинок с `priority` на hero
- Lazy-load для всего ниже first viewport

---

## Что проверяем после деплоя (чек-лист)

```bash
# 1. SSR работает?
curl https://blog/.../ | grep -c '<h1>'         # >= 1
curl https://blog/.../blog/ai-vs-marketolog | grep -c '<h1>'   # >= 1

# 2. Нет noindex?
curl -s https://blog/.../ | grep -c 'noindex'   # = 0

# 3. robots.txt отдаётся text?
curl -I https://blog/.../robots.txt | grep -i content-type
# должен быть text/plain

# 4. llms.txt отдаётся?
curl -I https://blog/.../llms.txt | grep -i 200

# 5. sitemap.xml отдаётся xml?
curl -I https://blog/.../sitemap.xml | grep -i content-type
# должен быть application/xml

# 6. JSON-LD на странице?
curl -s https://blog/.../blog/ai-vs-marketolog | grep -c 'application/ld+json'   # >= 1

# 7. Schema валидна?
# открыть https://search.google.com/test/rich-results и вставить URL

# 8. Lighthouse?
# Performance 90+, SEO 100, Best Practices 90+
```

---

## После деплоя (отдельно)

1. Submit sitemap в Google Search Console + Bing Webmaster Tools
2. Подключить `geo-perplexity-research` для мониторинга цитирования (после пополнения $50 на API)
3. Включить мониторинг 8 GEO-метрик (Citation Frequency, SOMV, Answer Inclusion, Entity Recognition, Sentiment, Prompt Coverage, Retrieval Success, Conversion Influence)
4. Через 2-4 недели первая проверка цитирования в Perplexity
5. Через 8 недель полный GEO-аудит и пересмотр приоритетов
