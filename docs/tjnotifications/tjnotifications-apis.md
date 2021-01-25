---
title:       TJNotifications APIs
description: TJNotifications (com_tjnotifications) APIs
path:        blob/master/docs/tjnotifications
source:      tjnotifications-apis.md
hero:        TJNotifications - APIs
date:        2021-01-25
categories:
  - TJNotifications
tags:
  - Joomla
  - TJNotifications
  - com_tjnotifications
  - apis
---

# TJ Notifications APIs

## API: Subscribe to notification

=== "Method"
    ``` json
    POST
    ```

=== "URL"
    ``` json
    {{host}}/api/tjnotifications/subscription
    ```

=== "Headers"
    ``` json
    Content-Type: application/x-www-form-urlencoded
    Authorization: Bearer 416iasdiasodsidi4b59bae008a05a
    ```

=== "Data Posted"
    Content-Type: application/x-www-form-urlencoded

    ```
    backend: email / sms / push / onsite
    address: example@example.com / 1234567890 / dfr1234560qwe / NA
    state: 1
    is_confirmed: 1
    title: Example User - Email / sms / push / onsite
    device_id:
    platform:
    ```

=== "CURL example"
    ``` bash
    curl --location --request POST '{{host}}/api/tjnotifications/subscription' \
    --header 'Content-Type: application/x-www-form-urlencoded' \
    --header 'Authorization: Bearer 416iasdiasodsidi4b59bae008a05a' \
    --data-urlencode 'backend=email' \
    --data-urlencode 'address=example@example.com' \
    --data-urlencode 'state=1' \
    --data-urlencode 'is_confirmed=1' \
    --data-urlencode 'title=Example User - Email' \
    --data-urlencode 'device_id=' \
    --data-urlencode 'platform='
    ```

=== "Response"
    You will get a response, which looks like

    ``` json
    {
        "err_msg": "",
        "err_code": "",
        "response_id": 37,
        "api": "tjnotifications.subscription",
        "version": "",
        "data": {
            "id": "4",
            "title": "Example User - Email",
            "user_id": "613",
            "backend": "email",
            "address": "example@example.com",
            "device_id": "",
            "platform": "",
            "state": "1",
            "is_confirmed": "1",
            "created_by": "613",
            "modified_by": "613",
            "checked_out": "0",
            "created_on": "2021-01-25 06:02:19",
            "updated_on": "2021-01-25 06:03:09",
            "checked_out_time": "0000-00-00 00:00:00",
            "params": null
        }
    }
    ```

## API: Onsite Notification Messages - Get Undelivered (New) Messages List

=== "Method"
    ``` json
    GET
    ```

=== "URL"
    ``` json
    {{host}}/api/tjnotifications/messages?userid={{userid}}&limit={{limit}}&start={{start}}
    ```

=== "Headers"
    ``` json
    Authorization: Bearer 416iasdiasodsidi4b59bae008a05a
    ```

=== "URL params"
    Example

    ```
    userid:614
    limit:10
    start:0
    ```

=== "CURL example"
    ``` bash
    curl --location --request GET '{{host}}/api/tjnotifications/messages?userid=614&limit=10&start=0' \
    --header 'Content-Type: application/x-www-form-urlencoded' \
    --header 'Authorization: Bearer 416iasdiasodsidi4b59bae008a05a' \
    ```

=== "Response"
    You will get a response, which looks like

    ``` json
    {
        "err_msg": "",
        "err_code": "",
        "response_id": 54,
        "api": "tjnotifications.messages",
        "version": "",
        "data": {
            "notifications": {
                "0": {
                    "id": "21",
                    "title": "You checked into Event 1",
                    "body": "You have successfully checked into <strong>Event 1</strong>. Thank you for your presence.",
                    "icon": "https://img.icons8.com/ios/72/blood-sample.png",
                    "link": "{{host}}/index.php/component/jticketing/event-1?Itemid=139",
                    "created_on": "2021-01-25 06:00:37",
                    "read": "0"
                },
                "1": {
                    "id": "20",
                    "title": "You checked into Event 1",
                    "body": "You have successfully checked into <strong>Event 3</strong>. Thank you for your presence.",
                    "icon": "https://img.icons8.com/ios/72/blood-sample.png",
                    "link": "{{host}}/index.php/component/jticketing/event-3?Itemid=139",
                    "created_on": "2021-01-22 11:54:33",
                    "read": "0"
                },
                "total": 20
            }
        }
    }
    ```

## API: Onsite Notification Message - Mark as read

=== "Method"
    ``` json
    POST
    ```

=== "URL"
    ``` json
    {{host}}/api/tjnotifications/messagemarkasread
    ```

=== "Headers"
    ``` json
    Content-Type: application/x-www-form-urlencoded
    Authorization: Bearer 416iasdiasodsidi4b59bae008a05a
    ```

=== "Data Posted"
    Content-Type: application/x-www-form-urlencoded

    ```
    id:4
    ```

=== "CURL example"
    ``` bash
    curl --location --request POST '{{host}}/api/tjnotifications/messagemarkasread' \
    --header 'Content-Type: application/x-www-form-urlencoded' \
    --header 'Authorization: Bearer 416iasdiasodsidi4b59bae008a05a' \
    --data-urlencode 'id=4'
    ```

=== "Response"
    You will get a response, which looks like

    ``` json
    {
        "err_msg": "",
        "err_code": "",
        "response_id": 48,
        "api": "tjnotifications.messagemarkasread",
        "version": "",
        "data": {
            "success": true
        }
    }
    ```