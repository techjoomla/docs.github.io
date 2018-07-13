---
date: 2018-02-09
title: Reports with dynamic columns
description: Reports with dynamic columns
categories:
  - TJ Reports
tags:
  - Joomla
  - tjreports
type: Document
showSidebar: true
published: true
nav_ordering: 4
pageTitle: "Reports with dynamic columns"
permalink: tj-reports/reports-with-dynamic-columns.html
---

In this type of a report, there may be one or more fixed columns and one or more columns that change based on a filter value. An example is the plugin for RSForm, where the report shows the list of responses once a form is selected. There are a few of fixed columns like date and submitter, but the rest of the columns are dynamically shown based on the fields in the selected form. 

Such a report can be easily achieved by setting or overriding the `columns` property after knowing the results, say in the `getItems()` method. A limitation for such report is that one cannot set the default config from the administrator as columns are not known.

Below is sample code from the RS Form plugin to demonstrate how this can work.

```php
public function getItems()
{
	// Fetch Results
	$rows = $this->rsmodel->getSubmissions();

	// If no result reset columns
	if (empty($rows))
	{
		$this->setState('colToshow', array());

		return false;
	}

	// Get first Row
	$firstRow = reset($rows);

	// Keys of the results will be the columns
	$columns  = array_keys($firstRow['SubmissionValues']);

	// Set Column Title
	foreach ($columns as $key => &$column)
	{
		$this->columns[$column] = array('title' => $column);
	}

	// Get currently selected columns to show
	$colToshow = $this->getState('colToshow', array());

	if (!$colToshow)
	{
		$this->setState('colToshow', $columns);
	}

	// Set all functionality on the selected columns
	$this->defaultColToShow = $this->sortableWoQuery = $this->sortableColumns  = $this->showhideCols = $columns;

	return $final;
}
```

We also need to ensure that the Hide/Show columns gets reset when the Form Filter is changed in this case. To do this, you may add the below code in an onChange event where the HTML gets generated inside the `displayFilters()` method

```javascript
onchange="document.getElementById('ul-columns-name').remove(); tjrContentUI.report.submitTJRData();"
```

Example for how it’s done in the RS Form field
```php
$filterHtml  = '<div class="input-append input-group">';
$filterHtml .= JHtml::_('select.genericlist', $formToshow, 'filters[formid]',
					'class="filter-input" size="1" onchange="document.getElementById(\'ul-columns-name\').remove();tjrContentUI.report.submitTJRData();"', 'value', 'text', $searchValue);
$filterHtml	.= '</div>';
```


