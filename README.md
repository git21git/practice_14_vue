# practice_14_vue: Менеджер книг для чтения

Автор: Чудинова Мария Александровна

## Описание проекта

`practice_14_vue` — учебное приложение на Vue 3 для ведения списка книг. Пользователь может добавлять книги, отмечать их как прочитанные, ставить оценки, искать записи, фильтровать список и видеть статистику. Данные сохраняются в `localStorage` и восстанавливаются после обновления страницы.

Репозиторий: https://github.com/git21git/practice_14_vue  
GitHub Pages: https://git21git.github.io/practice_14_vue/

## Функции приложения

- добавление книги через форму;
- поля книги: название, автор, жанр;
- отметка книги как прочитанной;
- оценка прочитанной книги от 1 до 5 звёзд;
- удаление книги;
- фильтрация: все, непрочитанные, прочитанные;
- поиск по названию или автору;
- статистика: всего книг, прочитано, непрочитано;
- сохранение и восстановление списка книг через `localStorage`.

## Используемые технологии

- HTML5;
- CSS3;
- JavaScript;
- Vue 3;
- Composition API;
- Vite;
- localStorage;
- Git;
- GitHub Pages;
- `gh-pages`.

## Структура проекта

```text
practice_14_vue/
├── index.html
├── package.json
├── package-lock.json
├── README.md
├── REPORT.md
├── vite.config.js
└── src/
    ├── App.vue
    ├── main.js
    ├── style.css
    └── components/
        ├── BookCard.vue
        ├── BookFilters.vue
        ├── BookForm.vue
        └── BookStats.vue
```

## Установка

```bash
npm install
```

## Локальный запуск

```bash
npm run dev
```

## Сборка

```bash
npm run build
```

## Предпросмотр production-сборки

```bash
npm run preview
```

## Деплой на GitHub Pages

```bash
npm run deploy
```

После деплоя приложение должно быть доступно по адресу:

```text
https://git21git.github.io/practice_14_vue/
```
