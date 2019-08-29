---
date: 2019-08-29
title: Mobile App Use Case
categories:
  - Joomla REST API
tags:
  - Joomla
  - REST API
  - Mobile App
type: Document
showSidebar: true
published: true
nav_ordering: 5
pageTitle: "Mobile App Use Case"
permalink: joomla-rest-api/com-api-mobile-app-use-case.html
---

Let's consider you are building a mobile app with Joomla as backend for APIs.
You can use com_api, plg_api_users, and create your own API plugins as needed.

Considering that:
  - you have installed latest [com_api](https://github.com/techjoomla/com_api/releases/) and its latest [user plugin](https://github.com/techjoomla/plg_api_users/releases/) 
  - and you have built a mobile application which interacts with your website

## Steps to use com_api:
  - 1.1 From the mobile app, when user wants to login for very first time - user will enter username and password in the mobile application login form.
  - 1.2 Your mobile app will catch those credentials,
  - 1.3 and make an API call as below

*Example:*

**Method:** POST

**API URL:**

```
{{host}}/index.php?option=com_api&app=users&resource=login&format=raw
```

**HEADERS to be set:**

```
Content-Type: application/x-www-form-urlencoded
```

**POST DATA:**

```
username: user entered value
password: user entered value
```

**RESPONSE:**
If credentials are correct, you will get a response, which looks like


```
{
    "err_msg": "",
    "err_code": "",
    "response_id": 214,
    "api": "users.login",
    "version": "",
    "data": {
       "auth": "c8b16517a0a21c446f1ee9980944cd7e",
        "code": "200",
        "id": "653",
        "jwt": "eyJ0eXAiOiJKV1QiLCJhreebGciOiJIUzI1NiJ9.eyJpZCI6IjY1MyJ9.rere-ZAhpXYXvCHuZbqqTmtwjUvlv8ZnA2t-PxzI"
    }
}
```

## Steps to use com_api: (continued)
 - 2.1. Now, once you get the response to above, you get an auth key, which you can use for next API calls.
 - 2.2 Your mobile app should store the auth key lets say local storage or whatver way you prefe

```
"auth": "c8b16517a0a21c446f1ee9980944cd7e"
```

 - 3.0 Now for subsequent API calls you can pass this auth key in the header and still access APIs without passing username and password.


*Example:*

**Method:** GET

**API URL:**

```
{{host}}/index.php?option=com_api&app=users&resource=user&format=raw&id=619
```

**HEADERS to be set:**
```
Content-Type: application/x-www-form-urlencoded
Authorization: Bearer c8b16517a0a21c446f1ee9980944cd7e
```

**RESPONSE:**
You will get a response, which looks like

```
{
    "err_msg": "",
    "err_code": "",
    "response_id": 218,
    "api": "users.user",
    "version": "",
    "data": {
        "id": "619",
        "name": "asdasd adasda",
        "username": "asdadad",
        "email": "asdsd@asdasd.com",
        "block": "0",
        "sendEmail": "1",
        "registerDate": "2019-05-10 06:54:13",
        "lastvisitDate": "2019-08-27 12:44:44",
        "activation": "0",
        "params": "",
        "groups": {
            "8": "8"
        },
        "guest": 0
    }
}
```

If you set up SEF, and have created menu for com_api (with `api` as alias), you can use SEF URLs like below instead of the one used in above example

```
{{host}}/api/users/user/619
```
