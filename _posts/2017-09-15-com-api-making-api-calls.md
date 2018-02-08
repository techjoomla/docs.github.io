---
date: 2017-09-15
title: Making calls to API resources
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
---


## API URLs
The URL to access any route is of the format - 

`/api/{plugin}/{resource}`

Tip : If your resource expects an `id` parameter in the URL, you can use `/api/{plugin}/{resource}/{id}` as the API url. Other querystring need to be sent as is.

To enable SEF URLs for endpoints, make sure you have created a Joomla menu of the type API > API Endpoint. If you create the menu using any other alias than `api` make sure you use the apppropriate slug in the endpoint. If you do not have SEF URLs enabled, use  the endpoint URL as index.php?option=com_api&app={app}&resource={resource}&format=raw.

## Authentication
The API token is used for authentication. The token needs to be passed via the Authorization header using the Bearer scheme eg: `Authorization: Bearer <token>`. Previous versions also allowed passing the token as a querystring variable with the name `key`. The querystring approach will be deprecated in the future version. It is possible for an app to make an entire resource or a specific HTTP method in a resource public

Note : Sometimes Apache does not pass on the Authorization header, in such cases send then token using the X-Authorization header i.e. `X-Authorization: Bearer {token}`

## Output Format
The default output format is JSON. However it's also possible to get XML output by setting the `Accept: application/xml` header.

# Overriding Output
If you wish to modify the 'envelope' of the response, you can copy the file `components/com_api/libraries/response/jsonresponse.php` into `templates/{your template}/json/api.php` and modify the structure of the output. A similar override is possible by creating a xml.php as well.
