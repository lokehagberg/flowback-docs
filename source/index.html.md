---
title: Flowback API

language_tabs: # must be one of https://git.io/vQNgJ
  - shell

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

```shell
curl "api_endpoint_here" \
-H "Authorization: your_token_here"
``` 

> Make sure to replace `your_token_here` with your API key.

Flowback uses API keys to allow access to the API. You can get an API key using the [Login API endpoint](#register).

Flowback expects for the API key to be included in all API requests to the server (aside from registration) 
in a header that looks like the following:

`Authorization: your_token_here`

<aside class="notice">
You must replace <code>your_token_here</code> with your personal API key.
</aside>

# Pagination

All of the list API endpoints offer pagination support. This feature can be accessed using the query params
of the associated API endpoint.

### Query Parameters

| Parameter                | Default | Description                                                                                     |
|--------------------------|---------|-------------------------------------------------------------------------------------------------|
| `page` *int, optional*   | 1       | The page of the paginator.                                                                      |
| `limit` *int, optional*  | Varies  | The maximum amount of entries allowed per page.                                                 |
| `offset` *int, optional* | 0       | The offset of the paginator, eg. an offset of 2 means the page starts 2 entries ahead per page. |

> Every List API will have these parameters in its outermost json object:

```json
{
  "count": 19,
  "next": "https://example.com/api_endpoint?page=2&limit=1&offset=0",
  "previous": false,
  "total_page": 2,
  ...
}
```

# User

## Register

```shell
curl "https://flowback.org/register" \
-H "Authorization: your_token_here" \
-d "username=flowback" \
-d "email=flowback@example.com"
```

This endpoint registers an user to the site. An activation email will be sent to the user containing the 
`verification_code`, which will be needed for the [next step of the registration.](#verify-register)

### HTTP Request

`POST /register`

### Body Parameters

| Parameter           | Notes |
|---------------------|-------|
| `username` *string* |       |
| `email` *string*    |       |

## Verify Register

```shell
curl "https://flowback.org/register/verify" \
-H "Authorization: your_token_here" \
-d "verification_code=your_verification_code" \
-d "password=your_password"
```

This endpoint completes an user registration, and creates a password associated to the user.



### HTTP Request

`POST /register/verify`

### Body Parameters

| Parameter                    | Notes                                             |
|------------------------------|---------------------------------------------------|
| `verification_code` *string* | Can be obtained trough the `/registrer` endpoint. |
| `password` *string*          |                                                   |

<aside class="warning">Make sure that the user doesn't do any typing mistakes with the password by e.g.
requiring the user to type their password twice.</aside>

## List Users

```shell
curl "https://flowback.org/users" \
-H "Authorization: your_token_here"
```

> The above command returns JSON structured like this:

```json
{
  "data": [
    {
      "id": 1,
      "username": "flowback",
      "profile_image": "https://example.com/media/image.png"
    },
    ...
  ]
}
```

This endpoint gets a list of users.

### HTTP Request

`GET /users`

### URL Parameters

| Parameter                     | Notes |
|-------------------------------|-------|
| `username` *string, optional* |       |

## Get User

```shell
curl "https://flowback.org/user/<id>" \
-H "Authorization: your_token_here"
```

> The above command returns JSON structured like this:

```json
{
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

| Parameter            | Notes                                   |
|----------------------|-----------------------------------------|
| `id` *int, optional* | Leave it blank to get the current user. |



## Update User

```shell
curl "https://flowback.org/user/update" \
-H "Authorization: your_token_here" \
-d "username=flowback" \
-F "profile_image=@profile_image.png" \
-F "banner_image=@banner_image.png" \
-d "bio=Welcome to my profile!"
-d "website=https://example.com/"
```

This endpoint updates an user.

### HTTP Request

`GET /user`

### URL Parameters

| Parameter                        | Notes |
|----------------------------------|-------|
| `username` *string, optional*    |       |
| `profile_image` *file, optional* |       |
| `banner_image` *file, optional*  |       |
| `bio` *int, optional*            |       |
| `website` *int, optional*        |       |
