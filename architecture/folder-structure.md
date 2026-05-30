# Folder Structure · Next.js 14 App Router

Минимальная структура без overengineering. То что НЕ нужно на MVP — не создаём (i18n-роутинг, тесты, Storybook, e2e, CI).

```
ai-marketolog-blog/
├── app/
│   ├── layout.tsx                   # Root layout: <html>, <body>, шрифты, базовые мета, Person/Org Schema
│   ├── page.tsx                     # / Главная
│   ├── opengraph-image.tsx          # OG для главной
│   ├── icon.tsx                     # favicon динамически
│   │
│   ├── robots.ts                    # /robots.txt с Allow для AI-ботов
│   ├── sitemap.ts                   # /sitemap.xml автогенерация
│   │
│   ├── blog/
│   │   ├── page.tsx                 # /blog индекс
│   │   ├── opengraph-image.tsx      # OG для индекса
│   │   └── [slug]/
│   │       ├── page.tsx             # /blog/[slug] pillar или spoke
│   │       ├── opengraph-image.tsx  # OG для статьи (динамическая по слагу)
│   │       └── [sub]/
│   │           ├── page.tsx         # /blog/[slug]/[sub] глубокий spoke
│   │           └── opengraph-image.tsx
│   │
│   ├── kurs/
│   │   ├── page.tsx                 # /kurs лендинг
│   │   └── opengraph-image.tsx
│   │
│   ├── vlad/
│   │   ├── page.tsx                 # /vlad about
│   │   └── opengraph-image.tsx
│   │
│   └── not-found.tsx                # 404 страница (ОТДЕЛЬНО, не попадает в main pages)
│
├── components/
│   ├── layout/
│   │   ├── Header.tsx
│   │   ├── Footer.tsx
│   │   └── Breadcrumbs.tsx
│   │
│   ├── home/
│   │   ├── Hero.tsx
│   │   ├── HeroMetrics.tsx
│   │   ├── PillarGrid.tsx
│   │   ├── LatestArticles.tsx
│   │   └── VladCard.tsx
│   │
│   ├── blog/
│   │   ├── ArticleHero.tsx
│   │   ├── TldrBlock.tsx
│   │   ├── Sidebar.tsx
│   │   ├── Toc.tsx
│   │   ├── ReadingProgress.tsx
│   │   ├── AuthorBio.tsx
│   │   ├── SpokeList.tsx
│   │   ├── RelatedArticles.tsx
│   │   ├── ArticleCard.tsx
│   │   ├── ArticlesGrid.tsx
│   │   ├── ClusterFilter.tsx
│   │   └── ClusterTag.tsx
│   │
│   ├── mdx/                         # компоненты для использования внутри .mdx
│   │   ├── PromptBlock.tsx          # синий блок промпта Claude
│   │   ├── ToolMention.tsx          # inline-чип инструмента
│   │   ├── CaseCard.tsx             # карточка кейса с большой жёлтой цифрой
│   │   ├── ComparisonTable.tsx
│   │   ├── Quote.tsx                # цитата Vlad'а
│   │   ├── Stat.tsx                 # выделенная статистика
│   │   ├── Callout.tsx              # warning/tip/insight
│   │   └── index.ts                 # экспорт всех для MDX provider
│   │
│   ├── kurs/
│   │   ├── KursHero.tsx
│   │   ├── ModuleGrid.tsx
│   │   ├── WhoFor.tsx
│   │   ├── PricingPreview.tsx
│   │   └── FinalCta.tsx
│   │
│   ├── vlad/
│   │   ├── VladHero.tsx
│   │   ├── Bio.tsx
│   │   ├── Timeline.tsx
│   │   ├── SocialLinks.tsx
│   │   └── ArticlesByVlad.tsx
│   │
│   └── shared/
│       ├── CtaBanner.tsx            # большой CTA «Залетай на курс»
│       ├── FaqAccordion.tsx
│       ├── PageHero.tsx             # generic hero для /blog индекса
│       └── Button.tsx               # primary (жёлтый) / secondary / ghost
│
├── content/
│   ├── articles/                    # все статьи как .mdx
│   │   ├── ai-vs-marketolog.mdx                          # C10, MVP-волна 1
│   │   ├── claude-code-marketing.mdx                     # C01, волна 2
│   │   ├── 3-ai-sotrudnika.mdx                           # C02, волна 2
│   │   ├── vibe-coding-landing.mdx                       # C03, волна 2
│   │   └── vibe-coding-landing--numerolog-za-60-min.mdx  # spoke C05, волна 2
│   │
│   ├── kurs/
│   │   ├── modules.ts               # 9 модулей курса
│   │   ├── tarify.ts                # 4 тарифа
│   │   └── faq.ts                   # FAQ для /kurs
│   │
│   └── vlad/
│       ├── bio.mdx                  # bio Vlad'а
│       ├── timeline.ts              # вехи
│       └── faq.ts                   # FAQ для /vlad
│
├── lib/
│   ├── content.ts                   # getAllArticles, getArticleBySlug, getPillars
│   ├── clusters.ts                  # 12 кластеров (id, title, slug, description, icon)
│   ├── schema.ts                    # JSON-LD генераторы
│   ├── reading-time.ts              # подсчёт времени чтения
│   ├── mdx-options.ts               # rehype/remark плагины (slug, autolink, prism)
│   └── utils.ts                     # cn, slugify, formatDate
│
├── public/
│   ├── llms.txt                     # для AI-агентов
│   ├── images/
│   │   ├── vlad.jpg                 # портрет Vlad'а (для /vlad и author bio)
│   │   ├── og-default.png           # fallback OG
│   │   └── articles/                # картинки внутри статей
│   ├── icons/
│   │   ├── claude.svg
│   │   ├── nextjs.svg
│   │   ├── vercel.svg
│   │   └── telegram.svg
│   └── favicon.ico
│
├── styles/
│   └── globals.css                  # tailwind directives + CSS variables (--bg, --fg, --accent, --claude)
│
├── .env.local                       # NEXT_PUBLIC_BASE_URL, и т.д.
├── .gitignore
├── next.config.mjs
├── tailwind.config.ts               # кастомные цвета (черный, жёлтый, синий Claude), шрифты
├── tsconfig.json
├── package.json
└── README.md
```

