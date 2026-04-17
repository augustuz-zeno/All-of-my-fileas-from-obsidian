[[0 HTB HTTP]]
# Задание из [[6 GET]]
### The exercise above seems to be broken, as it returns incorrect results. Use the browser devtools to see what is the request it is sending when we search, and use cURL to search for 'flag' and obtain the flag.

> [!danger]
> Authenticate to 154.57.164.79 , with user "admin" and password "admin"

Вариант 1:
```bash
curl --compressed -H "Authorization: Basic YWRtaW46YWRtaW4=" "http://154.57.164.79:31534/search.php?search=flag"
```
### Разбор флагов:

- **`curl`**: Инструмент для передачи данных с серверов и на них. В отличие от браузера, он отправляет только то, что вы укажете.
    
- **`--compressed`**: Говорит серверу: «Я умею читать сжатые данные (gzip)». Если сервер присылает сжатый ответ, cURL сам его распакует в читаемый текст. Без этого флага вы бы увидели в консоли набор случайных символов.
    
- **`-H "Authorization: Basic ..."`**: Самая важная часть. Это заголовок авторизации. Сайт защищен паролем (admin:admin), и без этой строки сервер выдавал бы ошибку **401 Unauthorized** или **Access Denied**.
    
- **`"URL?search=flag"`**: Параметр `?search=flag` в конце URL — это GET-запрос. Мы напрямую сказали скрипту `search.php`, что именно мы ищем.
Вариант 2:
```bash
curl -u admin:admin "http://154.57.164.79:31534/search.php?search=flag"
```
### Разбор флагов:
- **`-u` (User)**: Флаг для **Basic Authentication**. cURL сам кодирует `логин:пароль` в Base64 и подставляет нужный заголовок. Это быстрее и меньше шанс ошибиться в строке кода.
# Задание из [[7 POST]]

### Obtain a session cookie through a valid login, and then use the cookie with cURL to search for the flag through a JSON POST request to '/search.php'

> [!danger]
> Authenticate to 154.57.164.64 , with user "admin" and password "admin"

## Цель задания

Авторизоваться в системе, получить идентификатор сессии (cookie) и использовать его для выполнения защищенного запроса к API для поиска флага.

---

## Этап 1: Аутентификация и получение Cookie

Для начала необходимо отправить учетные данные на сервер. Использование ключа `-i` в cURL позволяет увидеть заголовки (headers) ответа, в которых передается кука.

**Команда:**

Bash

```
curl -X POST -d "username=admin&password=admin" http://154.57.164.64:31918/ -i
```

**Ключевой элемент ответа:** `Set-Cookie: PHPSESSID=aq7nlle0nn2kvarf85ags2om2d; path=/` Сервер подтвердил валидность данных и присвоил уникальный ID сессии.

---

## Этап 2: Поиск флага через JSON POST

Имея активную сессию, отправляем запрос к эндпоинту `/search.php`. Условие требует формат **JSON**, поэтому необходимо явно указать заголовок `Content-Type`.

**Команда:**
```Bash
curl -X POST http://154.57.164.64:31918/search.php -H "Cookie: PHPSESSID=aq7nlle0nn2kvarf85ags2om2d" -H "Content-Type: application/json" -d "{\"search\":\"flag\"}"
```

**Разбор параметров:**

- `-H "Cookie: ..."` — передача сессии для прохождения авторизации.
    
- `-H "Content-Type: application/json"` — информирование сервера, что данные передаются в формате JSON.
    
- `-d "{\"search\":\"flag\"}"` — тело запроса (экранирование кавычек обязательно в Windows CMD).

---
## Результат

Сервер возвращает JSON-массив с найденным значением: **`HTB{p0$t_r3p34t3r}`**

---
### Пояснение механизмов

1. **Stateful Auth**: Сервер запоминает состояние клиента через `PHPSESSID`. Без передачи этой куки во втором запросе сервер бы «забыл» нас и выдал ошибку доступа.
    
2. **JSON Payload**: В отличие от обычных форм (application/x-www-form-urlencoded), JSON требует строгого синтаксиса и соответствующих HTTP-заголовков для корректной обработки на стороне бэкенда.
# Задание из [[8 CRUD API]]
### First, try to update any city's name to be 'flag'. Then, delete any city. Once done, search for a city named 'flag' to get the flag.
### 1. Что такое CRUD?

Это аббревиатура четырех основных операций с данными:

- **C**reate (Создать) — `POST`
    
- **R**ead (Прочитать) — `GET`
    
- **U**pdate (Обновить) — `PUT` / `PATCH`
    
- **D**elete (Удалить) — `DELETE`
### 2. Структура эндпоинта (URL)

В данном задании API реализовано через **REST-подобные пути**. Это значит, что информация о том, _что_ мы меняем, передается прямо в адресе: `http://IP:PORT/api.php/название_таблицы/имя_объекта`

- **api.php** — точка входа (скрипт, который обрабатывает запросы).
    
- **/city/** — указывает серверу, что мы работаем с таблицей городов.
    
- **/London** — конкретный идентификатор (ключ) записи, которую мы хотим изменить или удалить.
### 3. Разбор команд

#### Обновление (PUT)
```bash
curl -X PUT http://.../api.php/city/London -d "{\"city_name\":\"flag\", \"country_name\":\"HTB\"}"
```

- **Что произошло:** Мы сказали серверу: "Найди в таблице `city` запись `London` и замени её данные на те, что в `-d`".
    
- **Результат:** Имя города в базе стало `flag`.
#### Удаление (DELETE)
```bash
curl -X DELETE http://.../api.php/city/Birmingham
```

- **Что произошло:** Метод `DELETE` сообщает серверу, что запись `Birmingham` нужно полностью стереть.
#### Чтение (GET)
```shell
curl -s http://.../api.php/city/flag
```

- **Что произошло:** Мы запросили данные по ключу `flag`. Поскольку мы до этого переименовали London в flag, сервер нашел эту запись и выдал JSON, в котором вместо страны был спрятан «флаг» задания.