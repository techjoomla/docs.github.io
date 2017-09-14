---
date: 2017-01-15
title: TJ Reports - Architecture Overview
categories:
  - TJ Reports
type: Document
---

# Overview
In terms of data structure, the TJ Reports framework is a typical Joomla MVC List view, with added features for saving queries, modifying columns and dynamic filters. Since reports are typically tabular data, reusing existing list view infrastructure was an obvious choice. So under the hood, it simply extends JModelList. The key difference is that instead of a single model, the framework allows plugins to extend the base reports model and provide a query or list for the report.

# Plugin Architecture
TJ Reports allows a plugin driven approach to creating "pluggable models" that can be rendered as a report. 
Each plugin is essentially a model class that can have Joomla's `getListQuery()` or simply `getItems()` methods to either return a query object, or directly the array of items to display. So TJReports does not use plugins in their classic event/dispatcher pattern. Instead, TJ Reports uses plugins to simplify installation and set any defaults via parameters.

To implement filters, sorting and dynamic columns there are additional methods that allow setting column names, toggle sortable and filter columns and choose which columns may be hidden by the user.

Once a plugin is installed, an instance of the plugin needs to be created for the report to be accessed. This is similar to how Modules work in Joomla. Each instance can have default filters, columns and sorting. 

Additionally, ACL can be applied to each instance so it becomes possible to create multiple lists from the same plugin, but with different filters that can be set such that users cannot change the filters.

# Component Architecture
After installing plugins, an instance of a plugin needs to be set up
