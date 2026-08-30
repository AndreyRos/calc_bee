# Калькулятор калыма 🐑 (calc_bee)

Шуточный Telegram Mini App (TMA) для расчета выкупа за девушку в баранах с генерацией вирусных Stories (9:16), официальных PDF-сертификатов, звуковыми эффектами и рекламным модулем.

Стек: Vanilla JavaScript · Tailwind CSS · Telegram WebApp API · Web Audio API · html2canvas · jsPDF · canvas-confetti · GitHub Pages

## 1. Архитектура и Data Flow

### Структура репозитория
- `index.html` — единая точка входа (HTML5, Tailwind CSS CDN, Vanilla JS, Telegram SDK, Web Audio API, html2canvas, jsPDF).
- `README.md` — архитектурная спецификация и руководство по деплою/запуску.
- `CHANGELOG.md` — версионный журнал изменений (SemVer).
- `.cursorrules` и `.cursor/rules/` — правила разработки, документации и отчётности.

### Схема потоков
Пользователь / Telegram Client -> Telegram Mini App UI -> Telegram WebApp SDK -> Формула расчета калыма -> Web Audio Синтезатор 🐑 -> html2canvas / jsPDF Экспорт -> Stories 9:16 PNG / Гербовый Сертификат PDF.

### Поток данных
1. Пользователь запускает Mini App, имя автоматически подтягивается из Telegram.
2. При выборе критериев срабатывает тактильный отклик (HapticFeedback) и динамический пересчет черновика (шкала 0–100).
3. Формула: Итог = сумма баллов за 10 блоков (в каждом 0 / 3 / 7 / 10). Базовой ставки нет, clamp строго [0, 100] баранов.
4. Вердикт по шкале: 0–25 бюджетный стартап, 26–50 студенческий тариф, 51–75 золотой фонд, 76–90 премиум-невеста, 91–100 абсолютный джекпот.
5. При расчете запускается countUp, `audio.onResult` (0–40 грустный звук, 41–75 блеяние, 76–100 фанфары + хор овец) и конфетти от 76 баллов.
6. Результат экспортируется через селект формата: Stories 9:16 PNG, сертификат PNG или гербовый PDF с номером № БАРАН-XXXX-VIP и шкалой 0–100. На каждом документе водяной знак со ссылкой на бота (`BOT_CONFIG` → t.me/calc_bee_bot) и рекламный баннер «Место для вашей рекламы».

## 2. Диагностический чеклист

- Telegram SDK: window.Telegram.WebApp инициализирован, first_name подставлен.
- Тактильный отклик: HapticFeedback дает легкую вибрацию на опциях, тяжелую на результате.
- Модель баллов: 10 радио-блоков, live-счётчик `~N 🐑 из 100`, итог не выходит за [0, 100].
- Аудиомодуль: Web Audio API Oscillator синтезирует блеяние без блокировок браузера; пороги `onResult` — 0–40 / 41–75 / 76–100.
- Экспорт: селект формата + кнопка «Экспортировать результат» вызывают html2canvas / jsPDF без смещения шрифтов; водяной знак бота и рекламный баннер читаются на PNG, ссылка бота кликабельна в PDF.
- Хостинг: GitHub Pages отдает статус 200 по адресу https://andreyros.github.io/calc_bee/
- Реклама / AdTracker: в консоли есть `[AdTracker] Event: … | Total: …`; в `localStorage` ключ `calc_bee_ad_stats` содержит `banner_views`, `contact_clicks`, `copy_clicks`, `export_impressions`, `last_interaction`.

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

По вопросам размещения рекламы в приложении (контакт задаётся в `AD_CONFIG` в `index.html`, `type: "telegram"` или `"email"`):
- Telegram: [@andreyros](https://t.me/andreyros)
- Email (если `type: "email"`): irabotaros@gmail.com, тема «Реклама в calc_bee»
