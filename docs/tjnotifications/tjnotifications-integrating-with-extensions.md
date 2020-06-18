---
title:       Integrating with Extensions
description: Integrating TJNotifications with your extensions
path:        blob/master/docs/tjnotifications
source:      tjnotifications-integrating-with-extensions.md
hero:        TJNotifications - Integrating with Extensions
date:        2020-06-18
categories:
  - TJNotifications
tags:
  - Joomla
  - TJNotifications
  - com_tjnotifications
---

## For TJNotifications v2.0.0 onwards

### 1. Install required extensions along with your extensions

To use TJ Notifications, you need to install

- com_tjnotifications
- lib_techjoomla


### 2. Install your notification templates

Each extension, which has some default emails, need to register those email templates in TJNotifications.

The easiest way to do during installation of your extension is

- create a json file of your notification templates
- and use TJNotification helper methods in your extension's installer script to register/update templates

!!! info
    All such templates get stored in `#__tj_notification_templates` and `#__tj_notification_templates_config` table.

#### 2.1 Sample format of JSON file

You can give file name as `extension_tjnotification_templates.json`

```json
{
    "template1": {
        "id": "",
        "client": "com_jgive",
        "key": "createCampaignMailToAdmin",
        "title": "Create Campaign email to admin (Approval Off)",
        "user_control": 1,
        "state": 1,
        "core": 1,
        "email": {
            "state": 1,
            "cc": "",
            "bcc": "",
            "from_name": "",
            "from_email": "",
            "emailfields": {
                "emailfields0": {
                    "id": "",
                    "language": "*",
                    "subject": "New Campaign \"{subject.title}\" has gone live on {subject.sitename}",
                    "body": "<p>Dear {info.adminname},<br /><br />Campaign with title <strong>{campaign.title}</strong> was created by {campaign.first_name}<br />Click <a href='{campaign.allcampaigns}'>here</a> to view all campaigns on the site.<br /><br />This is system generated email, please do not reply. Many thanks from <strong>{info.sitename}</strong></p>"
                }
            }
        },
        "replacement_tags": [
            {
                "name": "campaign.first_name",
                "description": "Name of Campaign Promoter"
            },
            {
                "name": "campaign.title",
                "description": "Name of Campaign"
            },
            {
                "name": "campaign.allcampaigns",
                "description": "All campaigns page link"
            },
            {
                "name": "info.sitename",
                "description": "Sitename"
            },
            {
                "name": "info.adminname",
                "description": "Siteadmin"
            }
        ]
    },
    "template2": {
        "id": "",
        "client": "com_jgive",
        "key": "createCampaignMailToPromoter",
        "title": "Create Campaign email to Campaign Owner (Approval OFF)",
        "user_control": 1,
        "state": 1,
        "core": 1,
        "email": {
            "state": 1,
            "cc": "",
            "bcc": "",
            "from_name": "",
            "from_email": "",
            "emailfields": {
                "emailfields0": {
                    "id": "",
                    "language": "*",
                    "subject": "Your Campaign \"{subject.title}\" is now live on {subject.sitename}",
                    "body": "<p>Hello {campaign.first_name},<br /><br />Your Campaign - <strong>{campaign.title}</strong> has been created and published.<br />Please visit this <a href='{campaign.mycampaigns}'>link</a> to view all your campaigns.<br /><br />This is system generated email, please do not reply. Many thanks from {info.sitename}</p>"
                }
            }
        },
        "replacement_tags": [
            {
                "name": "campaign.first_name",
                "description": "Name of Campaign Promoter"
            },
            {
                "name": "campaign.title",
                "description": "Name of Campaign"
            },
            {
                "name": "campaign.mycampaigns",
                "description": "Link to my Campaigns"
            },
            {
                "name": "info.sitename",
                "description": "Sitename"
            }
        ]
    }
}
```

!!! note
    Please note that, TJNotifications 2.0.0 onwards, replacements tags needs to be specified in double curly brackets like `{{ replacement_tag }}` instead of older way as `{ replacement_tag }`

#### Extra: Sample JSON for multiple backends like email, push, SMS, whatsapp

In future, once support for other backends is added, sample JSON for multiple backends like email, push, SMS, whatsapp can be as:

