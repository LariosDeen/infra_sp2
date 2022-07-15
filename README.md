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
```sh
docker-compose up -d --build
```
:black_small_square: Выполнить миграции 
```sh
docker-compose exec web python manage.py migrate
```
:black_small_square: Создать суперпользователя
```sh
docker-compose exec web python manage.py createsuperuser
```
:black_small_square: Копировать статические файлы в директорию static, медиа файлы в директорию 
media внутри рабочей директории запущенного контейнера web
```sh
docker-compose exec web python manage.py collectstatic --no-input
```
:black_small_square: Восстановить базу данных можно из файла infra_sp2/infra/nginx/fixtures.json командой
```sh
docker-compose exec web python manage.py loaddata fixtures.json
```
:black_small_square: Остановить все контейнеры можно командой
```sh
docker-compose stop
```
:black_small_square: Остановить контейнеры и удалить все остановленные контейнеры и зависимости 
можно командой
```sh
docker-compose down -v
```



### Примеры

Пользователь аутентифицируется посредвстом сервиса Simple JWT.  
Получите код подтверждения регистрации.  
Отправьте POST запрос с именем пользователя и e-mail на эндпойнт:

```
http://localhost/api/v1/auth/signup/
```

Получите токен для доступа к функциям сервиса проекта. 
Отправьте POST запрос с именем пользователя и полученным по e-mail 
кодом подтвреждения на эндпойнт:

```
http://localhost/api/v1/auth/token/
```

При желании можно заполнить пустые поля в своем профайле, отправив PATCH запрос 
на эндпойнт:

```
http://localhost/api/v1/users/me/
```

Получение списка произведений GET запрос:

```
http://localhost/api/v1/titles/
```

Получение информации о произведении GET запрос:

```
http://localhost/api/v1/titles/{titles_id}/
```

Создайте отзыв о произведении POST запрос:

```
http://localhost/api/v1/titles/{title_id}/reviews/
```

Все доступные команды по использованию сервиса можно посмотреть на URL:

```
http://localhost/redoc/
```

### В проекте использованы технологии:

* Python
* Django
* Django REST Framework
* Simple JWT
* Docker
* Postgre
* Gunicorn
* Nginx

Проект выполнил студент 31 когорты Яндекс Практикума  
Лариос Димитри  
https://github.com/LariosDeen  
https://t.me/dimilari


 