---
title:       Architecture Overview
description: Architecture Overview of TJReports (com_tjreports), a reports manager for Joomla
path:        docs/tj-reports
source:      tjreports-architecture-overview.md
hero:        TJReports - Architecture Overview
date:        2020-04-22
categories:
  - TJReports
tags:
  - Joomla
  - TJReports
  - com_tjreports
---

In terms of data structure, the TJReports framework is a Joomla MVC List view, with added features for saving queries, modifying columns and dynamic filters. Since reports are typically tabular data, reusing existing list view infrastructure was an obvious choice. So under the hood, it simply extends JModelList.

The key difference is that instead of a single model, the framework allows plugins to extend the base reports model and provide a query or list for the report.

## Plugin Architecture
TJReports allows a plugin driven approach to creating "pluggable models" that can be rendered as a report. Each plugin is essentially a model class that can have Joomla's `getListQuery()` or `getItems()` method to either return a query object, or directly the array of items to display.

!!! note
	So TJReports does not use plugins in their classic event/dispatcher pattern. Instead, TJReports uses plugins to simplify installation and set any defaults via parameters.

To implement filters, sorting and dynamic columns there are additional methods that allow setting column names, toggle sortable and filter columns and choose which columns may be hidden by the user.

Once a plugin is installed, an instance of the plugin needs to be created for the report to be accessed. This is similar to how Modules work in Joomla. Each instance can have default filters, columns and sorting.

This way, it is possible to set up different reports by using a combination of default columns and filters. Plugin instances are stored in a table along with their configuration. The reports framework loads an instance of a report plugin by it's id.

Additionally, ACL can be applied to each instance so it becomes possible to create multiple lists from the same plugin, but with different filters that can be set such that users cannot change the filters.

## Component Architecture
As mentioned above, the component loads a single plugin instance using it's configuration. The component also applies the ACL based on the instance configuration.

The component allows users to create saved queries. Saved queries are stored similar to an instance, but they inherit the ACL and configuration superset of the instance they are created from.