```json
{
    "template1": {
        "id": "",
        "client": "com_jgive",
        "key": "createCampaignMailToAdmin",
        "title": "Create Campaign email to admin (Approval Off)",
        "user_control": 1,
        "state": 1,
        "core": 1,
        "email": {
            "state": 1,
            "cc": "",
            "bcc": "",
            "from_name": "",
            "from_email": "",
            "emailfields": {
                "emailfields0": {
                    "id": "",
                    "language": "*",
                    "subject": "New Campaign \"{{subject.title}}\" has gone live on {{subject.sitename}}",
                    "body": "<p>Dear {{info.adminname}},<br /><br />Campaign with title <strong>{{campaign.title}}</strong> was created by {{campaign.first_name}}<br />Click <a href='{{campaign.allcampaigns}}'>here</a> to view all campaigns on the site.<br /><br />This is system generated email, please do not reply. Many thanks from <strong>{{info.sitename}}</strong></p>"
                }
            }
        },
        "push": {
            "state": 1,
            "pushfields": {
                "pushfields0": {
                    "id": "",
                    "language": "*",
                    "body": "{\"message\": {\"webpush\": {\"notification\": {\"title\": \"New Campaign gone live\", \"body\": \"New Campaign \"{{subject.title}}\" has gone live on {{subject.sitename}}\", \"icon\": \"https://my-server/icon.png\", \"url\": \"https://my-server/icon.png\"}}}}}"
                }
            }
        },
        "sms": {
            "state": 1,
            "smsfields": {
                "smsfields0": {
                    "id": "",
                    "language": "*",
                    "body": "Hi {{info.adminname}}, \n New campaign {{campaign.title}} created by {{campaign.first_name}} went live on site {{info.sitename}}"
                }
            }
        },
        "whatsapp": {
            "state": 1,
            "whatsappfields": {
                "whatsappfields0": {
                    "id": "",
                    "language": "*",
                    "body": "Hi {{info.adminname}}, \n New campaign {{campaign.title}} created by {{campaign.first_name}} went live on site {{info.sitename}}"
                }
            }
        },
        "replacement_tags": [
            {
                "name": "campaign.first_name",
                "description": "Name of Campaign Promoter"
            },
            {
                "name": "campaign.title",
                "description": "Name of Campaign"
            },
            {
                "name": "campaign.allcampaigns",
                "description": "All campaigns page link"
            },
            {
                "name": "info.sitename",
                "description": "Sitename"
            },
            {
                "name": "info.adminname",
                "description": "Siteadmin"
            }
        ]
    }
}
```

#### 2.2 Script to install default templates

This script should be used by extension developer to install some default templates while installation of their own extension. Developer can use this script as given below with correct file path of JSON file.

This script also takes care while updating your extisting extension. So in case if you already have existing emails with a defined "key" this script won't update new email templates. We will need to create a task to forcefully update all conents. So it won't discard customised emails on various sites.

```php
<?php
jimport('joomla.application.component.model');
use Joomla\CMS\Factory;

JTable::addIncludePath(JPATH_ADMINISTRATOR . '/components/com_tjnotifications/tables');

$db    = Factory::getDbo();
$query = $db->getQuery(true);
$query->select($db->quoteName('key'));
$query->from($db->quoteName('#__tj_notification_templates'));
$query->where($db->quoteName('client') . ' = ' . $db->quote("com_jgive"));
$db->setQuery($query);
$existingKeys = $db->loadColumn();

JModelLegacy::addIncludePath(JPATH_ADMINISTRATOR . '/components/com_tjnotifications/models');
$notificationsModel = JModelLegacy::getInstance('Notification', 'TJNotificationsModel');

$filePath     = file_get_contents('/path/to/extension_tjnotification_templates.json');
$str          = file_get_contents($filePath);
$templateJson = json_decode($str, true);

if (count($templateJson) != 0)
{
	foreach ($templateJson as $index => $templateData)
	{
		if (!in_array($array['key'], $existingKeys))
		{
			$notificationsModel->createTemplates($templateData);
		}
	}
}
```

### 3. The magic method Tjnotifications::send()

With the help of this method you can finally send the notification.

```php
<?php
jimport('techjoomla.tjnotifications.tjnotifications');

Tjnotifications::send($client, $key, $recipients, $replacements, $options);
```

Obviously, before sending, you need to prepare the parameters for Tjnotifications::send()

- `$client` is the name of your extension, eg: com_jgive
- `$key` is the unique reference for the type of email within your extension. Eg: donation.thankyou, campaign.update, order.thankyou
- `$recipients` is an array of JUser objects, array of emails, array of phone numbers (in case of SMS/ whatsapp), array of notification tokens (push). See example below
- `$replacements` is an object of objects, containing all replacements. The object and their properties are mapped against the values in the template body. `$replacements->order->id` maps to `{order.id}` for example.
- `$options` an instance of JRegistry. Contains additional options that may be used by the notification provider. In case of email options may include attachments, guestEmails etc

Finally, call  `Tjnotifications::send($client, $key, $recipients, $replacements, $options);`

This will send the notifications based on user preferences. If the user has disabled any of the notifications or specific delivery preferences, those notifications will not be sent to the user.

The 3rd party developer does not need to be aware of these settings.


### 4. Sample code to send an email using Tjnotifications.
This is the sample code for sending email from your extension. Generally, extensions have some methods to send an email in the model file, Extension developer can refer the following method to send an email from their extension.

