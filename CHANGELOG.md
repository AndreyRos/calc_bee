# Журнал изменений (Changelog)

## [1.8.1] - 2026-08-31

### Share
- [fix]: Убран повтор CTA «А сколько барашек дадут за тебя?»: фраза остаётся только в тексте `shareResult`, у `share.html` / `index.html` `og:title` и `og:description` сведены к «Калькулятор калыма» / «Шуточный рейтинг выкупа в баранах», чтобы превью Telegram не дублировало сообщение. Файлы: share.html, index.html.

## [1.8.0] - 2026-08-31

### Share
- [feat]: Кнопка «Поделиться» собирает вирусный текст (`А сколько барашек дадут за тебя? Нажми и узнай 👇` + `https://t.me/calc_kalym_bot/app`) и карточку с картинкой `mainpic.jpg`: `t.me/share/url` ведёт на `share.html` с Open Graph (`og:image`), страница сразу редиректит в Mini App. Файлы: index.html, share.html, README.md.

## [1.7.3] - 2026-08-31

### Share
- [fix]: Шаринг больше не отдаёт GitHub Pages: `shareLink` жёстко `https://t.me/calc_kalym_bot/app`, та же ссылка дублируется в тексте (Telegram подменяет `t.me` в `share/url` на URL Mini App). `BOT_CONFIG.url` и плашки Stories/сертификата ведут в `/app`. Файлы: index.html, README.md.

### Export
- [fix]: Плашка `#stBotLink` выровнена по центру (`min-height: 36px`, emoji в слоте 18px, `width: 100%`), тень не режется (`padding` футера 4px). Файлы: index.html.

## [1.7.2] - 2026-08-31

### Share
- [fix]: `shareResult` больше не шарит `window.location.href`: `url` собирается как `t.me/share/url` с прямой ссылкой Mini App (`BOT_CONFIG.url + '/app'` → `https://t.me/calc_kalym_bot/app`) и вирусным текстом (рейтинг Госкомитета, имя, бараны, ранг). Открытие через `tg.openTelegramLink(url)` с фоллбэком на `window.open`. Файлы: index.html, README.md.

## [1.7.1] - 2026-08-31

### Export
- [fix]: Плашка `#stScale` (`/ 100 🐑`) больше не режет текст и эмодзи: `min-width: 124px`, `min-height: 32px`, `padding: 8px 20px`, `overflow: visible`; цифры и барашек разведены по `inline-flex` со слотом 18px под 🐑; строка `#stScoreRow` выровнена по центру крупного счёта. Файлы: index.html.

## [1.7.0] - 2026-08-31

### Monetization
- [feat]: Подключена сеть Adsgram (`sad.min.js` в `<head>`). Модуль `AdController` инициализирует `window.Adsgram.init({ blockId, debug: false })`, `showRewarded()` резолвит просмотр и реджектит пропуск/ошибку, а при отсутствии SDK или плейсхолдере `YOUR_ADSGRAM_BLOCK_ID` не ломает флоу и резолвит Promise. Экспорт Stories/PNG/PDF (`runExport`) ждёт rewarded-ролик, перед показом toast «Загрузка рекламного видео... 🎬»; `AdTracker` считает `rewarded_shows` (`onStart`) и `rewarded_completions` (`onReward`). Файлы: index.html, README.md.

## [1.6.3] - 2026-08-31

### Export
- [fix]: Реклама переведена на `AD_CONFIG.type: "email"` (`irabotaros@gmail.com`, тема «Реклама в calc_bee»): баннер на странице, плашки Stories `#stAdText` и сертификата `#certAdText` показывают email и собирают `mailto:irabotaros@gmail.com?subject=…`, кнопка копирования пишет тот же адрес в буфер. В Stories у `#stBadge` заданы `overflow: visible; box-sizing: border-box`, плашка `#stScale` (`/ 100 🐑`) получила `width: auto; max-width: fit-content; white-space: nowrap; padding: 4px 10px; display: inline-flex`, строка `#stScoreRow` выровнена через `align-items: baseline`. Бот зафиксирован как `@calc_kalym_bot` (`BOT_CONFIG`, кнопка `#stBotLink`). Файлы: index.html, README.md.

## [1.6.2] - 2026-08-31

### Export
- [fix]: `BOT_CONFIG` переведён на актуальный бот `@calc_kalym_bot` (`https://t.me/calc_kalym_bot`): плашка `#stBotLink` в Stories, строка верификации `#certBotMark` в сертификате и кликабельный `addPdfBotLink` в PDF ведут на новый handle. Файлы: index.html, README.md.

## [1.6.1] - 2026-08-31

