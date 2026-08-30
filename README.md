# Калькулятор калыма 🐑 (calc_bee)

Шуточный Telegram Mini App (TMA) для расчета выкупа за девушку в баранах с генерацией вирусных Stories (9:16), официальных PDF-сертификатов, звуковыми эффектами и рекламным модулем.

Стек: Vanilla JavaScript · Tailwind CSS · Telegram WebApp API · Web Audio API · html2canvas · jsPDF · canvas-confetti · GitHub Pages

## 1. Архитектура и Data Flow

### Схема потоков
Пользователь / Telegram Client -> Telegram Mini App UI -> Telegram WebApp SDK -> Формула расчета калыма -> Web Audio Синтезатор 🐑 -> html2canvas / jsPDF Экспорт -> Stories 9:16 PNG / Гербовый Сертификат PDF.

### Поток данных
1. Пользователь запускает Mini App, имя автоматически подтягивается из Telegram.
2. При выборе критериев срабатывает тактильный отклик (HapticFeedback) и динамический пересчет.
3. Базовая ставка — 50 баранов, модификаторы от -35 до +40, clamp-ограничение [-20, 250].
4. При расчете запускается countUp счетчик, блеяние овец и салют из конфетти.
5. Результат можно скачать как Stories (9:16) или PDF-сертификат с номером № БАРАН-XXXX-VIP.

## 2. Диагностический чеклист

- Telegram SDK: window.Telegram.WebApp инициализирован, first_name подставлен.
- Тактильный отклик: HapticFeedback дает легкую вибрацию на опциях, тяжелую на результате.
- Аудиомодуль: Web Audio API Oscillator синтезирует блеяние без блокировок браузера.
- Экспорт: html2canvas и jsPDF рендерят документы без смещения шрифтов.
- Хостинг: GitHub Pages отдает статус 200 по адресу https://andreyros.github.io/calc_bee/

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

По вопросам размещения рекламы в приложении:
- Email: irabotaros@gmail.com
- Тема письма: Реклама в calc_bee
