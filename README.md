![Workflow](https://github.com/APanov13/yamdb_final/actions/workflows/yamdb_workflow.yml/badge.svg)

IP сервера:
http://130.193.37.29/api/v1/


# Описание проекта YaMDb  

Проект YaMDb собирает отзывы (Review) пользователей на произведения (Titles). Произведения делятся на категории: «Книги», «Фильмы», «Музыка». Список категорий (Category) может быть расширен администратором.  

Сами произведения в YaMDb не хранятся, здесь нельзя посмотреть фильм или послушать музыку.
В каждой категории есть произведения: книги, фильмы или музыка. Например, в категории «Книги» могут быть произведения «Винни-Пух и все-все-все» и «Марсианские хроники», а в категории «Музыка» — песня «Давеча» группы «Насекомые» и вторая сюита Баха.    

Произведению может быть присвоен жанр (Genre) из списка предустановленных (например, «Сказка», «Рок» или «Артхаус»). Новые жанры может создавать только администратор.  

Пользователи оставляют к произведениям текстовые отзывы (Review) и ставят произведению оценку в диапазоне от одного до десяти (целое число); из пользовательских оценок формируется усреднённая оценка произведения — рейтинг (целое число). На одно произведение пользователь может оставить только один отзыв.  

### Технологии испльзуемые в проекте 
* Python 3.7
* Django REST framework 3.12.4
* Git
* SQLite3  

### Запуск проекта  
Cоздать и активировать виртуальное окружение:  

`python -m venv venv  `
    
`source venv/bin/activate`  

Установить зависимости из файла requirements.txt:  

`pip install -r requirements.txt`  

В главной папке проекта (api_yamdb) выполнить миграции:  

`python manage.py migrate`  

Запустить проект:  

`python manage.py runserver`  

### Документация к API  

Документация доступна по адресу: [http://127.0.0.1:8000/redoc/](http://127.0.0.1:8000/redoc/) 
     
### Пользовательские роли
* Аноним — может просматривать описания произведений, читать отзывы и комментарии.  
* Аутентифицированный пользователь (user) — может читать всё, как и Аноним, может публиковать отзывы и ставить оценки произведениям (фильмам/книгам/песенкам), может комментировать отзывы; может редактировать и удалять свои отзывы и комментарии, редактировать свои оценки произведений. Эта роль присваивается по умолчанию каждому новому пользователю.  
* Модератор (moderator) — те же права, что и у Аутентифицированного пользователя, плюс право удалять и редактировать любые отзывы и комментарии.  
* Администратор (admin) — полные права на управление всем контентом проекта. Может создавать и удалять произведения, категории и жанры. Может назначать роли пользователям.  
* Суперюзер Django должен всегда обладать правами администратора, пользователя с правами admin. Даже если изменить пользовательскую роль суперюзера — это не лишит его прав администратора. Суперюзер — всегда администратор, но администратор — не обязательно суперюзер.  

### Самостоятельная регистрация новых пользователей
1. Пользователь отправляет POST-запрос с параметрами email и username на эндпоинт `/api/v1/auth/signup/`.
2. Сервис YaMDB отправляет письмо с кодом подтверждения (`confirmation_code`) на указанный адрес email.
3. Пользователь отправляет POST-запрос с параметрами `username` и `confirmation_code` на эндпоинт `/api/v1/auth/token/`, в ответе на запрос ему приходит `token` (JWT-токен).  

В результате пользователь получает токен и может работать с API проекта, отправляя этот токен с каждым запросом.
После регистрации и получения токена пользователь может отправить PATCH-запрос на эндпоинт `/api/v1/users/me/` и заполнить поля в своём профайле (описание полей — в документации). 

### Создание пользователя администратором
Пользователя может создать администратор — через админ-зону сайта или через POST-запрос на специальный эндпоинт `api/v1/users/` (описание полей запроса для этого случая — в документации). В этот момент письмо с кодом подтверждения пользователю отправлять не нужно.  

После этого пользователь должен самостоятельно отправить свой `email` и `username` на эндпоинт `/api/v1/auth/signup/` , в ответ ему должно прийти письмо с кодом подтверждения.  

Далее пользователь отправляет POST-запрос с параметрами `username` и `confirmation_code` на эндпоинт `/api/v1/auth/token/`, в ответе на запрос ему приходит `token` (JWT-токен), как и при самостоятельной регистрации.  

### Примеры запросов и ответов API  

#### Добавление произведения  

  
  `POST /api/v1/titles/`
##### Ответ API:

```json
{
"name": "string",
"year": 0,
"description": "string",
"genre": [
"string"
],
"category": "string"
}
```
 
#### Получение информации о произведении  

  
  `GET /api/v1/titles/{titles_id}/`
##### Ответ API:

```json
{
"id": 0,
"name": "string",
"year": 0,
"rating": 0,
"description": "string",
"genre": [
{
"name": "string",
"slug": "string"
}
],
"category": {
"name": "string",
"slug": "string"
}
}
```
#### Частичное обновление информации о произведении  

  
  `PATCH /api/v1/titles/{titles_id}/`
##### Ответ API:

```json
{
"name": "string",
"year": 0,
"description": "string",
"genre": [
"string"
],
"category": "string"
}
```
#### Удаление произведения 

  
  `DELETE /api/v1/titles/{titles_id}/`

 

### Разработчики  
[Саша Смирнов](https://github.com/crush-on-anechka) - написал всю часть, касающуюся управления пользователями (Auth и Users): систему регистрации и аутентификации, права доступа, работу с токеном, систему подтверждения через e-mail. 

[Антон Панов](https://github.com/APanov13)  - написал категории (Categories), жанры (Genres) и произведения (Titles): модели, представления и эндпойнты для них. 

[Артем Фирсов](https://github.com/apfirsov) (Тимлид) - написал систему с отзывами (Review) и комментариями (Comments): описал модели, представления, настроил эндпойнты, определил права доступа для запросов. Реализовал рейтинги произведений и механизм импорта данных из csv файлов.
