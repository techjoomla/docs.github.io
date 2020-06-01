---
date: 2020-04-22
title: Introduction
description: Universal Content Manager (UCM) extension for Joomla. Uses TJ-Fields for fields management.
categories:
  - TJ-UCM
tags:
  - Joomla
  - Universal Content Manager
type: Document
nav_ordering: 1
showSidebar: true
published: true
pageTitle: "Universal Content Manager (UCM) extension for Joomla"
permalink: tj-ucm/com-tjucm-introduction.html
---

Universal Content Manager (UCM) is an application built on top of TJ-Fields.

Using UCM you can easily create fully customizable forms (content/UCM Type) in Joomla.

## Features of UCM

#### 1) Save As Draft
You can configure your created form (content/UCM Type) to allow users to save the filled form in draft mode i.e the user can partially fill the form and complete as per thair convenience.

#### 2) Autosave
This config of UCM Type allows using Ajax-based autosave of form which will autosave the form data in draft mode when a user changes the value of any of the fields on the form. This ensures that the user does not lose the changes done if the form is accidentally closed or any other issue occurs while filling the form.

#### 3) Flexibility to use different layouts
You can create different form layouts for different UCM Types and configure the UCM Type to use those layouts.

#### 4) UCM Type and Field permissions
You can control the access of UCM Types and Fields in them to show/hide UCM Types and Fields from users of different user groups. Following are the permission which you can use.

##### UCM Type permission

- **Create Item** : Allows users in the group to create an item in the given UCM type.
- **View all Items** : Allows users in the group to view items created by any user in the given UCM type.
- **Edit all Items** : Allows users in the group to edit items created by any user in the given UCM type.
- **Edit all Item State** : Allows users in the group to edit state of any item created by any user in the given UCM type.
- **Edit Own Item** : Allows users in the group to edit own items in the given UCM type.
- **Delete all Items** : Allows users in the group to delete items created by any user in the given UCM type.

##### Field permission

- **Add Field Value** : Allows users in the group to add value to the field.
- **View Field Value** : Allows users in the group to view the field's value.
- **Edit Field Value** : Allows users in the group to edit the field's value.
- **Edit Own Field Value** : Allows users in the group to edit the field's value added by the user.

#### 5) List view for items
UCM also provides a list view to show the list of submitted items against the UCM Type.

#### 6) Category for UCM Items
You can create different categories for a UCM Type and store the items of the UCM Type against them.
Eg. Consider a company has created a form to collect data of aspirants to join the company in different departments (Accounts, Testing, Development etc), The form for collecting the data will be same which will collect the same type of data but for different departments. In this case you can create UCM Type categories as "Accounts", "Testing", "Development" and add "UCM-Category" field in the form which will help in storing the data of people applying for different departments.
