---
date: 2017-01-15
title: Integration Guide
categories:
  - TJ Reports
tags:
  - Joomla
  - tjreports
type: Document
---

# TJ Reports Integration Guide

## Installing the default instances
The reporting plugin only offers the code needed for the report. For the report to be usable, it needs to be configured. An extension developer who wishes to create default 'instances' for the plugins can do that by adding the below method in the installation script.

In order to use plugin, after installation you need to enable that plugin and to add your plugin detail to table. Below is the code that will add new entry in the table for missing plugins.

```
JModelLegacy::addIncludePath(JPATH_ADMINISTRATOR . '/components/com_tjreports/models');
$model = JModelLegacy::getInstance('Reports', 'TjreportsModel');
$installed = $model->addTjReportsPlugins();
```

If you are intalling plugin as a part of a package. You can call above method in postflight of package script. Above method requires some detail from your plugin so you must have below method that will return plugin detail.

```
public function getPluginDetail()
{
  $detail = array('client' => 'com_example', 'title' => JText::_('PLG_TJREPORTS_EXAMPLEREPORT_TITLE'));

  return $detail;
}
```


```$detail``` array of above method should have following details -

a) ```client``` - This is used for grouping of your plugins.

b) ```title``` - This is the title of your plugin visible to users.

## Linking to TJ reports
Just like other infrastructure extensions, you can pass the client context to show reports related to your extension. For instance Shika uses `index.php?option=com_tjreports&client=tjlms` for reports contextual to Shika LMS. The client is be specified when creating instances.

When user passes client parameter in the url at backend, framework also fetches the menus added in the `addSubmenu` of the component you passed in the client parameter. 

Below is the default URL for your extension 

`index.php?option=com_tjreports&client=com_example&task=reports.defaultReport`

This  will bring you to the first plugin of that client type. 


