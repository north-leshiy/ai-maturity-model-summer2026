# AGENT.md

Презентация на движке **Shower** (`@shower/core` + `@shower/ribbon`). Весь дек — один файл `index.html`. Размер слайда — **1024×640**.

## Dev-сервер для превью

Перед любой визуальной проверкой через `agent-browser` убедись, что dev-сервер поднят. **Не поднимай свой `python -m http.server`** — у проекта есть shower serve.

1. Проверь, занят ли порт 8080:
   ```bash
   curl -s -o /dev/null -w "%{http_code}\n" http://localhost:8080/
   ```
   `200` — сервер уже работает, переходи к скриншотам.
2. Если не `200` — подними сам (в фоне):
   ```bash
   npm run serve   # = shower serve, слушает :8080
   ```

## URL-схема

- Один слайд в полный экран: `http://localhost:8080/index.html?full#<N>` (N — номер слайда, с 1).
- Обзор-сетка всех слайдов: `http://localhost:8080/index.html`.
- Превью диаграмм уровней: `http://localhost:8080/diagrams-preview.html`.

## Визуальная проверка

Полный рецепт снятия скриншота слайда через `agent-browser` — в скиле **`preview-slide`** (`.claude/skills/preview-slide/SKILL.md`).
