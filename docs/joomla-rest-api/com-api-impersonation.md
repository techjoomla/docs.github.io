---
title:       Impersonating another user
description: Impersonating another user using REST API framework for Joomla (com_api)
path:        blob/master/docs/joomla-rest-api
source:      com-api-impersonation.md
hero:        Joomla REST API - Impersonating another user
date:        2019-04-13
categories:
  - Joomla REST API
tags:
  - Joomla
  - REST API
  - com_api
---


## Overview

Using the impersonation feature it is possible for a Super Admin user to impersonate another user. To use this feature, the token being used for an API call should be belonging to a Super Admin user (specifically, any user having the `core.admin` permission).

To impersonate another user, pass the header `X-Impersonate`. The value of the header is the user id of the user to impersonate eg: `X-Impersonate` : `42`.

It is also possible to use the username or email of the user eg: `X-Impersonate` : `email:user@email.com` or `X-Impersonate` : `username:ashwin`.

!!! danger "Security Note"
    When using this feature, we recommend the below steps to improve the security of your setup

    - Create a new token to be used only for impersonation instead of using an existing token.

    - Absolutely avoid using this token for client side requests. Use it only in server side communication.

## More details of how this works

Normally, the com_api framework sets the `JUser` object for any API call based on the token passed in the Authorization header. Plugins can then access the user object making the API call via `$this->plugin->get('user')`.

This feature adds support for a new `X-Impersonate` header which allows the API caller to set a different user than the one making the API call. The Impersonate header can accept either the id, username or email of the user to impersonate.

The Impersonate header cannot be used by all users, only by Super Users.

### Example

Consider the example below

| ID | Name | Email | API Token | Level |
| ---- | ---- | ---- | ---- | ---- |
| 20 | Rahul | rahul@mail.com | rrrrrr | Super User |
| 21 | Jaya | jaya@mail.com | jjjjjj | Registered |
| 22 | Kevin | kevin@mail.com | kkkkkkk | Registered |

**Case 1**
```
GET /jgive/campaign/619
"Authorization": "Bearer rrrrrr"
"X-Impersonate": "21"
```

``` bash
curl --location --request GET 'http://{{host}}/index.php?option=com_api&app=jgive&resource=campaign&id=619&format=raw' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--header 'Authorization: Bearer rrrrrr'
--header 'X-Impersonate: 21'
```

In this case the user object available to the campaign resource will be that of userid 21.

**Case 2**
```
GET /jgive/campaign/619
"Authorization": "Bearer jjjjjj"
"X-Impersonate": "22"
```

``` bash
curl --location --request GET 'http://{{host}}/index.php?option=com_api&app=jgive&resource=campaign&id=619&format=raw' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--header 'Authorization: Bearer jjjjjj'
--header 'X-Impersonate: 22'
```

This API call will return a 403 error since the user with token jjjjjj is not allowed to use impersonation.

**Case 3**
```
GET /jgive/campaign/619
"Authorization": "Bearer rrrrrr"
"X-Impersonate": "email:kevin@mail.com"
```

``` bash
curl --location --request GET 'http://{{host}}/index.php?option=com_api&app=jgive&resource=campaign&id=619&format=raw' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--header 'Authorization: Bearer rrrrrr'
--header 'X-Impersonate: email:kevin@mail.com'
```

In this case the user object available to the campaign resource will be that of userid 22 i.e. the user with the email kevin@mail.com

**Case 4**
```
GET /jgive/campaign/619
"Authorization": "Bearer jjjjjj"
```

``` bash
curl --location --request GET 'http://{{host}}/index.php?option=com_api&app=jgive&resource=campaign&id=619&format=raw' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--header 'Authorization: Bearer jjjjjj'
```

This is how com_api works as of today, the campaign resource will receive the user object for userid 21

!!! info
    Feature available since `v2.4.0`
