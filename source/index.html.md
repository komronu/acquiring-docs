---
title: Алиф Эквайринг

language_tabs:
  - http

includes:
  - checkout
  - card
  - debit
  - p2p
  - recurring
---

# Спецификация API

## Запросы и ответы

API представляет собой HTTP-based RPC (remote procedure call). Название метода (процедуры) передается в URL. Каждый HTTP-запрос должен иметь метод `POST`. В качестве типа тела запроса используется JSON.


Каждый HTTP ответ имеет статус `200 - OK`. В корне тела ответа есть поле `code`, которое показывает с каким статусом был завершен запрос. Со всеми кодами можно ознакомиться [ниже](/#f886d8017b).
В теле неуспешного ответа есть поле `message`, которое дает небольшое описание об ошибке.


> **Пример запроса**:
```
POST - /checkout.payment.create
```

```json
{
    "session_ref": "63c99faa-d3b2-41ef-a8d7-1e13f109d913",
    "pan": "8600140214357387",
    "exp": "1024"
}
```

> **Успешный ответ**:

```json
{
    "code": 1000,
    "data": {
        "msg": "sup"
    }
}
```

> **Неуспешный ответ**:

```json
{
    "code": 1002,
    "message": "unauthorized"
}
```

## Коды ответов
- `1000` - OK
- `1001` - Bad request
- `1002` - Unauthorized
- `1003` - Internal error
- `1004` - Not found
- `1005` - Unprocessable entity

## Пагинация

В запросах, поддерживающих пагинацию (для получения среза списка) в корень тела запроса добавляется два атрибута:

> **Пример запроса:**

```json
{
  "page": 1,
  "per_page": 3
}
```

Описание атрибутов:

| Ключ     | Описание                                                                     |
| -------- | ---------------------------------------------------------------------------- |
| page     | Номер среза. Необязательный атрибут, по умолчанию равен 1                    |
| per_page | Количество элементов в срезе. Необязательный атрибут, по умолчанию равен 100 |

## Авторизация запросов
Каждому мерчанту выданы секретный ключ и токен. В то время как для вызова некоторых методов будет достаточно только токена, для вызова других также нужна цифровая подпись — HMAC SHA-256 хэш, созданный из тела запроса и секретного ключа.

> **Псевдокод получения сигнатуры:**

```python
json_body = '{"msg": "ping"}'
secret_key = 'your_secret_key'

signature = hmacsha256(msg=json_body, key=secret_key)
```

Для токена и сигнатуры выделены HTTP-заголовки `token` и `signature` соответственно.


<!-- 
The Monzo API is designed to be a predictable and intuitive interface for interacting with users' accounts. We offer both a REST API and webhooks.

The [Developers category](https://community.monzo.com/c/developers) on our forum is the place to get help with our API, discuss ideas, and show off what you build.

<!-- <aside class="warning">
    <strong>The Monzo Developer API is not suitable for building public applications.</strong><br>
    You may only connect to your own account or those of a small set of users you explicitly allow. Please read our <a href="https://monzo.com/blog/2017/05/11/api-update/">blog post</a> for more detail.
</aside> -->

<!-- <aside class="info">
    <strong>Upcoming Strong Customer Authentication changes</strong><br>
New rules for all banks, including Monzo, mean we’ll start increasing security around third party integrations with customers' accounts. 
Please see the <a href=#authentication>Authentication</a> and <a href=#list-transactions>Transactions</a> sections for more details.
</aside> -->

<!-- <aside class="notice">
    For firms authorised as <a href="https://www.fca.org.uk/account-information-service-ais-payment-initiation-service-pis">Account Information Service Providers</a> under PSD2 we offer an <a href=#account-information-services-api>AISP API</a>.
    We also offer an <a href=#payment-initiation-services-api>API for authorised PISPs</a> and <a href=#confirmation-of-funds-api>CBPIIs</a>.
</aside> --> 
