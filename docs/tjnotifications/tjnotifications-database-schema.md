---
title:       TJNotifications Database Schema
description: Database Schema of TJNotifications (com_tjnotifications), notifications manager for Joomla
path:        blob/master/docs/tjnotifications
source:      tjnotifications-database-schema.md
hero:        TJNotifications - Database Schema
date:        2020-06-18
categories:
  - TJNotifications
tags:
  - Joomla
  - TJNotifications
  - com_tjnotifications
---


The notifications uses the following tables.

**#__tj_notification_templates**

This table contains 3 sets of columns per provider. When we add support for more providers, we will need to add 3 columns for each. the _status column indicates if that provider is enabled. The _body column stores the default body for the template. The _subject column is for providers that need a subject / title (eg: email, notifications).


| Column Name | Data Type |
|:-----------------|:-------------|
| id | INT |
| client | VARCHAR |
| key | VARCHAR |
| email_status | INT|
| sms_status | INT |
| push_status |  INT |
| web_status | INT |
| email_body | TEXT |
| sms_body | TEXT |
| push_body | TEXT |
| web_body | TEXT |
| email_subject | TEXT |
| sms_subject | TEXT |
| push_subject | TEXT |
| web_subject | TEXT |
| state | INT |
| created_on  | DATE |
| updated_on | DATE |
| is_override | INT |

**#__tj_notification_providers**

Master table for the providers

| Column Name | Data Type |
|---------------|-------------|
| provider | VARCHAR |
| state | INT |

**#__tj_notification_user_exclusions**

This table stores the user level settings. If a user disables some emails for some providers, then that exclusion is added to this table.

| Column Name | Data Type |
|---------------|-----------|
| user_id | INT |
| client | VARCHAR |
| key | VARCHAR |
| provider | VARCHAR |


**#__tj_notification_queue**

This table has a queue of outbound communications. TJNotifications adds its communication to this queue.

| Column Name | Data Type | Index | Comments |
|---------------|---------|-------|----------|
| queue_id | INT | Primary | |
| provider | VARCHAR | INDEX | Communication provider, eg: email, sms |
| context | VARCHAR | INDEX | |
| context_id | VARCHAR | INDEX | |
| body | TEXT | | |
| subject | TEXT | | |
| recipient | VARCHAR | | The recipient changes based on the provider. Eg: If provider is email, the recipient is an email. If the  provider is SMS, the recipient is phone number |
| created | DATETIME | | |
| expiry | DATETIME | | |
| retries | INT | | How many times was sending tried |
| last_attempted | DATETIME | | For future use. Stores the last attempt date if it is being retried multiple times |
| status | TINYINT | INDEX | Status of sending. 1=Sent, 0=Not sent |
| options | TEXT | | |