```php
<?php
jimport('techjoomla.tjnotifications.tjnotifications');
use Joomla\CMS\Factory;

public function save($data)
{
	$client = "com_jcregister";
	$key    = "order";

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

		// Add notifiation tokens / device ids for push (optional)
		'push' => array (
			'notification_token1',
			'notification_token2'
		),

		// Add mobile numbers for sms (optional) - will be supported in future
		'sms' => array (
			'+91987654320',
			'+91987654321'
		),

		// Add mobile numbers for whatsapp (optional) - will be supported in future
		'whatsapp' => array (
			'+91987654320',
			'+91987654321'
		)
	);

	// Set replacements
	$order_info->id      = "236";
	$order_info->amount  = "500";
	$order_info->status  = "Pending";

	$customer_info->name    = "hemant";
	$customer_info->address = "";
	$customer_info->zip     = "411004";

	$replacements->order    = $order_info;
	$replacements->customer = $customer_info;

	// Set options
	$options = new JRegistry();
	$options->set('attachment', '/var/www/html/joomla/media/attach.pdf');
	$options->set('guestEmails', array("testuser@mailinator.com"));

	// Notify user
	Tjnotifications::send($client, $key, $recipients, $replacements, $options);
}
```

### 5. Register admin side sidebar menus you need

Add menus as

```php
<?php
use Joomla\CMS\Factory;

public static function addSubmenu($vName = '')
{
	$input = Factory::getApplication()->input;
	$vName = $input->get('view', '', 'STRING');

	$notificationTemplatesView     = 'index.php?option=com_tjnotifications&view=notifications&extension=com_jgive&filter[client]=com_jgive';
	$notificationSubscriptionsView = 'index.php?option=com_tjnotifications&view=subscriptions&extension=com_jgive';
	$notificationLogsView          = 'index.php?option=com_tjnotifications&view=logs&extension=com_jgive&filter[client]=com_jgive';

	JHtmlSidebar::addEntry(JText::_('COM_GIVE_TITLE_NOTIFICATION_TEMPLATES'), $notificationTemplatesView, $vName == 'notifications');
	JHtmlSidebar::addEntry(JText::_('COM_GIVE_TITLE_NOTIFICATION_SUBSCRIPTIONS'), $notificationSubscriptionsView, $vName == 'subscriptions');
	JHtmlSidebar::addEntry(JText::_('COM_GIVE_TITLE_NOTIFICATIONS_LOGS'), $notificationLogsView, $vName == 'logs');
}
```

Thats it!


!!! danger
    Instructions below are for older version of TJNotifications < 2.0.0 onwards

## For TJNotifications older versions v1.x.y

### 1. Install required extensions along with your extensions

To use TJ Notifications, you need to install

- com_tjnotifications
- lib_techjoomla


### 2. Install your notification templates
Each extension, which has some default emails, need to register those email templates in TJNotifications.

The easiest way to do during installation of your extension is

- create a json file of your notification templates
- and use TJNotification helper methods in your extension's installer script to register/update templates

!!! info
    All such templates get stored in `#__tj_notification_templates` table.

### 2.1 Sample format of JSON file

You can give file name as `extension_tjnotification_templates.json`

```json
{
    "template1": {
        "id": "",
        "client": "com_jgive",
        "key": "createCampaignMailToAdmin",
        "title": "Create Campaign email to admin (Approval Off)",
        "email_body": "<p>Dear {info.adminname},<br /><br />Campaign with title <strong>{campaign.title}</strong> was created by {campaign.first_name}<br />Click <a href='{campaign.allcampaigns}'>here</a> to view all campaigns on the site.<br /><br />This is system generated email, please do not reply. Many thanks from <strong>{info.sitename}</strong></p>",
        "sms_body": "",
        "push_body": "",
        "web_body": "",
        "email_subject": "New Campaign \"{subject.title}\" has gone live on {subject.sitename}",
        "sms_subject": "",
        "push_subject": "",
        "web_subject": "",
        "email_status": 1,
        "sms_status": 0,
        "push_status": 0,
        "web_status": 0,
        "user_control": 1,
        "core": 1,
        "replacement_tags": [
            {
                "name": "campaign.first_name",
                "description": "Name of Campaign Promoter"
            },
            {
                "name": "campaign.title",
                "description": "Name of Campaign"
            },
            {
                "name": "campaign.allcampaigns",
                "description": "All campaigns page link"
            },
            {
                "name": "info.sitename",
                "description": "Sitename"
            },
            {
                "name": "info.adminname",
                "description": "Siteadmin"
            }
        ]
    },
    "template2": {
        "id": "",
        "client": "com_jgive",
        "key": "createCampaignMailToPromoter",
        "title": "Create Campaign email to Campaign Owner (Approval OFF)",
        "email_body": "<p>Hello {campaign.first_name},<br /><br />Your Campaign - <strong>{campaign.title}</strong> has been created and published.<br />Please visit this <a href='{campaign.mycampaigns}'>link</a> to view all your campaigns.<br /><br />This is system generated email, please do not reply. Many thanks from {info.sitename}</p>",
        "sms_body": "",
        "push_body": "",
        "web_body": "",
        "email_subject": "Your Campaign \"{subject.title}\" is now live on {subject.sitename}",
        "sms_subject": "",
        "push_subject": "",
        "web_subject": "",
        "email_status": 1,
        "sms_status": 0,
        "push_status": 0,
        "web_status": 0,
        "user_control": 1,
        "core": 1,
        "replacement_tags": [
            {
                "name": "campaign.first_name",
                "description": "Name of Campaign Promoter"
            },
            {
                "name": "campaign.title",
                "description": "Name of Campaign"
            },
            {
                "name": "campaign.mycampaigns",
                "description": "Link to my Campaigns"
            },
            {
                "name": "info.sitename",
                "description": "Sitename"
            }
        ]
    }
}
```

