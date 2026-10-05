# n8n lead intake template

Шаблон n8n-воркфлоу: заявка с сайта или формы приходит на webhook, LLM определяет приоритет и пишет краткую выжимку, результат прилетает вам в Telegram.

Это **шаблон-заготовка**, а не готовый кейс: подстройте промпт и поля под свой бизнес.

## Как это работает

1. `Webhook: new lead` принимает POST на `/webhook/lead` с полями `name`, `phone`, `message`, `source`.
2. `Normalize input` приводит данные в порядок и ограничивает длину текста.
3. `LLM: qualify lead` отправляет заявку в OpenAI и получает JSON: приоритет (`hot` / `warm` / `cold`), краткое описание и следующий шаг.
4. `Build message` собирает текст уведомления. Если модель ответила не JSON, придёт заявка с пометкой «проверить вручную».
5. `Telegram: notify me` отправляет сообщение в ваш чат.

## Запуск

1. В n8n: **Workflows → Import from file** и выберите `workflow.json`.
2. Подключите credentials: **OpenAI API** и **Telegram Bot** (токен от @BotFather).
3. В узле Telegram замените `YOUR_TELEGRAM_CHAT_ID` на ваш chat id.
4. Активируйте воркфлоу и отправьте тестовую заявку:

```bash
curl -X POST https://YOUR-N8N-HOST/webhook/lead \
  -H "Content-Type: application/json" \
  -d '{"name":"Иван","phone":"+7 900 000-00-00","message":"Хочу купить двушку в Краснодаре, бюджет до 9 млн","source":"site"}'
```

## Что можно доработать

- записывать заявки в Google Sheets или PostgreSQL;
- отправлять первый ответ клиенту автоматически;
- добавить напоминание, если заявку никто не взял в работу;
- заменить OpenAI на Claude API.

## Автор

Артём, AI-автоматизация: Python, REST API, webhooks, n8n, OpenAI/Claude API, Telegram-боты.
Telegram: [@krd_lider](https://t.me/krd_lider)

Лицензия: MIT