---

## Зависимости (package.json)

**Production:**
- `next@^14.2` — фреймворк
- `react@^18` `react-dom@^18`
- `next-mdx-remote@^4` или `@next/mdx` — для MDX в content/
- `rehype-slug` — id для H2/H3 (TOC)
- `rehype-autolink-headings` — кликабельные # на H2
- `rehype-pretty-code` или `shiki` — подсветка кода в блоках промптов
- `remark-gfm` — таблицы и task lists
- `tailwindcss@^3.4` `autoprefixer` `postcss`
- `clsx` или `class-variance-authority` — для условных классов
- `date-fns` — даты

**Dev:**
- `typescript@^5`
- `@types/node` `@types/react` `@types/react-dom`
- `eslint` `eslint-config-next`

Никаких UI-библиотек (shadcn/radix) на MVP — пишем компоненты сами, экономим вес бандла. Если позже понадобится FAQ-аккордеон сложнее простого `<details>`, поставим `@radix-ui/react-accordion`.

---

## CSS-переменные (styles/globals.css)

```css
@tailwind base;
@tailwind components;
@tailwind utilities;

:root {
  --bg: #000000;
  --fg: #ffffff;
  --accent: #F5C518;          /* жёлтый Vlad */
  --claude: #9333ea;          /* синий Claude точечно */
  --muted: #1a1a1a;
  --muted-fg: #a3a3a3;
  --border: #262626;
}

html { background: var(--bg); color: var(--fg); }
body { font-family: var(--font-ibm-plex-sans), system-ui, sans-serif; }

h1, h2, h3 { font-family: var(--font-unbounded), serif; font-weight: 800; }
code, pre { font-family: var(--font-jetbrains-mono), monospace; }
```

Шрифты (`app/layout.tsx`):
```tsx
import { Unbounded, IBM_Plex_Sans, JetBrains_Mono } from 'next/font/google'

const unbounded = Unbounded({ subsets: ['latin', 'cyrillic'], variable: '--font-unbounded' })
const ibmPlex = IBM_Plex_Sans({ subsets: ['latin', 'cyrillic'], weight: ['400', '600'], variable: '--font-ibm-plex-sans' })
const jetbrains = JetBrains_Mono({ subsets: ['latin'], variable: '--font-jetbrains-mono' })
```

