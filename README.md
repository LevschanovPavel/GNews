# REST API для получения новостей

## Функционал

### 1. Проинициализировать проект

- Реализовать базовый веб-сервер на express (для лайвкодинга минимально в одном файле)
- Допускается использование [npmjs.com](https://www.npmjs.com) — в качестве документации
- Запуск проекта командами `npm start` для продакшена и `npm run dev` для разработки
- Структура проекта:

```
project/
│    
├── gnews.js
├── index.js
├── .env
├── package.json

для опциональных заданий
├── utils.js
├── middleware.js
├── translate.js    
```

- Технологии: Node.js, Express, Axios, dotenv, translate, nodemon
- Сервис предоставляющий доступ к новостям: [GNews](https://gnews.io)
  - GET: `/api/v4/top-headlines`
    - Request parameters:
      - apiKey: `b977d558cf0591192d78c47fca669ed2`
      - category: `business`, `entertainment`, `general`, `health`, `science`, `sports`, `technology`
      - max: `10`
  - Пример URL:
    ```
    https://gnews.io/api/v4/top-headlines?country=us&category=general&apikey=b977d558cf0591192d78c47fca669ed2&max=10
    ```

---

### 2. Реализовать получение новостей по категории  
#### GET `/news/:category`

- Роут должен возвращать новости из выбранной категории (для лайвкодинга пагинацию опускаем, просто 10 штук)
- Использует GNews API
- Структура ответа:

```json
{
  "totalArticles": "общее кол-во статей",
  "category": "название категории",
  "articles": [ 
    { 
      "title", 
      "description", 
      "url", 
      "image"
    },
    "...",
    "...",
  ]
}
```

- Если категория не поддерживается — вернуть ошибку `400`.

---

### 3. Реализовать валидацию категории

- Создать функцию `isValidCategory(category)`, которая проверяет, что категория содержится в списке поддерживаемых GNews.
- Поддерживаемые категории:
  ```js
  ['business','entertainment','general','health','science','sports','technology']
  ```
- Функцию вынести в `utils.js`

---

### 4. Реализовать middleware проверки токена

- Проверять наличие заголовка `Authorization: Bearer <token>`
- Токен сравнивается со значением из `.env` (`AUTH_TOKEN`)
- При ошибке возвращать:
  - `401` если заголовок отсутствует
  - `403` если токен неверный

---

### 5. Перевод заголовков и описаний на русский язык

- Расширить `GET /news/:category`
- Добавить опциональный query-параметр `lang`. Например: `lang=ru`
- Если указан — возвращать `title` и `description` статей на указанный язык
- Использовать библиотеку `translate` из `translate.js`
- Варианты `lang`:

| Язык      | Код |
|-----------|-----|
| Chinese   | zh  |
| French    | fr  |
| German    | de  |
| Italian   | it  |
| Russian   | ru  |
