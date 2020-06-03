---
title:       Writing a report plugin
description: Writing a report plugin for TJReports (com_tjreports), a reports manager for Joomla
path:        blob/master/docs/tj-reports
source:      tjreports-write-report-plugin.md
hero:        TJReports - Writing a report plugin
date:        2020-04-22
categories:
  - TJReports
tags:
  - Joomla
  - TJReports
  - com_tjreports
---


The plugin type for TJ Reports plugins is `tjreports`.

## Folder structure

- plugins/tjreports/examplereport/examplereport.xml
- plugins/tjreports/examplereport/examplereport.php
- plugins/tjreports/examplereport/language/en-GB/en-GB.plg_tjreports_examplereport.ini

## Manifest File (examplereport.xml)

```xml
<?xml version="1.0" encoding="utf-8"?>
<extension version="3.1" type="plugin" group="tjreports" method="upgrade">
	<name>PLG_TJREPORTS_EXAMPLEREPORT</name>
	<author>Techjoomla</author>
	<creationDate>17th June 2016</creationDate>
	<copyright>(C)techjoomla.com</copyright>
	<license>http://www.gnu.org/copyleft/gpl.html GNU/GPL</license>
	<authorEmail>contact@techjoomla.com</authorEmail>
	<authorUrl>www.techjoomla.com</authorUrl>
	<version>1.0.0</version>
	<description>PLG_TJREPORTS_EXAMPLEREPORT_DESC</description>
	<files>
		<filename plugin="examplereport">examplereport.php</filename>
		<filename>index.html</filename>
		<folder>language</folder>
	</files>
</extension>
```

## The main report file (examplereport.php)

Create a file for plugin same as plugin name - ```examplereport.php```

At the top of the file include TJReport Model

```php
<?php
JLoader::import('com_tjreports.models.reports', JPATH_SITE . '/components');
```

Extend your plugin from TjreportsModelReports

```php
<?php
class TjreportsModelExamplereport extends TjreportsModelReports
```

Default ordering column will be assigned to default_order

```php
<?php
protected $default_order = 'name';
```

Default ordering direction will be assigned to default_order_dir

```php
<?php
protected $default_order_dir = 'ASC';
```


All columns details must be added to $columns variable. `$columns` is an associative array and one of the main entity of report plugin. Key of this Array denotes the report header and  Value is an associative array which can have following detail as key value -

- `table_column` - is directly linked to table column. This is also used in framework for ordering. If you have complex column like having COUNT than you should leave its value blank
-  `title` - Title will be displayed on header and on hideshow checkbox
-  `disable_sorting` - Set to true of this column is not sortable by default all columns are sortable


```php
<?php
public function __construct($config = array())
{
	$this->columns = array(
		'attempt' => array('table_column' => 'lt.attempt', 'title' => 'COM_TJLMS_TITLE_ATTEMPTS'),
		'usergroup' => array('title' => 'COM_TJLMS_REPORT_USERGROUP', 'disable_sorting' => true),
	);

	parent::__construct($config);
}
```

!!! info
	Most important method of your plugin is `getListQuery`. You must have this method and this should return query for your report.

To utilize framework where and order by functionality you must call parent getListQuery method and then add your extra clauses in that query object. When you call parent method framework add where clauses as per the filters you have defined in `displayFilters` method(see below for more detail)

```php
<?php
$query = parent::getListQuery();
```


```php
<?php
protected function getListQuery()
{
	$db = $this->_db;

	/* Create query instance by calling parents getListQuery */
	$query = parent::getListQuery();

	$query->from($db->quoteName('#__tjlms_lesson_track', 'lt'));

	if (in_array('usergroup', $colToshow))
	{
		if (isset($filters['usergroup']) && !empty($filters['usergroup']))
		{
			$subQuery = $db->getQuery(true);
			$subQuery->select('ugm.user_id');
			$subQuery->from($db->quoteName('#__user_usergroup_map') . ' as ugm');
			$subQuery->where($db->quoteName('ugm.group_id') . ' = ' . (int) $filters['usergroup']);
			$query->where('lt.user_id IN(' . $subQuery . ')');
		}
	}

	return $query;
}
```

One can get columns to show using below line. $colToshow is an array of all columns key that use has checked to display for ex - attempt. This should not be called inside constructor.

```php
<?php
$colToshow = (array) $this->getState('colToshow');```


One can get active filters using below line. $filters is an associative array with key as column name and value as selected value for the filter. This should not be called inside constructor.

```php
<?php
$filters = $this->getState('filters');```


If you want to add any styling you can add your styles in getStyles method

```php
<?php
public function getStyles()
{
	return array(
		JUri::root(true) . '/css/style.css'
	);
}
```

This framework also support build in filter functionality. You need to defined on which column you want to perform filteration. Framework currently supports text, dropdown and calendar type filter. There can have max two level of filters, one on Top level and other on Header level.

Filters will be defined inside `displayFilters` method and this method must return associative array and this can have max size of 2.

Header level filters will be defined in 0 index. eg. - attempt, name

Top level filters will be defined in 1 index. For instance in a single course report if you want to select course and then its report will be populated
Each filter will be an associative aray which can have follwoing keys-

- `search_type` - it can be text, select, date.range, calendar, html for select type field you need to mention select_options which is an array of  `JHTML::_('select.option')`
- `searchin` -  if you want framework to add where clause for search criteria you can define type. This is the column name where you want to search. Or you can leave it blank to add your own where clause. For that you need to add where clause in `getListQuery` method.
- `type` - This can be 'equal’,’custom’ otherwise by default it will match for like clause.

```php
<?php
public function displayFilters()
{
	$dispFilters = array(
		array(
			'attempt' => array(
				'search_type' => 'text', 'type' => 'equal', 'searchin' => 'lt.attempt'),
			'name' => array(
				'search_type' => 'select', 'select_options' => $lessonFilter, 'type' => 'equal', 'searchin' => 'lt.lesson_id'
			),
		),
		array(
			'last_accessed_on' => array(
				'search_type' => 'date.range',
				'searchin' => 'last_accessed_on',
				'last_accessed_on_from' => array('attrib' => array('placeholder' => 'FROM (YYYY-MM-DD)')),
				'last_accessed_on_to' => array('attrib' => array('placeholder' => 'TO (YYYY-MM-DD)')),
			)
		)
	);

	return $dispFilters;
}
```

If you want to perform any data manipulation before sending it to framework for displaying, you can override below `getItems` method

```php
<?php
public function getItems()
{
	$items = parent::getItems();

	foreach ($items as $item)
	{
		if (empty($item['last_accessed_on']) || $item['last_accessed_on'] == '0000-00-00 00:00:00')
		{
			$item['last_accessed_on'] = ' - ';
		}
	}

	return $items;
}
```
