---
date: 2017-01-15
title: Writing a report plugin
categories:
  - TJ Reports
tags:
  - Joomla
  - tjreports
type: Document
---

# Example Report Plugin
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

```php
<?php  
// No direct access  
defined('_JEXEC') or die('Restricted access');

//Include TJReport Model
JLoader::import('com_tjreports.models.reports', JPATH_SITE . '/components');

/**
 * Example report plugin of TJReport
 *
 * @since  1.0.0
 */
class TjreportsModelExamplereport extends TjreportsModelReports
{
	// Default ordering column
	protected $default_order = 'name';

	// Default ordering direction
	protected $default_order_dir = 'ASC';

	/* Must have constructor that will define columns of the report */
	/**
	 * Constructor.
	 *
	 * @param   array  $config  An optional associative array of configuration settings.
	 *
	 * @see     JModelLegacy
	 * @since   1.6
	 */
	public function __construct($config = array())
	{
		/**
		 * $this->columns is an associative array and one of the main entity of report plugin
		 * In this Array Key denotes the report header
		 * Value is also an associative array where -
		 * a)'table_column' - is directly linked to table column. This is also used in framework for ordering.
		 * If you have complex column like having COUNT than you should leave its value blank
		 * b) 'title’ - Title will be displayed on header and on hideshow checkbox
		 * c) 'disable_sorting' - Set to true of this column is not sortable by default all columns are sortable
		 */
		$this->columns = array(
			'attempt' => array('table_column' => 'lt.attempt', 'title' => 'COM_TJLMS_TITLE_ATTEMPTS'),
			'name' => array('table_column' => 'l.name', 'title' => 'COM_TJLMS_ATTEMPTREPORT_NAME'),
			'username' => array('table_column' => 'u.username', 'title' => 'COM_TJLMS_REPORT_USERUSERNAME'),
			'usergroup' => array('title' => 'COM_TJLMS_REPORT_USERGROUP', 'disable_sorting' => true),
		);

		parent::__construct($config);
	}

	/* Add your stylesheets here if you want any styling added to the page */
	/**
	 * Get style for left sidebar menu
	 *
	 * @return ARRAY Keys of data
	 *
	 * @since   2.0
	 * */
	public function getStyles()
	{
		return array(
			JUri::root(true) . '/css/tjlms_backend.css'
		);
	}

	/* Add filters dropdown or textboxes to the header or on the top of the table */
	/**
	 * Create an array of filters
	 *
	 * @return    ARRAY Filters used in reports
	 *
	 * @since    1.0
	 */
	public function displayFilters()
	{
		/**
		 * This method should return associative array and this can have max size of 2
		 * Header level filters will be defined in 0 index. eg. - attempt, name
		 * Top level filters will be defined in 1 index like if in single course report you want to select course and then it’s report will be populated. eg. - last_accessed_on
		 * Each filter will be an associative aray which can have follwoing keys-
		 * a)search_type - it can be text, select, date.range, calendar, html
		 * for select type field you need to mention select_options which is an array of  JHTML::_('select.option')
		 * b) searchin -  if you want framework to add where clause for search criteria you can define type. This is the column name where you want to search.
		 * c) type - This can be 'equal’,’custom’ otherwise by default it will match for like clause.
		 */

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

	/* Reports main query will be defined in this function */
	/**
	 * Method to get a JDatabaseQuery object for retrieving the data set from a database.
	 *
	 * @return  JDatabaseQuery  A JDatabaseQuery object to retrieve the data set.
	 *
	 * @since   1.6
	 */
	protected function getListQuery()
	{
		$db        = $this->_db;

		/* Create query instance by calling parents getListQuery */
		$query     = parent::getListQuery();

		/* You can get which columns are visible by calling below method */
		$colToshow = (array) $this->getState('colToshow');

		/**
		 * Currently active filters and it’s value are stored in filters and can get from below method.
		 * Also make sure you do not call getstate inside constructor and populatestate method
		 */
		$filters = $this->getState('filters');
		$user     = JFactory::getUser();
		$userId   = $user->id;

		/*
		 * Must have columns to get details of non linked data like completion
		 * and not defined in title_column of $this->columns array
		 */
		$query->select(array('user_id'));
		$query->from($db->quoteName('#__tjlms_lesson_track', 'lt'));
		$query->join('INNER', $db->quoteName('#__tjlms_lessons', 'l') . ' ON (' . $db->quoteName('lt.lesson_id') . ' = ' . $db->quoteName('l.id') . ')');
		$query->join('INNER', $db->quoteName('#__tjlms_courses', 'c') . ' ON (' . $db->quoteName('c.id') . ' = ' . $db->quoteName('l.course_id') . ')');
		$query->join('INNER', $db->quoteName('#__users', 'u') . ' ON (' . $db->quoteName('lt.user_id') . ' = ' . $db->quoteName('u.id') . ')');

		if (in_array('usergroup', $colToshow))
		{
			$subQuery = $db->getQuery(true);
			$subQuery->select('ugm.group_id');
			$subQuery->from($db->quoteName('#__user_usergroup_map') . ' as ugm');
			$subQuery->where($db->quoteName('ugm.user_id') . ' = ' . $db->quoteName('lt.user_id'));
			$query->select('(SELECT GROUP_CONCAT(ug.title SEPARATOR ", ") from  #__usergroups ug where ug.id IN(' . $subQuery . ')) as usergroup');

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

	/* If you want to perform any data manipulation before sending it to framework you can override below getItems method */
	/**
	 * Method to get an array of data items.
	 *
	 * @return  mixed  An array of data items on success, false on failure.
	 *
	 * @since   1.6
	 */
	public function getItems()
	{
		$items = parent::getItems();

		$lmsparams = JComponentHelper::getParams('com_tjlms');
		$date_format_show = $lmsparams->get('date_format_show', 'Y-m-d H:i:s');

		jimport('techjoomla.common');
		$tjCommon = new TechjoomlaCommon;

		foreach ($items as $item)
		{
			if (empty($item['last_accessed_on']) || $item['last_accessed_on'] == '0000-00-00 00:00:00')
			{
				$item['last_accessed_on'] = ' - ';
			}
			else
			{
				$item['last_accessed_on'] = $tjCommon->getDateInLocal($item['last_accessed_on'], 0, $date_format_show);
			}
		}

		return $items;
	}
}
```
