---
name: preview-slide
description: Визуально проверить слайд(ы) презентации на движке Shower через agent-browser — снять скриншот слайда, диаграммы или всего дека и посмотреть, как он рендерится. Использовать, когда пользователь просит "визуально проверь", "покажи слайд N", "как выглядит", "сними скриншот слайда/диаграммы", "прогони весь дек" или проверить вёрстку после правки HTML/SVG.
allowed-tools: Bash(curl:*), Bash(npm run serve:*), Bash(npx agent-browser:*), Read
---

# preview-slide

Снять скриншот слайда презентации (движок **Shower**, файл `index.html`, слайд 1024×640) через `agent-browser` и посмотреть результат. Браузер уже установлен (`agent-browser install`).

## Рецепт

### 1. Поднять / проверить dev-сервер
Сервер shower serve слушает **:8080**. Не поднимай свой http.server.
```bash
curl -s -o /dev/null -w "%{http_code}\n" http://localhost:8080/   # 200 → уже работает
npm run serve                                                     # если не 200 — подними в фоне
```

### 2. Выставить вьюпорт под слайд (обязательно — иначе чёрные поля)
```bash
npx agent-browser set viewport 1024 640
```

### 3. Открыть нужный URL
- Один слайд в полный экран: `http://localhost:8080/index.html?full#<N>` (N с 1)
- Обзор-сетка всех слайдов: `http://localhost:8080/index.html`
- Превью диаграмм: `http://localhost:8080/diagrams-preview.html`

```bash
npx agent-browser open "http://localhost:8080/index.html?full#2"
npx agent-browser wait 1000          # дать слайду отрендериться
npx agent-browser screenshot /tmp/slide.png        # для длинных страниц добавь --full
```

### 4. Посмотреть результат
Прочитай PNG инструментом **Read** — изображение покажется визуально. Найди баги (наезды текста, обрезку, кривое выравнивание) → правь HTML/SVG → повтори шаги 3–4.

## Прогнать весь дек
Узнай число слайдов и сними каждый:
```bash
npx agent-browser open "http://localhost:8080/index.html"
npx agent-browser get count ".slide"      # сколько слайдов
```
Затем цикл по `?full#1..N`, снимая `/tmp/slide-<N>.png`, и читай их через Read.

## Заметки
- Refs `@eN` устаревают после каждого изменения страницы — пере-снимай `snapshot` перед взаимодействием.
- Полный гайд по agent-browser: `npx agent-browser skills get core --full`.
- Контекст движка и сервера — в `AGENT.md` в корне проекта.
