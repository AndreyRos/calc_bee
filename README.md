# Калькулятор калыма 🐑 (calc_bee)

Шуточный Telegram Mini App (TMA) для расчета выкупа за девушку в баранах с генерацией вирусных Stories (9:16), официальных PDF-сертификатов, звуковыми эффектами и рекламным модулем.

Стек: Vanilla JavaScript · Tailwind CSS · Telegram WebApp API · Web Audio API · html2canvas · jsPDF · canvas-confetti · GitHub Pages

## 1. Архитектура и Data Flow

### Структура репозитория
- `index.html` — единая точка входа (HTML5, Tailwind CSS CDN, Vanilla JS, Telegram SDK, Web Audio API, html2canvas, jsPDF).
- `share.html` — лендинг шаринга с Open Graph (`og:image` → `mainpic.jpg`) и редиректом в Mini App.
- `mainpic.jpg` — промо-картинка превью шаринга в Telegram.
- `README.md` — архитектурная спецификация и руководство по деплою/запуску.
- `CHANGELOG.md` — версионный журнал изменений (SemVer).
- `.cursorrules` и `.cursor/rules/` — правила разработки, документации и отчётности.

### Схема потоков
Пользователь / Telegram Client -> Telegram Mini App UI -> Telegram WebApp SDK -> Формула расчета калыма -> Web Audio Синтезатор 🐑 -> html2canvas / jsPDF Экспорт -> Stories 9:16 PNG / Гербовый Сертификат PDF.

### Поток данных
1. Пользователь запускает Mini App, имя автоматически подтягивается из Telegram.
2. При выборе критериев срабатывает тактильный отклик (HapticFeedback) и динамический пересчет черновика (шкала 0–100).
3. Формула: Итог = сумма баллов за 10 блоков (в каждом 0 / 3 / 7 / 10). Базовой ставки нет, clamp строго [0, 100] баранов.
4. Вердикт совпадает с визуальным рангом `CERT_TIERS` (шаг 10): 0–9 тотальный дефицит, 10–19 бюджетный стартап, 20–29 студенческий эконом, 30–39 базовый городской, 40–49 твердый середняк, 50–59 золотая середина, 60–69 комфорт-плюс, 70–79 бизнес-класс, 80–89 премиум-невеста, 90–100 султанский джекпот.
5. При расчете запускается countUp, `audio.onResult` (0–40 грустный звук, 41–75 блеяние, 76–100 фанфары + хор овец) и конфетти от 76 баллов.
6. Перед экспортом `runExport` проверяет `AdController.isConfigured()`: rewarded-ролик Adsgram и тост «Загрузка рекламного видео... 🎬» только если блок задан; иначе сразу генерация с тостом «Генерируем результат... ⏳». На `#exportBtn` крутится спиннер-овечка («Печатаем сертификат...»), `_busy` и кнопка снимаются в `finally`. `capture` временно выносит карточку в `fixed` с координатами 0, чтобы html2canvas не снимал `left: -12000px`. Результат — Stories 9:16 PNG, сертификат PNG или гербовый PDF с номером № БАРАН-XXXX-VIP и шкалой 0–100. На каждом документе водяной знак Mini App (`BOT_CONFIG` → t.me/calc_kalym_bot/app) и рекламный баннер.
7. Визуальный ранг карточек (`CERT_TIERS`, шаг 10 баллов): 0–9 тотальный дефицит (рваный чек, штамп «БРАК», caution-штриховка + штрих-код), 10–19 бюджетный стартап (крафт, ключи/коробки), 20–29 студенческий эконом (тетрадная клетка, чертежи), 30–39 базовый городской (dot matrix / modern grid), 40–49 твердый середняк (шеврон / колосья), 50–59 золотая середина (дамасский ромб ГОСТ), 60–69 комфорт-плюс (изометрические кубы, tech-сетка), 70–79 бизнес-класс (pinstripe + соты), 80–89 премиум / шейх-люкс (арабеска с золотым свечением), 90–100 султанский джекпот (sunburst + дворцовая мозаика). Паттерны — CSS + inline Data-URI SVG на слоях `#stWallpaper` / `#stPattern` / `#certPattern` (opacity 0.09–0.14, без внешних запросов). Индекс: `Math.min(9, Math.floor(rams / 10))`.
8. Шаринг (`shareResult`) открывает `t.me/share/url` со ссылкой Mini App `https://t.me/calc_kalym_bot/app` (без GitHub Pages в тексте). Текст: рейтинг Госкомитета, имя, бараны, ранг, CTA «А сколько барашек дадут за тебя? Нажми и узнай 👇» и та же ссылка. Вызов через `tg.openTelegramLink` / `openLink`, фоллбэк — `window.open`.

