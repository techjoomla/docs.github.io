---
date: 2017-01-15
title: Importer Plugin Development
description: Guideline to create plugin for Importer
categories:
  - TJ_Importer
tags:
  - Joomla
  - REST API
  - Importer Tool
  - Bulk Tool
type: Document
---

To start using Importer on your Joomla site, you should know how it works and how to write plugins for your data container (zoo, articles, categories, users, etc.)

In this part, we are going to learn the same..

So, to get started, you should have a Joomla site with com_api installed. Then install com_importer and the importer core plugins.
Then for reference, install any of supported plugins (importer_zoo, importer_jarticle, importer_jcategory, etc.)

Now, to write importer plugins for your extension, you should have basic knowledge of how api works and PHP and Joomla code.

Before actually start writing the importer plugin for your data container, let’s first go through the basic rules and resource (or we can say files) that you need to create.

The name of your plugins should have prefix as “importer_”
For eg importer_zoo / importer_jarticle, etc.

You will need to compulsorily create the following resources (or we can say files) for your plugins. The resources name are standardized and explained below


## clienttypes :
  - has only get method.
  - Returns types for Items. (type is an entity based on which your component have different set of fields).
  - In case your component do not have Type entity, then just return an empty object.

##### API Url

`GET /index.php?option=com_api&app=importer_zoo&resource=clienttypes&format=raw`

##### Request Params

`None`

##### Response -

```javascript
{
	"add-film": "Film Masterlist",
	"agraphy-timeline": "Agraphy Timeline",
	"antiquarian-printmaking": "PRNT",
	"antiquities": "ANTQ",
	"architectural-heritage-of-the-indian-sub-continent": "ARCH",
	"article-masterlist": "Article Masterlist",
	"article": "Article",
	"auctions": "Auction Masterlist",
	"author": "Author",
	"auto-model-masterlist": "AUTO Model Masterlist"
}
```


## clientcolumns :
  - Contains only get method
  - Returns field details in a required format


##### API Url

`GET /index.php?option=com_api&app=importer_zoo&resource=clientcolumns&format=raw`

##### Request Params

Param Name  | Mandatory     |  Comment
------------- | -------------
type  | No | selected type 

##### Returned data should be in following format -

```javascript
[{
	"id": "zooid",
	"name": "Zoo-id",
	"type": "text",
	"readOnly": true,
	"primary": 1,
	"defaultCol": true,
	"option": []
}, {
	"id": "name",
	"name": "Zoo Name",
	"type": "text",
	"readOnly": false,
	"primary": 0,
	"defaultCol": true,
	"option": []
}, {
	"id": "alias",
	"name": "Alias",
	"type": "text",
	"readOnly": false,
	"primary": 0,
	"defaultCol": true,
	"option": []
}, {
	"id": "state",
	"name": "State",
	"type": "text",
	"readOnly": false,
	"primary": 0,
	"defaultCol": false,
	"option": []
}]
```

## clientdefaultVal :
  - Contains only get method
  - Returns field details to set default value at step 1 while adding new data


##### API Url

`GET /index.php?option=com_api&app=importer_zoo&resource=clientdefaultVal&format=raw`

##### Request Params

Param Name  | Mandatory     |  Comment
------------- | -------------
type  | No | selected type 

##### Returned data should be in following format -

```javascript
[{
	"type": "select",
	"label": "Category",
	"id": "category",
	"options": {
		"15": "- ANTQ (Id : 15)",
		"24": "- ANTQ.arm (Id : 24)",
		"96": "- ANTQ.dol (Id : 96)",
		"25": "- ANTQ.gam (Id : 25)",
		"26": "- ANTQ.min (Id : 26)",
		"28": "- ANTQ.msk (Id : 28)",
		"27": "- ANTQ.ptg (Id : 27)",
		"30": "- ANTQ.scu (Id : 30)",
		"31": "- ANTQ.thk (Id : 31)",
		"32": "- ANTQ.toy (Id : 32)",
		"713": "- ARCH (Id : 713)",
		"733": "- ARCH.bridges (Id : 733)",
		"734": "- ARCH.cap (Id : 734)",
		"735": "- ARCH.chu (Id : 735)",
		"736": "- ARCH.civ (Id : 736)",
		"737": "- ARCH.edi (Id : 737)",
		"738": "- ARCH.for (Id : 738)",
		"739": "- ARCH.gat (Id : 739)",
	}
}]
```


## clientoptions :

  - Contains only get method
  - Returns options to set while fetching records for editing


##### API Url

`GET /index.php?option=com_api&app=importer_zoo&resource=clientoptions&format=raw`

##### Request Params

Param Name  | Mandatory     |  Comment
------------- | -------------
type  | No | selected type 

##### Returned object should be in format -

```javascript
[{
	"Category": {
		"antq": "- ANTQ",
		"antq.arm": "--- ANTQ.arm (443)",
		"antq.dol": "--- ANTQ.dol (55)",
		"antq.gam": "--- ANTQ.gam (1)",
		"antq.min": "--- ANTQ.min (226)",
		"antq.msk": "--- ANTQ.msk (0)",
		"antq.ptg": "--- ANTQ.ptg (56)"
	}
},
 {
	"State": ["All", "Published", "Unpublished"]
}]
```