(Те же шрифты, что у Vlad'а — для визуальной семьи.)

---

## Tailwind config основное

```ts
// tailwind.config.ts
export default {
  content: ['./app/**/*.{ts,tsx}', './components/**/*.{ts,tsx}', './content/**/*.mdx'],
  theme: {
    extend: {
      colors: {
        bg: 'var(--bg)',
        fg: 'var(--fg)',
        accent: 'var(--accent)',
        claude: 'var(--claude)',
        muted: { DEFAULT: 'var(--muted)', fg: 'var(--muted-fg)' },
        border: 'var(--border)',
      },
      fontFamily: {
        display: ['var(--font-unbounded)'],
        sans: ['var(--font-ibm-plex-sans)'],
        mono: ['var(--font-jetbrains-mono)'],
      },
      typography: ({ theme }) => ({
        invert: {
          css: {
            '--tw-prose-body': theme('colors.fg'),
            '--tw-prose-headings': theme('colors.fg'),
            '--tw-prose-links': theme('colors.accent'),
            '--tw-prose-bold': theme('colors.fg'),
            '--tw-prose-quotes': theme('colors.muted.fg'),
            '--tw-prose-quote-borders': theme('colors.accent'),
            '--tw-prose-code': theme('colors.accent'),
            '--tw-prose-pre-bg': theme('colors.muted.DEFAULT'),
            '--tw-prose-th-borders': theme('colors.border'),
            '--tw-prose-td-borders': theme('colors.border'),
          },
        },
      }),
    },
  },
  plugins: [require('@tailwindcss/typography')],
}
```

---

## MDX frontmatter (контракт статьи)

```yaml
---
title: "AI и профессия маркетолога: что делать, если 80% задач уже автоматизируется"
description: "AI закрывает 80% задач маркетолога в 2026. Манифест Vlad Yasko с историей провала $1M и планом действий на 90 дней."
slug: "ai-vs-marketolog"
cluster: "C10"
pillar: true
parentPillar: null              # для spoke = slug pillar'а
publishedAt: "2026-05-30"
updatedAt: "2026-05-30"
readingTime: 12                 # минут
keywords:
  - "AI заменит маркетолога"
  - "профессия маркетолог будущее"
  - "AI vs маркетолог"
tldr: |
  AI в 2026 закрывает 80% повседневных задач маркетолога: анализ конкурентов, тексты, лендинги, чат-боты.
  Маркетологи без Claude Code теряют рынок и зарплаты в течение 12-18 месяцев. Vlad прошёл этот путь сам:
  потерял $1M в крипте, упал в депрессию на 487 дней, восстановился через AI. Статья — диагноз и план на 90 дней.
faq:
  - q: "AI заменит маркетологов полностью?"
    a: "Нет. AI закрывает исполнение, маркетолог остаётся в стратегии и репутации. Но соотношение меняется."
  - q: "Что делать маркетологу без технического бэкграунда?"
    a: "Перейти на Claude Code — он не требует программирования, команды на обычном русском."
  - q: "Стоит ли идти учиться маркетингу в 2026?"
    a: "Классическому маркетингу — нет. Live-курсам с практикой в AI-инструментах — да."
howTo: null                     # или объект если статья пошаговая
ogImageTitle: "AI vs маркетолог"  # для динамической OG
---

## H2 1. Что AI уже умеет лучше маркетолога в 2026

80% повседневных задач маркетолога это анализ, тексты, лендинги, переписка, отчёты. AI закрывает все 5 категорий уже сейчас, не в будущем...
```

---

## Команды разработки

```bash
# Создание
npx create-next-app@latest ai-marketolog-blog --typescript --tailwind --app --src-dir=false --import-alias='@/*'

# Установка зависимостей
npm install next-mdx-remote rehype-slug rehype-autolink-headings rehype-pretty-code remark-gfm date-fns clsx
npm install -D @tailwindcss/typography

# Разработка
npm run dev      # localhost:3000

# Сборка
npm run build
npm run start

# Деплой
npx vercel       # первый деплой, ответить на вопросы
npx vercel --prod  # promote to production
```

---

## Что добавляется потом (post-MVP)

| Когда | Что | Зачем |
|---|---|---|
| Неделя 2 | GA4 + PostHog | Аналитика |
| Неделя 2 | Plausible как опция | Privacy-friendly альтернатива |
| Неделя 2 | Sentry | Error tracking |
| Неделя 3 | i18n routing | Если запускаем uk/en |
| Неделя 3-4 | Email-форма «новые статьи» | Когда определимся с email-сервисом |
| Месяц 2 | Search (Algolia или MeiliSearch) | Когда статей > 20 |
| Месяц 2 | shadcn/ui | Если нужны сложные компоненты |
| Месяц 3 | CMS (Sanity или Contentlayer) | Когда команда копирайтеров > 1 |