## 2. Диагностический чеклист

- Telegram SDK: window.Telegram.WebApp инициализирован, first_name подставлен; шаринг идёт в `t.me/share/url` со ссылкой `https://t.me/calc_kalym_bot/app` (без `share.html` в сообщении).
- Тактильный отклик: HapticFeedback дает легкую вибрацию на опциях, тяжелую на результате.
- Модель баллов: 10 радио-блоков, live-счётчик `~N 🐑 из 100`, итог не выходит за [0, 100].
- Аудиомодуль: Web Audio API Oscillator синтезирует блеяние без блокировок браузера; пороги `onResult` — 0–40 / 41–75 / 76–100.
- Экспорт: селект формата + `#exportBtn` вызывают html2canvas / jsPDF; на время генерации кнопка disabled и показывает спиннер 🐑 «Печатаем сертификат...», состояние возвращается в `finally`; `capture` снимает карточку с `x/y/scroll = 0` (не с `left: -12000px`); в превью подсказка «💡 Зажми картинку, чтобы сохранить в галерею»; `#savePreview` кладёт PNG в blob без `fetch(data:)`, в жесте клика зовёт `navigator.share({ files })`, в Telegram при блоке скачивания копирует картинку в буфер или просит зажать превью; ранг `CERT_TIERS` совпадает со шкалой; геометрический watermark ранга рисуется inline (CSS + Data-URI SVG) без CORS; штамп не перекрывает рекламу и водяной знак; ссылка бота кликабельна в PDF; ошибка не глотается — `console.error` + toast «Ошибка экспорта: …».
- Хостинг: GitHub Pages отдает статус 200 по адресу https://andreyros.github.io/calc_bee/
- Реклама / AdTracker: в консоли есть `[AdTracker] Event: … | Total: …`; в `localStorage` ключ `calc_bee_ad_stats` содержит `banner_views`, `contact_clicks`, `copy_clicks`, `export_impressions`, `rewarded_shows`, `rewarded_completions`, `last_interaction`.
- Adsgram: в `<head>` загружен `https://sad.adsgram.ai/js/sad.min.js`; `AdController.init()` не падает; toast «Загрузка рекламного видео... 🎬» и `showRewarded()` только при `isConfigured() === true`; при плейсхолдере `YOUR_ADSGRAM_BLOCK_ID` сразу тост «Генерируем результат... ⏳» и файл без ролика.

## 3. Быстрый старт

Команда для локального запуска:
python -m http.server 8000

## 4. Настройка BotFather

1. Открой @BotFather в Telegram и вызови команду /newapp.
2. Выбери своего бота.
3. Укажи название и описание приложения.
4. В поле URL вставь: https://andreyros.github.io/calc_bee/
5. Задай short name (например, kalym).

## 5. Реклама и сотрудничество

Клики и показы рекламного блока считает `AdTracker` в `index.html` (ключ `calc_bee_ad_stats` в localStorage). При закрытии Mini App после клика по контакту или копированию бот может получить `sendData({ event: 'ad_click', type })`, если SDK доступен.

Rewarded-ролик перед экспортом крутит Adsgram только если `AdController.isConfigured()` (реальный `BLOCK_ID`, не плейсхолдер): SDK `https://sad.adsgram.ai/js/sad.min.js`, контроллер `AdController` в `index.html`. Подставь ID из [partner.adsgram.ai](https://partner.adsgram.ai) вместо `YOUR_ADSGRAM_BLOCK_ID`. Пока ID не задан, ролик не вызывается и генерация стартует сразу. Пропуск или ошибка ролика реджектит Promise — экспорт не стартует, `_busy` всё равно сбрасывается в `finally`. События `onStart` / `onReward` пишутся в `rewarded_shows` и `rewarded_completions`.

По вопросам размещения рекламы в приложении (контакт задаётся в `AD_CONFIG` в `index.html`, текущий `type: "email"`):
- Email: [irabotaros@gmail.com](mailto:irabotaros@gmail.com?subject=%D0%A0%D0%B5%D0%BA%D0%BB%D0%B0%D0%BC%D0%B0%20%D0%B2%20calc_bee), тема «Реклама в calc_bee»