#### 2.2 Script to install default templates
This script should be used by extension developer to install some default templates while installation of their own extension. Developer can use this script as given below with correct file path of JSON file.

This script also takes care while updating your extisting extension. So in case if you already have existing emails with a defined "key" this script won't update new email templates. We will need to create a task to forcefully update all conents. So it won't discard customised emails on various sites.

```php
<?php
use Joomla\CMS\Factory;

jimport('joomla.application.component.model');
JTable::addIncludePath(JPATH_ADMINISTRATOR . '/components/com_tjnotifications/tables');

$db    = Factory::getDbo();
$query = $db->getQuery(true);
$query->select($db->quoteName('key'));
$query->from($db->quoteName('#__tj_notification_templates'));
$query->where($db->quoteName('client') . ' = ' . $db->quote("com_jgive"));
$db->setQuery($query);
$existingKeys = $db->loadColumn();

JModelLegacy::addIncludePath(JPATH_ADMINISTRATOR . '/components/com_tjnotifications/models');
$notificationsModel = JModelLegacy::getInstance('Notification', 'TJNotificationsModel');

$filePath     = file_get_contents('/path/to/extension_tjnotification_templates.json');
$str          = file_get_contents($filePath);
$templateJson = json_decode($str, true);

if (count($templateJson) != 0)
{
	foreach ($templateJson as $index => $templateData)
	{
		if (!in_array($array['key'], $existingKeys))
		{
			$notificationsModel->createTemplates($templateData);
		}
	}
}
```

### The magic method Tjnotifications::send()
Prepare the parameters for Tjnotifications::send()

`$client` is the name of your extension, eg: com_jgive

`$key` is the unique reference for the type of email within your extension. Eg: donation.thankyou, campaign.update, order.thankyou

`$recipients` is an array of JUser objects
eg: `$recipients = JAccess::getUsersByGroup(2);`

`$replacements` is an object of objects, containing all replacements. The object and their properties are mapped against the values in the template body. `$replacements->order->id` maps to `{order.id}` for example.

`$options` an instance of JRegistry. Contains additional options that may be used by the notification provider. In case of email options may include cc, bcc, attachments, reply to, from, guestEmails etc

Finally, call  `Tjnotifications::send($client, $key, $recipients, $replacements, $options);` This will send the notifications based on user preferences. If the user has disabled any of the notifications or specific delivery preferences, those notifications will not be sent to the user. The 3rd party developer does not need to be aware of these settings.


### Sample code to send an email using Tjnotifications.
This is the sample code for sending email from your extension. Generally, extensions have some methods to send an email in the model file, Extension developer can refer the following method to send an email from their extension.

```php
<?php
jimport('techjoomla.tjnotifications.tjnotifications');
use Joomla\CMS\Factory;

public function save($data)
{
	$client = "com_jcregister";
	$key    = "order";

	$recipients[] = Factory::getUser(488);
	$recipients[] = Factory::getUser(489);

	// Set replacements
	$order_info->id      = "236";
	$order_info->amount  = "500";
	$order_info->status  = "Pending";

	$customer_info->name    = "hemant";
	$customer_info->address = "";
	$customer_info->zip     = "411004";

	$replacements->order    = $order_info;
	$replacements->customer = $customer_info;

	// Set options
	$options = new JRegistry();
	$options->set('subject', $order_info);
	$options->set('cc', 'demo@mailinator.com');
	$options->set('attachment', '/var/www/html/joomla/media/attach.pdf');
	$options->set('guestEmails', array("testuser@mailinator.com"));
	$options->set('from', "user@techjoomla.com");
	$options->set('fromname', "pranoti");

	// Notify user
	Tjnotifications::send($client, $key, $recipients, $replacements, $options);
}
```
