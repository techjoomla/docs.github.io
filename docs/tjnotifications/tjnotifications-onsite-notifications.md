---
title:       TJNotifications Onsite Notifications
description: TJNotifications (com_tjnotifications) Onsite Notifications
path:        blob/master/docs/tjnotifications
source:      tjnotifications-onsite-notifications.md
hero:        TJNotifications - Onsite Notifications
date:        2021-01-25
categories:
  - TJNotifications
tags:
  - Joomla
  - TJNotifications
  - com_tjnotifications
  - apis
---

# TJ Notifications - Onsite Notifications

## Template for Onsite Notification message body

Every notification template for onsite-notification must have following JSON structure.

The title, body, icon, link fields are needed as part of JSON template body.

``` json
{
    "title": "You checked into {{event.title}}",

    "body": "You have successfully checked into <strong>{{event.title}}</strong>. Thank you for your presence.",

    "icon": "https://example.com/images/checkin.png",

    "link": "https://example.com/index.php/component/jticketing/event-1?Itemid=139"
}
```

## Setup needed to make Onsite Notifications work

- Once "onsite" method is added in `defines.php` (or enabled from configuration in future)
- Goto admin backend, goto your component's notification templates
- On notification template edit form, enable onsite notifications
- In the notification message body add JSON like from above example, save template
- You must add recepients as a subscriber in TJNotifications OR pass user ids for "onsite" backend as receipients refer example below

```php
<?php
// Set recipients
$recipients = array (
    // Add JUsers
    Factory::getUser(488),
    Factory::getUser(489),

    // Add specific to, cc (optional), bcc (optional)
    'email' => array (
        'to' => array ($customer->email),
        'cc' => array ($admin->email)
    ),

    // Onsite notifications
    'onsite' => array (
        488,
        489
    ),

    // Add mobile numbers for sms (optional) - will be supported in future
    'sms' => array (
        '+91987654320',
        '+91987654321'
    )
);
```

- Now, as long as your code is triggering TJNotification lib send methods, notifications data will be added in to database table for newly generated notifications
- TJNotifications has both controller methods and API endpoints available to fetch these NEW notifications to be shown to end user
- You can use those endpoints to make ajax call on client side to get NEW notification messages to be shown to end user

## Sample client side code to fetch NEW notification messages:

```javascript
document.onreadystatechange = function () {
    if (document.readyState == "interactive") {
        // @withCredentials=true: pass the cross-domain cookies to server-side
        // Pass joomla userid below
        const source = new EventSource(
            'index.php?option=com_tjnotifications&task=messages.getNewMessagesStream&userid=123',
            {withCredentials:false}
        );

        source.addEventListener('message', function(event) {
            data = JSON.parse(event.data);

            if (typeof(data.notifications) != "undefined") {
                // Show new messages using some way
            }
        }, false);
    }
}
```