# Журнал изменений (Changelog)

## [1.4.0] - 2026-08-30

### Core
- [feat]: Математическая модель калыма переведена на сумму 10 блоков по 0–10 баллов без базовой ставки, итог строго clamp [0, 100] баранов. Анкета собрана из `QUESTION_BLOCKS` (кухня, характер, шопинг, авто, сборы, юмор, уборка, интересы, соцсети, быт) с градациями 10 / 7 / 3 / 0; живой черновик пишет `~N 🐑 из 100`. Вердикты: 0–25 бюджетный стартап, 26–50 студенческий тариф, 51–75 золотой фонд, 76–90 премиум-невеста, 91–100 абсолютный джекпот. `audio.onResult` и конфетти синхронизированы (0–40 грустный звук, 41–75 блеяние, 76–100 фанфары + хор овец + салют). Stories 9:16 и сертификат показывают шкалу 0–100. Файлы: index.html, README.md.

## [1.3.5] - 2026-08-30

### Monetization
- [feat]: Добавлен модуль `AdTracker` — локальная аналитика рекламного блока в `localStorage` по ключу `calc_bee_ad_stats` (`banner_views`, `contact_clicks`, `copy_clicks`, `export_impressions`, `last_interaction`). Счётчики растут при показе/расчёте, клике `openAdLink`, копировании контакта и экспорте Stories/сертификата; в консоль пишется `[AdTracker] Event: … | Total: …`. `Telegram.WebApp.sendData` с `{ event: 'ad_click', type }` вызывается только на `pagehide`/`visibilitychange` (hidden) после клика по контакту или копированию, чтобы не закрывать Mini App на каждое действие. Файлы: index.html, README.md.

## [1.3.4] - 2026-08-30

### UI
- [feat]: Кнопка копирования в `#adBanner` получила bounce/scale-pop (`scale-95` → `scale-105` → `scale-100`), смену фона на неоновый лайм и иконку ✅ «Скопировано!» на 2 с с `disabled`. Успех вызывает `notificationOccurred('success')`, `audio.click()` и toast. Файлы: index.html.

## [1.3.3] - 2026-08-30

### Monetization
- [feat]: В баннер `#adBanner` добавлена кнопка «📋 Скопировать». Клик пишет `email` или `telegramUsername` из `AD_CONFIG` в буфер через `navigator.clipboard.writeText`, даёт `HapticFeedback.notificationOccurred('success')`, toast «Контакт скопирован в буфер 📋» и временно меняет подпись на «✓ Скопировано!». Файлы: index.html.

## [1.3.2] - 2026-08-30

### Monetization
- [feat]: `AD_CONFIG` поддерживает `type: "telegram"` или `"email"`. Баннеры на странице, в Stories и в сертификате показывают @username или mailto; строка «Тема письма» скрывается для Telegram. Клик идёт через `openAdLink` (`openTelegramLink` / `openLink` / `mailto`). Файлы: index.html.

## [1.3.1] - 2026-08-30

### Monetization
- [feat]: Тексты и mailto рекламного блока вынесены в `AD_CONFIG`. Основной баннер, плашки в Stories/сертификате и `openAdMail` заполняются из одного объекта; тема письма кодируется через `encodeURIComponent`. Файлы: index.html.

## [1.3.0] - 2026-08-30

### Export
- [feat]: Во все форматы экспорта добавлен настраиваемый водяной знак с ссылкой на бота. Конфиг `BOT_CONFIG` задаёт handle и URL; Stories получает плашку «🤖 Бот: @calc_bee_bot», сертификат — строку верификации в футере; jsPDF вешает кликабельный `link()` на плашку, не перекрывая печать и подпись Чабана. Файлы: index.html.
- [feat]: В шаблоны Stories и сертификата встроен рекламный баннер «Место для вашей рекламы» (жёлтый Neubrutalism, email irabotaros@gmail.com, тема «Реклама в calc_bee»). Баннер стоит над водяным знаком и не перекрывает вердикт, перки, печать и подпись Чабана; html2canvas и jsPDF снимают его как часть макета. Файлы: index.html.

## [1.2.0] - 2026-08-30

### UI
- [feat]: Три кнопки экспорта на экране результата заменены на единый селект формата и кнопку «📥 Экспортировать результат». Выбор вызывает `exportStories`, `exportCertPng` или `exportCertPdf`; смена формата и клик дают `HapticFeedback.impactOccurred('light')`; toast и предпросмотр модалки сохранены. Файлы: index.html.

## [1.1.1] - 2026-08-30

### Docs
- [docs]: Зафиксированы системные правила Cursor для calc_bee. Добавлены `.cursorrules` и правила в `.cursor/rules/` (архитектура SPA, SemVer-шаблон CHANGELOG, состав README, запрет сборщиков, протокол отчётной таблицы). Файлы: .cursorrules, .cursor/rules/project-architecture.mdc, .cursor/rules/docs-standards.mdc, .cursor/rules/output-format.mdc, README.md, CHANGELOG.md.

## [1.1.0] - 2026-08-30

### Monetization & Core
- [feat]: Добавлен рекламный баннер и подключен GitHub Pages. Интегрирован рекламный блок с контактом irabotaros@gmail.com и темой «Реклама в calc_bee». Настроен деплой по адресу https://andreyros.github.io/calc_bee/. Файлы: index.html, README.md.
- [feat]: Генерация Stories 9:16 и гербового PDF-сертификата. Реализован экспорт вертикальной картинки 1080x1920 для соцсетей через html2canvas и официального документа с серийным номером № БАРАН-XXXX-VIP через jsPDF. Файлы: index.html.
- [feat]: Поддержка Telegram WebApp API и Web Audio. Интегрированы методы Telegram SDK (ready, expand, HapticFeedback), автоподстановка имени и синтез блеяния овец через Web Audio API. Файлы: index.html.

## [1.0.0] - 2026-08-30

### Initial Release
- [feat]: Базовая версия шуточного калькулятора калыма. Реализована форма с критериями (кухня, характер, шопинг, мемы, авто, чекбоксы) и формула расчета выкупа в баранах от -20 до 250. Файлы: index.html.
