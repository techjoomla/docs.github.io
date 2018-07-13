---
date: 2017-01-15
title: Introduction
description: REST API framework for Joomla
categories:
  - Joomla REST API
tags:
  - Joomla
  - REST API
type: Document
nav_ordering: 1
showSidebar: true
published: true
pageTitle: "REST API framework for Joomla"
permalink: joomla-rest-api/com-api-introduction.html
---


com_api is a quick and easy way to add REST APIs to Joomla. Extendible via plugins, you an easily add support for more Joomla extensions. To get started, download the component and install the API plugins you need. Enable the plugins and you are ready to fetch your content via APIs. 

To add additional resources to the API, plugins need to be created. Each plugin can provide multiple API resources. Plugins are a convenient way to group several resources. Eg: A single plugin could be created for Quick2Cart with separate resources for products, cart, checkout, orders etc.

## Plugin Terms

### app
An app is essentially a Joomla plugin. However the plugin itself does nothing more than load the resources it contains. So the app is mainly used to package the API plugin and to enable adding any API specific parameters. Each app will have one or more resources. 

### resource
Resources are files that have code to accept input and set the API output. You will usually have multiple resources in an app. A common use case is for an extension like Easysocial or Jomsocial to have a single app. The app contains resources for various objects like groups, events, photos, newsfeed etc. The resource will contain the methods `get()` `post()` `delete()` to perform CRUD operations on that type of object.

### key / token
The key is used to access authenticated resources. The admin section allows you to create keys. It's also possible to use the `/api/user/login` API to login using username and password and get a token in response.
