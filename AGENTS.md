# Сайт-портфолио Елены Карташовой

Статический сайт художницы на GitHub Pages. Сборщика нет: только HTML и CSS.

- Публичный адрес: https://kartashovaelena.github.io/
- Репозиторий: https://github.com/kartashovaelena/kartashovaelena.github.io (ветка `main`, корень)
- Публикация: пуш в `main` → GitHub Pages пересобирает сайт автоматически (обычно до минуты)

## Структура

```
index.html            главная — заглушка «сайт в разработке»
404.html              страница «Страница не найдена» (GitHub Pages отдаёт её автоматически)
favicon.ico           копия assets/icons/favicon.ico в корне для неявных запросов /favicon.ico
AGENTS.md             этот файл
assets/style.css      все стили
assets/avatar-ek.jpg  аватарка (640×640, исходник для иконок и OG)
assets/icons/         набор фавиконок (только prod), apple-touch, PWA, site.webmanifest
assets/og/            og-cover-1200x630.png и og-cover-1080x1080.png
design/               макеты, превью и исходники генерации — в git не попадает (.gitignore)
```

## Дизайн-токены

| Что | Значение |
|---|---|
| Шрифт | EB Garamond через Google Fonts, веса 400 и 500 |
| Цвет текста | `#000000` |
| Акцент / выделение | фон `#AEBC73`, текст `#000` |
| Hover ссылок | `#97A55F` (чуть темнее акцента), подчёркивание наследует цвет текста |
| Фон | `#FFFFFF` |
| Заголовок | 24px, weight 500, line-height 100%, по центру |
| Основной текст | 16px, weight 400, line-height 130% |
| Ссылки | 16px, weight 400, line-height 130%, чёрные, `underline dotted` 1px, `text-underline-offset: 3px` |
| Аватарка | 64×85 (овал 3:4), `border-radius: 50%`, `object-fit: cover` |
| Вертикальные отступы | аватар → 16px → заголовок → 8px → текст → 32px → ссылки; между ссылками 8px |
| Колонка контента | 320px, блок центрируется по X и Y на всех брейкпоинтах |

Брейкпоинтов как таковых нет: одна центрированная колонка работает на десктопе, планшете и мобилке. Проверять нужно минимум на 1440×900, 768×1024 и 390×844.

## Заглушка (index.html)

Порядок элементов: аватарка → заголовок «Художница Елена Карташова» → текст «Сайт в разработке. Вы можете связаться или ознакомиться с работами по ссылкам ниже:» → список ссылок.

Ссылки (все внешние — `target="_blank"` + `rel="noopener noreferrer"`, почта — `mailto:` без новой вкладки):

| Подпись | Куда |
|---|---|
| Авито | https://www.avito.ru/user/59639c4791173a082c0aa4af482d1f63/profile |
| Телеграм | https://t.me/nukyhelenuky |
| Тг-канал | https://t.me/helenuky |
| Группа Вконтакте | https://vk.ru/kartashovaelenart |
| Написать на почту | mailto:lenka24.lk34@gmail.com |

Ссылки размечены как `<ul class="links">`, чтобы порядок и отступы задавались одним `gap`.

## 404 (404.html)

Тот же каркас и стили, что у заглушки: заголовок «Страница не найдена» (24px medium) и ссылка «На главную» (`/`) в стиле ссылок заглушки. Аватарки и списка ссылок нет. `<meta name="robots" content="noindex">`.

## Фавиконки и OG

Набор собран скиллом favicon-creator из `design/avatar-crop.jpg` (кроп лица, 1024×1024), только окружение **prod**:

- `assets/icons/` — `favicon.ico` (16/24/32/48), PNG 16/24/32/48, варианты rounded и circle, `apple-touch-icon.png` 180, `icon-192/512`, `icon-maskable-512`; в `head` подключены базовые.
- `favicon.ico` продублирован в корень, потому что браузеры запрашивают его по умолчанию.
- `assets/icons/site.webmanifest` — имя, `short_name` «Елена Карташова», белый `theme_color`/`background_color`, пути к иконкам относительные.
- OG-картинки рисуются скриптом `design/make-og.py` (Pillow + EB Garamond Medium TTF из `design/fonts/`), без скриншотов браузера — иначе получаются искажения при масштабировании. Результат: `assets/og/og-cover-1200x630.png` и `assets/og/og-cover-1080x1080.png` — белый фон, овальная аватарка 3:4, подпись EB Garamond Medium, группа центрируется по X и Y.
- Размеры подписи масштабированы от сайта (24px на колонке 320px): 1200×630 — аватар 160×213, подпись 56px; 1080×1080 — аватар 180×240, подпись 64px. Литеральные 24px на OG-картинке нечитаемы в превью мессенджеров.
- Пересборка: `python3 design/make-og.py`. Если менялся файл OG, менять и имя файла (или добавлять `?v=`) — Telegram кеширует картинку по URL.
- При смене аватарки пересобрать: кроп → `node build.mjs --input design/avatar-crop.jpg --name "Художница Елена Карташова" --slug ek --envs prod --og false --zip false --out-dir assets/icons` (скилл `favicon-creator`), затем OG → `python3 design/make-og.py`.

## Правила

- Не менять типографику точечно «под макет»: все размеры и отступы живут в `assets/style.css`.
- Внешние ссылки всегда с `noopener noreferrer` и `target="_blank"`.
- `design/` не публикуется — макеты и генераторы остаются локальными.
- Тексты на русском, без капса и без декоративных элементов, которых нет в макете.
- Проверять изменения локально (`python3 -m http.server`) до пуша.

## Доступ и деплой

- Пуш: `git push origin main` из папки проекта. Токен GitHub (scope `repo`, выпущен 24.11.2026, истекает 24.12.2026) лежит в macOS Keychain под пользователем `kartashovaelena`, remote: `https://kartashovaelena@github.com/kartashovaelena/kartashovaelena.github.io.git`.
- Если пуш просит пароль — токен истёк, выпустить новый в Settings → Developer settings → Tokens (classic) и обновить в Keychain.
- GitHub Pages: ветка `main`, папка `/` (корень), без Jekyll-обработки (`.nojekyll` не нужен: нет папок с `_`).
