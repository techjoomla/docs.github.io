---
date: 2019-04-25
title: Integrating with extensions
description: Using TJ Queue in your extensions
categories:
  - TJ Queue
tags:
  - Joomla
  - queue
  - message
type: Document
nav_ordering: 1
showSidebar: true
published: true
pageTitle: "Integrating with extensions"
permalink: tj-queue/integrating-with-extensions.html
---
# TjQueue integration steps
* [Installation & configuration](#installation-and-configuration)
* [Produce Message](#produce-message)
* [Consume Message](#consume-message)

**_Note_**- php7.1 or higher version needed to run Tjqueue.

## Installation and configuration
* Installation
  - Backup your Joomla site using tools like Akeeba Backup before installation
  - Install TjQueue package. 

* Configuration
   
  | Config Name | Description  |
  | -- | -- |
  |  Topic Name | Name of the default topic to which messages will be sent. Developers may choose to create additional queues for other workloads and use them with the producer and CLI scripts. The suggested format for this is environment-purpose eg: prod-default-queue |
  | Queue Handler |Choose which adapter should handle the queue. Some adapters need additional configuration. eg: the SQS adapter will need the AWS credentials and the region name. |
  | AWS Key | Key to access AWS SQS  |
  | AWS Secret | Secret to access AWS SQS  |
  | AWS Region | Region where sqs in located e.g <b>ap-south-1</b> |

  Go to TjQueue -> Options -> and save the options.

## Produce Message
* [Message](#message)
* [Property](#property)
* [Delay](#delay)
* [Expiration (TTL)](#expiration-ttl)
* [Priority](#priority)
* [Timestamp, Content type, Message id](#timestamp-content-type-message-id)

### Message
A message data. 
e.g ['foo', 'bar'] or {foo:"Something"} or Hello world etc. 

```php
<?php
use Media\TJQueue\TJQueueProduce;
jimport('tjqueue.tjqueueproduce', JPATH_SITE . '/media');

// Create object of message produce class
$TJQueueProduce = new TJQueueProduce;

// Set message body - This can be any data
$messageBody = "This is message"
$TJQueueProduce->message->setBody($messageBody);

// Push the message to queue
$TJQueueProduce->produce();
```
### Property
```php
<?php
use Media\TJQueue\TJQueueProduce;
jimport('tjqueue.tjqueueproduce', JPATH_SITE . '/media');

// Create object of message produce class
$TJQueueProduce = new TJQueueProduce;

$TJQueueProduce->message->setProperty('client', 'pluginName.consumeClassFileName');

// Push the message to queue
$TJQueueProduce->produce();
```

### Delay 
Message sent with a delay set is processed after the delay time exceed.
Some brokers may not support it from scratch. 

```php
<?php
use Media\TJQueue\TJQueueProduce;
jimport('tjqueue.tjqueueproduce', JPATH_SITE . '/media');

// Create object of message produce class
$TJQueueProduce = new TJQueueProduce;
$TJQueueProduce->message->setDelay(60); // seconds

// Push the message to queue
$TJQueueProduce->produce();
```

### Expiration (TTL)

The message may have an expiration or TTL (time to live). 
The message is removed from the queue if the expiration exceeded but the message has not been consumed.
For example, it makes sense to send a forgot password email within the first few minutes, nobody needs it in an hour.

```php
<?php
use Media\TJQueue\TJQueueProduce;
jimport('tjqueue.tjqueueproduce', JPATH_SITE . '/media');

// Create object of message produce class
$TJQueueProduce = new TJQueueProduce;
$TJQueueProduce->message->setExpire(60); // seconds

// Push the message to queue
$TJQueueProduce->produce();
```

### Priority 

You can set a priority If you want a message to be processed quicker than other messages in the queue.
Client defines five priority constants:

* `messagePriority::VERY_LOW`
* `messagePriority::LOW`
* `messagePriority::NORMAL` (**default**)
* `messagePriority::HIGH`
* `messagePriority::VERY_HIGH`

```php
<?php
use Media\TJQueue\TJQueueProduce;
jimport('tjqueue.tjqueueproduce', JPATH_SITE . '/media');

// Create object of message produce class
$TJQueueProduce = new TJQueueProduce;
$TJQueueProduce->message->setMessagePriority('NORMAL'); 

// Push the message to queue
$TJQueueProduce->produce();
```

### Timestamp, Content type, Message id

Those are self describing things. 
Usually, they are set by Client so you don't have to worry about them. 
If you do not like what Client set you can always set custom values:
 
```php
<?php
use Media\TJQueue\TJQueueProduce;
jimport('tjqueue.tjqueueproduce', JPATH_SITE . '/media');

// Create object of message produce class
$TJQueueProduce = new TJQueueProduce;

$TJQueueProduce->message->setCustomMessageId('aCustomMessageId'); 
$TJQueueProduce->message->setTimestamp(time()); 
$TJQueueProduce->message->setContentType('text/plain'); 

// Push the message to queue
$TJQueueProduce->produce();
```

## Consume Message
* [Write consumer class](#Writing-consumer-class)
* [Message Properties](#message-properties)
* [Cron setup](#cron-setup)

### Writing consumer class
TjQueue dequeues the message and sends it to consume class, So now it's up to the consumer class to how to consume the message. 
TjQueue derive the consumer class name and class file path from **client** value. Let's see an example 
**E.g** client = core.email. 
TjQueue explode's the **client** by dot(.) operator. So here
* Plugin name:- **core** 
* File name:- **email.php**
* Class name (E.g Tjqueue**CoreEmail**)
* File path: /plugins/tjqueue/{PLUGIN_NAME}/consumers/{CONSUMER_NAME}.php.
  **E.g** /plugins/tjqueue/**core**/consumers/**email.php**
* This class file should have method **consume**

Refer the following code to implement message consumer class
```php
<?php
// No direct access
defined('_JEXEC') or die;

class TjQueuePluginNameConsumerName  // E.g TjqueueCoreEmail  
{
	/**
	 * Method the consume the Message
	 *
	 * @param   string  $message  A Message
	 *
	 * @return  boolean  This method should return acknowledgement flag, If 'true' then it will assume message is consumed successfully and it can be removed from queue. If 'false' then message consumption failed and needs to reprocess.
	 *
	 * @since 0.0.1
	 */
	public function consume($message)
	{
	    $messageBody = $message->getBody();
		/*
		 * Consume message code goes here.
		 */
	
	    // Send acknowledgment flag based to consumption success/failed
            return true/false;
       }
}
```
### Message Properties
Message object have getter's method to read the properties that set while crafting the message.
* Body
* Custom Message id
* Timestamp
* Content type
* Property
```php
<?php
$messageBody      = $message->getBody();
$aCustomMessageId = $message->getCustomMessageId(); 
$timestamp        = $message->getTimestamp(); 
$contentType      = $message->getContentType(); 
$property         = $message->getProperty('propertyName'); 
?>
```


### Cron Setup
TjQueue comes with Joomla CLI script which dequeues the message and sends it to the consumer class.
```php
php JOOMLA_SITE/cli/tjqueue.php -t TOPIC_NAME -n MESSAGES_LIMIT

OR

php JOOMLA_SITE/cli/tjqueue.php --topic="TOPIC_NAME" --n="MESSAGES_LIMIT"
```
#### Cron parameters
 * TOPIC_NAME: Name of the topic from which messages to read. 
   In case of *AWS SQS* this is the queue name.
* MESSAGES_LIMIT (optional): Number of Messages to read from the queue(Default 50).
