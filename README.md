# Тестовое задание для [Студии Артемия Лебедева](https://www.artlebedev.ru)

Vue-приложение с динамической генерацией формы по JSON-конфигу.

## Возможности

- Генерация полей из `src/data.json` (text, email, password, select, checkbox)
- Валидация через нативные HTML-атрибуты (`required`, `minLength`, `pattern`)
- Отображение введённых данных в реальном времени
- Попап подтверждения после отправки

## Стек

Vue 3 · TypeScript · Vite

## Запуск

```bash
npm install
npm run dev
```

Сборка:

```bash
npm run build
```

## Структура

- `src/data.json` — описание полей формы
- `src/components/FormGenerator.vue` — компонент генератора
- `src/components/AppPopup.vue` — модальное окно