### Export
- [fix]: Плашка `#stScale` (`/ 100 🐑`) в Stories выровнена по базовой линии крупного счёта: `#stScoreRow` переведён на `align-items: flex-end` с `gap: 10px`, у `#stCount` `line-height: 0.9`, у плашки `margin-bottom: 4px`, чтобы бейдж не висел выше цифр 0 / 79 / 100. Файлы: index.html.

## [1.6.0] - 2026-08-31

### Export
- [feat]: Stories `#storiesCard` / `#stPanel` и сертификат `#certInner` получают тематические геометрические водяные знаки на 10 рангов `CERT_TIERS`: CSS-градиенты (caution, craft fibers, blueprint, dot matrix, шеврон, дамаск, tech-сетка, pinstripe, sunburst) плюс inline Data-URI SVG (штрих-код, ключи/коробки, чертежи, колосья, ромбы ГОСТ, изометрические кубы, соты, арабеска, дворцовая мозаика). Слои `#stWallpaper`, `#stPattern`, `#certPattern` стоят под текстом (`pointer-events: none`, `z-index: 0`, opacity 0.09–0.14), без внешних HTTP — html2canvas не ловит CORS. Файлы: index.html, README.md.

## [1.5.6] - 2026-08-31

### Export
- [fix]: Герб-эмодзи в бейдже Stories `#stBadge` увеличен до 28px и выровнен по центру рядом с названием ранга, чтобы корона 👑 (и остальные значки) не терялась на фоне плашки. Файлы: index.html.

## [1.5.5] - 2026-08-31

### Export
- [fix]: Круглые и сургучные печати больше не режут текст: длинные строки ужаты (`VIP` / `ШЕЙХ`, `APPROVED` / `CLASS`, `ГОСТ` / `ЗНАК`), кегль считается по длине подписи, каждая строка `nowrap` внутри круга, `overflow: hidden` не даёт буквам вылезти на обод. Файлы: index.html.

## [1.5.4] - 2026-08-31

### Export
- [fix]: Перки Stories больше не режутся по `max-height: 52px`: у `#stPerks` сняты потолок и `overflow: hidden`, каждая плашка растёт под 2 строки (`line-height: 1.35`, `padding: 5px 7px`, `word-break: break-word`), полный текст вроде «Борщ и стейк ресторанного уровня» читается целиком. Файлы: index.html.

## [1.5.3] - 2026-08-31

### Export
- [fix]: Вердикт синхронизирован с 10 рангами `CERT_TIERS` (`tier.line`), чтобы бейдж и текст не повторяли чужой ранг. Stories режут перки в 2 однострочные строки с ellipsis; у «Комфорт-плюс» бейдж получил контрастный `badgeInk`, у премиума — короткий `shortTitle`, у джекпота штамп ужат до «ДЖЕКПОТ / 100% ВЫКУП», extra 60–69 сменён на 🥂 чтобы не дублировать 💎 премиума. Файлы: index.html, README.md.

## [1.5.2] - 2026-08-31

### Export
- [fix]: Stories `#storiesCard` переведены на колоночный Flexbox без абсолютных перекрытий: счёт `56px` и плашка `/ 100 🐑` стоят в одной строке, вердикт с 2 перками (11px) слева, печать ранга справа, компактный подвал (реклама 2 строки + `🤖 @calc_bee_bot` 28px) прижат через `margin-top: auto` внутри 360×640. В `CERT_TIERS` добавлен `score` — ярко-белый / золотой / неоновый цвет счёта для тёмных рангов (Дефицит, Бизнес, Премиум, Джекпот); имя, счёт и вердикт держат контраст ≥ 4.5:1. Сертификат `#certCard` собрал перки, дату, печать и подпись Чабана в flex-ряд, реклама и водяной знак больше не наезжают на сургуч. Файлы: index.html.

## [1.5.1] - 2026-08-31

### Export
- [fix]: Треугольный штамп «СДАНО НА ТРОЙКУ» рисуется через SVG, чтобы html2canvas не терял `clip-path`. Сургучные печати 70–100 получили внешнее кольцо вместо inset-тени, текст читается. Перки сертификата сужены (`max-width: 640px`), чтобы не наезжать на штамп и подпись. Файлы: index.html.

## [1.5.0] - 2026-08-31

### Export
- [feat]: Stories `#storiesCard` и сертификат `#certCard` получают 10 визуальных рангов из `CERT_TIERS` по десяткам `Math.min(9, Math.floor(rams / 10))`. Каждый ранг задаёт фон, рамку, герб-эмодзи и штамп (прямоугольник / треугольник / круг / сургуч); `fillExports` перекрашивает карточки без наложения на рекламный баннер и водяной знак бота. Файлы: index.html, README.md.

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
