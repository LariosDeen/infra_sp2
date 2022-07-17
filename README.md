![](https://img.shields.io/badge/Python-3.7.slim-red) 
![](https://img.shields.io/badge/Django-2.2.16-yellow)
![](https://img.shields.io/badge/DjangoRestFramework-3.12.4-blue)
![](https://img.shields.io/badge/DockerCompose-3.8-green)
![](https://img.shields.io/badge/Nginx-1.21.3.alpine-violet)
![](https://img.shields.io/badge/Postgres-13.0.alpine-white)

# infra_sp2

### Описание

Данный проект является проектом учебного курса Яндекс Практикум по
специальности Python-разработчик.
* Проект собирает отзывы (Review) пользователей на произведения (Titles). 
* Произведения делятся на категории: «Книги», «Фильмы», «Музыка». 
* Список категорий (Category) может быть расширен администратором (например, 
можно добавить категорию «Изобразительное искусство» или «Ювелирка»).
* Сами произведения в YaMDb не хранятся, здесь нельзя посмотреть фильм или 
послушать музыку.
* В каждой категории есть произведения: книги, фильмы или музыка. 
Например, в категории «Книги» могут быть произведения «Винни-Пух и 
все-все-все» и «Марсианские хроники», а в категории «Музыка» — песня «Давеча» 
группы «Насекомые» и вторая сюита Баха.
* Произведению может быть присвоен жанр (Genre) из списка предустановленных 
(например, «Сказка», «Рок» или «Артхаус»). Новые жанры может создавать только 
администратор.
* Благодарные или возмущённые пользователи оставляют к произведениям текстовые 
отзывы (Review) и ставят произведению оценку в диапазоне от одного до десяти 
(целое число); из пользовательских оценок формируется усреднённая оценка 
произведения — рейтинг (целое число). На одно произведение пользователь может 
оставить только один отзыв.

### Как запустить проект на компьютере

:black_small_square: Скопируйте проект на свой компьютер:

```sh
git clone https://github.com/LariosDeen/api_yamdb
```

:black_small_square: Создайте файл infra_sp2/infra/.env  
:black_small_square: Заполните его данными по образцу:

```sh
SECRET_KEY_SETTINGS = 'p&l%385148kslhtyn^55a1)ilz@4zqj=rq&agdol^##zgl9(vs'
DB_ENGINE=django.db.backends.postgresql # указываем, что работаем с postgresql
DB_NAME=postgres # имя базы данных
POSTGRES_USER=postgres # логин для подключения к базе данных
POSTGRES_PASSWORD=postgres # пароль для подключения к БД (установите свой)
DB_HOST=db # название сервиса (контейнера)
DB_PORT=5432 # порт для подключения к БД
```

:black_small_square: Установите Docker соответствующий вашей операционной системе.  
:black_small_square: Запустите Docker  
:black_small_square: Откройте командную строку вашей операционной системы.  
:black_small_square: Перейдите в директорию проекта infra_sp2/infra/  

#### В командной строке выполните команды:

:black_small_square: Команда для сборки образов, скачивания образов с docker hub и запуска 
контейнеров(выполнение инструкций из файла docker-compose.yaml)
```
docker-compose up -d --build
```

:black_small_square: Выполнить миграции 
```
docker-compose exec web python manage.py migrate
```

:black_small_square: Копировать статические файлы в директорию static, медиа 
файлы в директорию media внутри рабочей директории запущенного контейнера web
```
docker-compose exec web python manage.py collectstatic --no-input
```

:black_small_square: Можно заполнить базу данными из csv файлов директории 
static/data используя management command
```
docker-compose exec web python manage.py fill_db
```

:black_small_square: Можно восстановить базу данных из файла 
infra_sp2/infra/nginx/fixtures.json командой
```
docker-compose exec web python manage.py loaddata fixtures.json
```

:black_small_square: Создать суперпользователя
```
docker-compose exec web python manage.py createsuperuser
```

:black_small_square: Остановить все контейнеры можно командой
```
docker-compose stop
```
:black_small_square: Остановить контейнеры и удалить все остановленные 
контейнеры и зависимости можно командой
```
docker-compose down -v
```



### Примеры

##### Пользователь аутентифицируется посредвстом сервиса Simple JWT.  
Получите код подтверждения регистрации.  
Отправьте POST запрос с именем пользователя и e-mail на эндпойнт:
```
http://localhost/api/v1/auth/signup/
```

POST request:
```
{
    "email": "terminator@skynet.com",
    "username": "terminator"
}
```

Response:
```
{
    "email": "terminator@skynet.com",
    "username": "terminator"
}
```

##### Получите токен для доступа к функциям сервиса проекта. 
Отправьте POST запрос с именем пользователя и полученным по e-mail 
кодом подтвреждения на эндпойнт:
```
http://localhost/api/v1/auth/token/
```

POST request:
```
{
    "username": "terminator",
    "confirmation_code": "1234"
}
```

Response:
```
{
    "refresh": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0b2tlb90eXBlIjoicmVmcmVzaCIsImV4cCI6MTY1OTc4OTAwOSwiaWF0IjoxNjU4MDYxMDA5LCJqdGkiOiJkNTUyNTJlODQ0OGI0MDExYjFjOGYwZDYxOGU2ZjAxZCIsInVzZXJfaWQiOjF9.IVjgYCbZiQ_kdraTYIz4VdYYpZoh7kTMxpmjjJ1tkIg",
    "access": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0b2tlb90eXBlIjoiYWNjZXNzIiwiZXhwIjoxNjU4OTI1MDA5LCJpYXQiOjE2NTgwNjEwMDksImp0aSI6ImIzZDViMmY2YjExZjQxMTM4NTk1NWVmMzg5NmZmM2JkIiwidXNlcl9pZCI6MX0.dEfpwO3ZBA62R6lH6ybHx3KxCZU9PgQCoXvaEsl5UyI"
}
```

##### При желании можно заполнить пустые поля в своем профайле, отправив PATCH запрос на эндпойнт:
```
http://localhost/api/v1/users/me/
```
PATCH request:
```
{
    "username": "terminator",
    "email": "terminator@skynet.ru",
    "role": "user",
    "bio": "My biography.",
    "first_name": "Arnold",
    "last_name": "Schwarzenegger"
}
```

Response:
```
{
    "username": "terminator",
    "email": "terminator@skynet.ru",
    "role": "user",
    "bio": "My biography.",
    "first_name": "Arnold",
    "last_name": "Schwarzenegger"
}
```

##### Получение списка произведений GET запрос:
```
http://localhost/api/v1/titles/
```

Response:
```
{
    "count": 32,
    "next": "http://web:8000/api/v1/titles/?page=2",
    "previous": null,
    "results": [
        {
            "id": 3,
            "rating": 7,
            "genre": [
                {
                    "name": "Драма",
                    "slug": "drama"
                }
            ],
            "category": {
                "name": "Фильм",
                "slug": "movie"
            },
            "name": "12 разгневанных мужчин",
            "year": 1957,
            "description": null
        },
        {
            "id": 30,
            "rating": 10,
            "genre": [
                {
                    "name": "Рок",
                    "slug": "rock"
                }
            ],
            "category": {
                "name": "Музыка",
                "slug": "music"
            },
            "name": "Deep Purple — Smoke on the Water",
            "year": 1971,
            "description": null
        },
```


##### Получение информации о произведении GET запрос:
```
http://localhost/api/v1/titles/{titles_id}/
```

Response:
```
{
    "id": 5,
    "rating": 5,
    "genre": [
        {
            "name": "Комедия",
            "slug": "comedy"
        },
        {
            "name": "Детектив",
            "slug": "detective"
        },
        {
            "name": "Триллер",
            "slug": "thriller"
        }
    ],
    "category": {
        "name": "Фильм",
        "slug": "movie"
    },
    "name": "Криминальное чтиво",
    "year": 1994,
    "description": null
}
```

##### Создайте отзыв о произведении POST запрос:
```
http://localhost/api/v1/titles/{title_id}/reviews/
```
POST request:
```
{
    "genre": ["ballad"],
    "category": "book",
    "name": "Some name.",
    "year": 1999
}
```
Response:
```
{
    "id": 33,
    "genre": [
        "ballad"
    ],
    "category": "book",
    "name": "Some name.",
    "year": 1999,
    "description": null,
    "rating": null
}
```

##### Все доступные команды по использованию сервиса можно посмотреть на URL:

```
http://localhost/redoc/
```

##### Теги для postman:  

Body
```
raw
```
```
JSON
```

Headers
```
Authorization
```
```
Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0b2tlbl90eXBlIjoiYWNjZXNzIiwiZ
XhwIjoxNjU4OTMxNDA1LCJpYXQiOjE2NTgwNjc0MDUsImp0aSI6IjkzNDNlMzgzYmU1ZDRlMzg4ZD
A3OWI0ZTVlNTUzYzVhIiwidXNlcl9pZCI6MX0.sj5mOT2DrIX9TQ
```

### В проекте использованы технологии:

* Python
* Django
* Django REST Framework
* Simple JWT
* Docker
* Postgres
* Gunicorn
* Nginx

Проект выполнил студент 31 когорты Яндекс Практикума  
Лариос Димитри  
https://github.com/LariosDeen  
https://t.me/dimilari


 