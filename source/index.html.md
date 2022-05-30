---
title: Flowback API

language_tabs: # must be one of https://git.io/vQNgJ
  - python

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true

code_clipboard: true

meta:
  - name: description
    content: Documentation for the Kittn API
---

# Introduction

Welcome to the Flowback API! You can see all the available endpoints to the API. 

We have language bindings in Shell, Ruby, Python, and JavaScript! You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

# Authentication

> To authorize, use this code:

```python
import requests

headers = dict(Authorization='your_token_here')

# For this demonstration, we'll use a get request.
api = requests.get('api_endpoint_here', headers=headers)
``` 

> Make sure to replace `your_token_here` with your API key.

Flowback uses API keys to allow access to the API. You can get an API key using the [Login API endpoint](#get-a-specific-kitten).

Flowback expects for the API key to be included in all API requests to the server (aside from registration) 
in a header that looks like the following:

`Authorization: your_token_here`

<aside class="notice">
You must replace <code>your_token_here</code> with your personal API key.
</aside>

# User

## Register

```python
import requests

headers = dict(Authorization='your_token_here')
data = dict(username='flowback', email='flowback@example.com')

api = requests.post('/register', headers=headers, data=data)
```

This endpoint registers an user to the site. An activation email will be sent to the user containing the 
`verification_code`, which will be needed for the [next step of the registration.](#verify-register)

### HTTP Request

`POST /register`

### Body Parameters

| Parameter | Description                                        |
|-----------|----------------------------------------------------|
| username  | The username of the new user. This must be unique. |
| email     | The email of the new user. This must be unique.    |

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Verify Register

```python
import requests

headers = dict(Authorization='your_token_here')
data = dict(verification_code='<UUID4 code>', password='<secret>')

api = requests.post('/register/verify', headers=headers, data=data)
```

This endpoint completes an user registration, and creates a password associated to the user.

<aside class="warning">Make sure that the user doesn't do any typing mistakes with the password by e.g.
requiring the user to type their password twice.</aside>

### HTTP Request

`POST /register/verify`

### Body Parameters

| Parameter                            | Description                                       |
|--------------------------------------|---------------------------------------------------|
| `verification_code` *string (UUID4)* | Can be obtained trough the `/registrer` endpoint. |
| `password` *string*                  | The password of the new user.                     |

## List Users

```python
import requests

headers = dict(Authorization='your_token_here')
data = dict(page=1, limit=1, offset=0)

api = requests.get('/users', headers=headers, params=data)
```

> The above command returns JSON structured like this:

```json
{
  "count": 19,
  "next": "https://example.com/users?page=2&limit=1&offset=0",
  "previous": false,
  "total_page": 2,
  "data": [
    {
      "id": 1,
      "username": "flowback",
      "profile_image": "https://example.com/media/image.png"
    }
  ]
}
```

This endpoint gets a list of users.

### HTTP Request

`GET /users`

### URL Parameters

| Parameter                     | Description                                                           |
|-------------------------------|-----------------------------------------------------------------------|
| `id` *int, optional*          | The ID of the user.                                                   |
| `username` *string, optional* | The username of the user.                                             |
| `page` *int, optional*        | The page of the paginator.                                            |
| `limit` *int, optional*       | The content limit per page of the paginator. Can't be higher than 50. |
| `offset` *int, optional*      | The content offset of the paginator.                                  |

## Get User

```python
import requests

headers = dict(Authorization='your_token_here') 

api = requests.get('/users', headers=headers)
```

> The above command returns JSON structured like this:

```json
{
  "count": 19,
  "next": "https://example.com/users?page=2&limit=1&offset=0",
  "previous": false,
  "total_page": 2,
  "data": [
    {
      "id": 1,
      "email": "flowback@example.com",
      "username": "flowback",
      "profile_image": "https://example.com/media/image.png",
      "banner_image": "https://example.com/media/banner.png",
      "bio": "Hello world!",
      "website": "https://example.com/"
    }
  ]
}
```

This endpoint gets an user.

### HTTP Request

`GET /user`

### URL Parameters

| Parameter                     | Description                                                 |
|-------------------------------|-------------------------------------------------------------|
| `id` *int, optional*          | The ID of the user. Leave it blank to get the current user. |
