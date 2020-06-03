---
title:       Custom fields (com_fields) support in TJReports plugins
description: Custom fields (com_fields) support in TJReports plugins
path:        blob/master/docs/tj-reports
source:      tjreports-plugin-add-custom-fields-support.md
hero:        TJReports - Custom fields (com_fields) support in TJReports plugins
date:        2020-04-22
categories:
  - TJReports
tags:
  - Joomla
  - TJReports
  - com_tjreports
  - com_fields
---


## 1. Background

TJReport has plugins that let you index the custom fields data created using Joomla's `com_fields` component.

### Core TJReports plugins to index data created using com_fields

| Plugin group  | Plugin name | What does this plugin do |
| ------------- | ------------- | ------------- |
| content | tjreportsfields | This plugin adds / updates / deletes columns for *`#__tjreports_context` tables |
| user | tjreportsindexer | This plugin adds / updates / deletes user-data for `#__tjreports_com_users_user` table|

*In case of if you are using com_fields for com_users context table name will be as
`#__tjreports_context` = `#__tjreports_com_users_user`

## 2. Which all com_fields's field types are supported in TJReports?

| com_field field type  | tjreport indexer table column type | how / what indexer stores values for this | what type of filter in tjreports used for this |
| ------------- | ------------- | ------------- | ------------- |
| calendar | datetime  | save user entered  | daterange filter  |
| checkboxes | varchar (255)  | save TEXT and not VALUE  | select / equal match  |
| color | varchar (7)  | save user entered  | -  |
| editor | text  | save user entered  | text / equal match  |
| integer | int (11)  | save user entered  | select / equal match  |
| list | varchar (255)  | save TEXT and not VALUE  | select / equal match  |
| imagelist | _not supported_  | NA  | NA  |
| media | _not supported_   | NA  | NA  |
| radio | varchar (255)  | save TEXT and not VALUE  | select / equal match  |
| repeatable | _not supported_   | NA  | NA  |
| sql | text | save TEXT and not VALUE  | text / equal match  |
| text | text  | save user entered  | text / equal match  |
| textarea | text  | save user entered  | text / equal match  |
| url | varchar (250) [based on joomla weblinks table]  | save user entered  | text / equal match  |
| user | varchar (400) [based on joomla user table] | store user's name  | select / equal match  |
| usergrouplist | varchar (100) [based on joomla usergroups table]  | store usergorup title  | select / equal match  |

### Database table used for this (indexed data)

* Every `tjreport` plugin will rely on a certain table where custom fields data is stored in rows (1 record per row)
	- eg. If `context` is `com_users.user` -> table name should be `#__tjreports_com_users_user`
* Column which is needed here in this example is
  	- `record_id` *int (11)*

## 3. How to add user fields to any tjreports plugin?

Below are steps for adding those user fields data columns into an existing report.
Eg: For a plugin named `mytjreportplugin`, open plugin entry file `mytjreportplugin.php`

### Steps
1. [Setup custom fields columns](#step-1-setup-custom-fields-columns)
1. [Setup custom fields filters](#step-2-setup-custom-fields-filters)

#### Step 1: Setup custom fields columns

Change constructor from

```php
<?php
public function __construct($config = array())
{
	$this->columns = array (
		// Columns setup here
	);

	parent::__construct($config);
}
```

to below

```php
<?php
public function __construct($config = array())
{
	// Joomla fields integration
	// Define custom fields table, alias, and table.column to join on
	$this->customFieldsTable       = '#__tjreports_user_fields';
	$this->customFieldsTableAlias  = 'tuf';
	$this->customFieldsQueryJoinOn = 'lt.user_id';

	$this->columns = array (
		// Columns setup here
	);

	parent::__construct($config);
}
```

In step 1, we define

- `customFieldsTable`- Custom field's indexed table in which we store duplicated data
- `customFieldsTableAlias`  - Alias for above table
- `customFieldsQueryJoinOn` - Table name and column name on which TJReport will do database query join on (TJReports' model will do query join on the custom fields table's **`record_id`** column and with the other DB table which plugin mainly runs the report on.)


#### Step 2: Setup custom fields filters

Change code from

```php
<?php
public function displayFilters()
{
	// Set filters code here
	$dispFilters = array(
		array(
			// Set filters code here
		),
		array(
			// Set filters code here
		)
	);

	return $dispFilters;
}
```

to

```php
<?php
public function displayFilters()
{
	// Set filters code here
	$dispFilters = array(
		array(
			// Set filters code here
		),
		array(
			// Set filters code here
		)
	);

	// Joomla fields integration
	// Call parent function to set filters for custom fields
	if (method_exists(get_parent_class($this), 'setCustomFieldsDisplayFilters'))
	{
		parent::setCustomFieldsDisplayFilters($dispFilters);
	}

	return $dispFilters;
}
```

In step 2, we call the parent method `setCustomFieldsDisplayFilters()`, which sets up filters for columns from custom fields to be displayed on the report.

## 4. Conclusion

That's it, with these 2 simple steps, you will be able to easily add columns, filters, sorting on the columns form the Joomla user's custom fields.
