---
date: 2019-04-13
title: Impersonating another user
categories:
  - Joomla REST API
tags:
  - Joomla
  - REST API
type: Document
showSidebar: true
published: true
nav_ordering: 3
pageTitle: "Calling API resources"
permalink: joomla-rest-api/com-api-impersonation.html
---


`Feature available since 2.4.0`

Using the impersonation feature it is possible for a Super Admin user to impersonate another user. To use this feature, the token being used for an API call should be belonging to a Super Admin user (specifically, any user having the `core.admin` permission). 

To impersonate another user, pass the header `X-Impersonate`. The value of the header is the user id of the user to impersonate eg: `X-Impersonate` : `42`.

It is also possible to use the username or email of the user eg: `X-Impersonate` : `email:user@email.com` or `X-Impersonate` : `username:ashwin`.