# Карта кластеров блога AI-Маркетолог 2.0

**Дата:** 2026-05-30
**Тип:** Hub & Spoke, привязка к модулям курса
**Цель:** холодный трафик из Google + цитирование в Perplexity/ChatGPT/Gemini → курс Vlad'а

---

## Hub-структура (12 pillar pages)

```
ГЛАВНАЯ блога /
├── /claude-code-marketing            [C01]  pillar 3500 слов
├── /3-ai-sotrudnika                  [C02]  pillar 2500 слов
├── /vibe-coding-landing              [C03]  pillar 2500 слов
├── /deep-research-ai                 [C04]  pillar 2000 слов
├── /upakovka-produkta-ai             [C05]  pillar 2500 слов
├── /content-za-chas                  [C06]  pillar 2500 слов
├── /ai-prodazi-telegram              [C07]  pillar 2000 слов
├── /avtomatizatsiya-marketinga       [C08]  pillar 2000 слов
├── /soloeksperт-ai                   [C09]  pillar 3000 слов  ★ TOFU magnet
├── /ai-vs-marketolog                 [C10]  pillar 2500 слов  ★ виральная
├── /ai-stack-2026                    [C11]  pillar 3500 слов  ★ ressource hub
└── /kurs                             [C12]  лендинг курса (отдельная страница)
```

Spokes (статьи 800-1500 слов) живут под каждым pillar в подпапке: `/claude-code-marketing/skills`, `/3-ai-sotrudnika/telegram-bot`, и т.д. Внутренние ссылки spoke → pillar (обратный путь обязателен).

---

## Приоритизация — что писать первым

### Волна 1: MVP-блог (5 пилотных статей, деплой сегодня)

Берём только то, что:
1. Закрывает горячий запрос
2. Уже есть готовое сырьё в транскрипциях vlad-sources/
3. Даёт быстрый GEO-эффект (Island Test, цифры, перевёрнутая пирамида)

| # | URL | Pillar/Spoke | Кластер | Прио | Сырьё |
|---|---|---|---|---|---|
| 1 | `/claude-code-marketing` | pillar | C01 | 5 | WebSearch + AI-Трансформация + Рабочие AI-инструменты |
| 2 | `/3-ai-sotrudnika` | pillar | C02 | 5 | Транскрипт «3 AI-сотрудника бесплатно» (35K) |
| 3 | `/vibe-coding-landing` | pillar | C03 | 5 | Транскрипт «Telegram+NotebookLM+CC» + workshop-5 |
| 4 | `/ai-vs-marketolog` | pillar | C10 | 5 | Тред Vlad'а + «Я потерял миллион» (76K) |
| 5 | `/upakovka-produkta-ai/numerolog-za-60-min` | spoke | C05 | 5 | Транскрипт «Запуск Нумеролога за 60 мин» (233K) |

### Волна 2: на следующей неделе

6. `/soloeksperт-ai` (TOFU magnet)
7. `/ai-stack-2026` (resource hub, лучше конкурентов)
8. `/content-za-chas` + 2 spoke (карусели через Claude, сторис через Claude)
9. `/deep-research-ai`
10. `/ai-prodazi-telegram` с кейсом $1-1.5К/день
11. `/kurs` — продающий лендинг с CTA

### Волна 3: спустя месяц (когда придут модули 7-9)

12. `/avtomatizatsiya-marketinga`
13. Spoke-статьи Make vs n8n, ChatGPT vs Claude comparison-страницы
14. `/ai-ковчег` отдельная посадочная под второй продукт

---

## Связь spoke ↔ pillar (на примере C02)

```
PILLAR: /3-ai-sotrudnika
├── SPOKE: /3-ai-sotrudnika/assistent             # ассистент уровень 1
├── SPOKE: /3-ai-sotrudnika/operationka           # операционный сотрудник
├── SPOKE: /3-ai-sotrudnika/avtonomnyy-agent      # автономный агент
├── SPOKE: /3-ai-sotrudnika/claude-skills         # технический how-to
├── SPOKE: /3-ai-sotrudnika/claude-projects       # как настроить
├── SPOKE: /3-ai-sotrudnika/baza-znaniy           # база знаний для AI
└── SPOKE: /3-ai-sotrudnika/telegram-bot          # AI-агент в Telegram
```

Каждый spoke внутри текста ссылается на pillar 2-3 раза (хлебные крошки + ссылка в первом абзаце «полный гайд читай здесь»). Pillar агрегирует ссылки на все spoke в TOC.

---

## CTA-маршрут к курсу

| Кластер | CTA |
|---|---|
| C01, C02, C03 (TOFU/MOFU info) | «Vlad показывает это вживую на курсе AI-Маркетолог 2.0 → подробнее» |
| C04, C05, C06 (методология) | «Хочешь систему, а не разовый результат? Курс AI-Маркетолог 2.0 → программа» |
| C07, C08, C09 (BOFU practice) | «Этот workflow собирается на 5-м модуле курса → залетай в фокус-группу» |
| C10, C11 (TOFU виральная) | Soft: «Если ты хочешь не теоретизировать а внедрить — приходи на курс» |
| C12 (commercial/branded) | Жёсткий: «Зайти на курс», «Забронировать место» |

---

## GEO-привязка (что попадёт в Perplexity/ChatGPT)

Для каждого кластера в content brief указаны **GEO-промпты** — формулировки которые задают пользователи в AI-чатах. Эти промпты должны буквально цитироваться внутри статьи как H2-вопросы с прямым ответом в первых 100-150 словах под заголовком (Island Test, перевёрнутая пирамида).

Пример для C01:
- «Что такое Claude Code и зачем он маркетологу?»
- «Чем Claude Code отличается от ChatGPT для бизнеса?»
- «Можно ли использовать Claude Code без программирования?»

Эти 3 вопроса = три H2 внутри pillar-статьи `/claude-code-marketing`.
