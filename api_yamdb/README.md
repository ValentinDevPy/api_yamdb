# YaMDb api

## Самая актуальная версия API всемирно известного YaMDb.

YaTube API это RESTful API, позволяющий создавать и редактировать отзывы на различные произведения, оценивать их и
оставлять комментарии к отзывам. В основе проекта лежат Django и Django REST Framework.

## Особенности

- Пишите свои ревью и ставьте оценки произведениям.
- Авторизация происходит по токену. 
- Токен можно получить введя код подтверждения из электронного письма.
- При просмотре информации о каком-либо произведении будет выведена его средняя оценка.
- Для авторизованных пользователей присутствует возможность комментировать отзывы.

## Технологии

- [Django](https://github.com/django/django) - фреймворк, который включает в себя все необходимое для быстрой разработки
  различных веб-сервисовю
- [Django REST Framework](https://www.django-rest-framework.org/) - фреймворк, расширяющий возможности Django и
  позволяющий быстро писать RESTful API для Django-проектов.

Конечно же YaTube API это ПО с открытым исходным кодом
и [публичным репозиторием](https://github.com/krapiwin/api_yamdb)
на GitHub.

## Начало работы

### Как запустить проект:

Клонировать репозиторий и перейти в него в командной строке:

```
git clone https://github.com/krapiwin/api_yamdb
```

```
cd api_yamdb

```

Cоздать и активировать виртуальное окружение:

```
python3 -m venv venv
```

```
source env/bin/activate
```

Установить зависимости из файла requirements.txt:

```
python3 -m pip install --upgrade pip
```

```
pip install -r requirements.txt
```

Модуль manage.py расположен в:

```
api_final_yatube/api_yamdb/api_yamdb
```

Выполнить миграции:

```
python3 manage.py migrate
```

Запустить проект:

```
python3 manage.py runserver
```

API будет доступен по адресу:

```
http://127.0.0.1:8000/api/v1/
```

## Пользовательские роли
```Аноним — может просматривать описания произведений, читать отзывы и комментарии.```

```Аутентифицированный пользователь (user) — может, как и Аноним, читать всё, дополнительно он может публиковать отзывы и ставить оценку произведениям (фильмам/книгам/песенкам), может комментировать чужие отзывы;может редактировать и удалять свои отзывы и комментарии. Эта роль присваивается по умолчанию каждому новому пользователю.```

```Модератор (moderator) — те же права, что и у Аутентифицированного пользователя плюс право удалять любые отзывы и комментарии.```

```Администратор (admin) — полные права на управление всем контентом проекта. Может создавать и удалять произведения, категории и жанры. Может назначать роли пользователям.```

```Суперюзер Django — обладет правами администратора (admin)```

## Примеры работы с API:

Получение списка всех категорий и создание новой(доступно только для роли 'Administrator') происходит на эндпоинте:

```sh
http://127.0.0.1:8000/api/v1/categories/
```

Пример ответа при GET запросе с параметрами offset и count:
```
[
  {
    "count": 0,
    "next": "string",
    "previous": "string",
    "results": [
      {
        "name": "string",
        "slug": "string"
      }
    ]
  }
]

```

Редактирование категорий запрещено. Удаление категории происходит на эндпоинте:

```sh
http://127.0.0.1:8000/api/v1/categories/{slug}/
```


Получение списка жанров и создание нового(доступно только для роли 'Administrator') происходит на эндпоинте:

```sh
http://127.0.0.1:8000/api/v1/genres/
```
Пример ответа:
```
[
  {
    "count": 0,
    "next": "string",
    "previous": "string",
    "results": [
      {
        "name": "string",
        "slug": "string"
      }
    ]
  }
]
```

Получение конкретного жанра, а так же его редактирование запрещено. А удаление происходит на эндпоинте:

```sh
http://127.0.0.1:8000/api/v1/genres/{slug}/
```

Получение списка произведений и создание нового происходит на эндпоинте:

```sh
http://127.0.0.1:8000/api/v1/titles/
```

Пример ответа:
```
[
  {
    "id": 0,
    "title": "string",
    "slug": "string",
    "description": "string"
  }
]
```

Для конкретной группы доступен только метод GET(id - первичный ключ таблицы Groups) на эндпоинте:

```sh
http://127.0.0.1:8000/api/v1/groups/{id}/
```

Пример ответа:
```
{
  "id": 0,
  "title": "string",
  "slug": "string",
  "description": "string"
}
```

Следующий эндпоинт возвращает все подписки пользователя, сделавшего запрос. Анонимные запросы запрещены (метод GET):

```sh
http://127.0.0.1:8000/api/v1/follow/
```

Пример ответа:
```
[
  {
    "user": "string",
    "following": "string"
  }
]
```

Подписка пользователя от имени которого сделан запрос на пользователя переданного в теле запроса. Анонимные запросы
запрещены (метод POST):

```sh
http://127.0.0.1:8000/api/v1/follow/
```

Создание, обновление и верификация токена происходит на следущих эндпоинтах:

```sh
http://127.0.0.1:8000/api/v1/titles/{title_id}/reviews/{review_id}/comments/
```

Пример ответа:
```
[
  {
    "count": 0,
    "next": "string",
    "previous": "string",
    "results": [
      {
        "id": 0,
        "text": "string",
        "author": "string",
        "pub_date": "2019-08-24T14:15:22Z"
      }
    ]
  }
]
```

Получение конкретного комментария, а так же его редактирование и удаление
(доступно только для автора комментария, модератора или администратора) происходит на эндпоинте:

```sh
http://127.0.0.1:8000/api/v1/titles/{title_id}/reviews/{review_id}/comments/{comment_id}/
```
Пример ответа: 
```
{
  "id": 0,
  "text": "string",
  "author": "string",
  "pub_date": "2019-08-24T14:15:22Z"
}
```

Регистрация пользователя и получение jwt токена происходит на следующих эндпоинтах:

```sh
http://127.0.0.1:8000/api/v1/auth/signup/
http://127.0.0.1:8000/api/v1/auth/token/
```

## Лицензия

**MIT**

**Free Software, Hell Yeah!**