## clientrecords :

  - Contains only post method
  - Returns records based on the selected type and options


##### API Url

`POST /index.php?option=com_api&app=importer_zoo&resource=clientrecords&format=raw`

##### Request Params

Param Name  | Mandatory     |  Comment
------------- | -------------
batchparams  | Yes | selected type 
fields       | Yes |
ids          | No  | While fetching records to edit, if you have provided record identifiers, those you will get here
startPoint   | Yes |
countall     | Yes |
limit        | Yes |

##### Returned object should be in format -

```javascript
{
	"allReCount": 903,
	"records": [{
		"zooid": 282962,
		"97df63ba-a8d6-4d69-b2ff-3e32bdb52ba7": "newton-17",
		"d238463c-be81-4d7b-af00-8315d8c6d20f": "",
		"3ec5cd06-a41e-4917-8746-8e47bce354b6": "",
		"13999f73-ce3b-4d93-926a-1134520f1410": "",
		"d3e76c69-eea1-4718-8e21-619c6045b7c6": "",
		"420af9b1-e9cf-4fad-bf54-14c5edf6312a": "",
		"name": "newton-17",
		"alias": "newton-17",
		"category": 24,
		"state": "0"
	}, {
		"zooid": 282963,
		"97df63ba-a8d6-4d69-b2ff-3e32bdb52ba7": "newton-18",
		"d238463c-be81-4d7b-af00-8315d8c6d20f": "",
		"3ec5cd06-a41e-4917-8746-8e47bce354b6": "",
		"13999f73-ce3b-4d93-926a-1134520f1410": "",
		"d3e76c69-eea1-4718-8e21-619c6045b7c6": "",
		"420af9b1-e9cf-4fad-bf54-14c5edf6312a": "",
		"name": "newton-18",
		"alias": "newton-18",
		"category": 24,
		"state": "0"
	}, {
		"zooid": 282964,
		"97df63ba-a8d6-4d69-b2ff-3e32bdb52ba7": "newton-19",
		"d238463c-be81-4d7b-af00-8315d8c6d20f": "",
		"3ec5cd06-a41e-4917-8746-8e47bce354b6": "",
		"13999f73-ce3b-4d93-926a-1134520f1410": "",
		"d3e76c69-eea1-4718-8e21-619c6045b7c6": "",
		"420af9b1-e9cf-4fad-bf54-14c5edf6312a": "",
		"name": "newton-19",
		"alias": "newton-19",
		"category": 24,
		"state": "0"
	}]
}
```

## clientvalidate :

  - Contains only post method
  - Returns invalid field id of every row of handsontable

##### API Url

`POST /index.php?option=com_api&app=importer_zoo&resource=clientvalidate&format=raw`

##### Request Params

Param Name  | Mandatory     |  Comment
------------- | -------------
records  | Yes | Array of record objects to be validate
batchDetails       | Yes | Object of Batch details

##### Response should be in below format -

```javascript
{
	"297357": ["fe449ba6-f824-4977-98c8-174cb6efc9e6", "3fb0b63b-3e43-4773-a542-1d2a244d67ca"],
	"297359": null,
	"297363": ["fe449ba6-f824-4977-98c8-174cb6efc9e6"]
}
```

  - It is a json containing key as tempId and value as invalid field id’s
  - Do note that it should return the data in sequence of the rows.


## clientimport : 

  - Contains only post method
  - Returns the row data with id of item that we get after saving the row data.
  - Also returns the invalid row in case if any.

##### API Url

`POST /index.php?option=com_api&app=importer_zoo&resource=clientimport&format=raw`

##### Request Params

Param Name  | Mandatory     |  Comment
------------- | -------------
records  | Yes | Array of record objects to be imported
batchDetails       | Yes | Object of Batch details

##### Response should be in below format -

```javascript
{
	"records": [{
		"zooid": 308997,
		"name": "name 1",
		"alias": "name-1",
		"state": null,
		"category": "24",
		"tempId": "297357",
		"16462d5e-4a1e-4eec-8a75-e797f0774a73": null,
		"cab5f214-8780-43c5-8aca-d1141abdbd64": null,
		"id": 308997
	}, {
		"zooid": 309001,
		"name": "name 2",
		"alias": "name-2",
		"state": null,
		"category": "24",
		"207a7d33-bd8d-42c0-964f-3a6bf952c5a9": null,
		"tempId": 297359,
		"id": 309001
	}, {
		"zooid": 309005,
		"name": "name 3",
		"alias": "name-3",
		"state": null,
		"category": "24",
		"bb1aa66a-dae2-45bf-a6a5-19350649ef8b": null,
		"207a7d33-bd8d-42c0-964f-3a6bf952c5a9": null,
		"tempId": 297363,
		"id": 309005
	}],
	"invalid": {
		"297357": null,
		"297359": null,
		"297363": null
	}
}
```
  - Here 'records' node contains the saved row with updated primary id value and 'invalid' node contains any invalid record details.

--------------------------------------------------------------------------------------------------------------------------------
Once you are done writing your extension’s API plugin, install it on your site and then login to the front end of your site with super-user and access com_importer by the following URL -

index.php?option=com_importer&view=importer&clientapp=zoo

Where zoo is the suffix of you plugin name i.e. importer_zoo.
