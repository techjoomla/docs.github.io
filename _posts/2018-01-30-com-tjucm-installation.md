---
date: 2018-01-30
title: Package Installation
categories:
  - TJ UCM
tags:
  - Joomla
  - TJUCM
  - TJFields
type: Document
---

# Steps to install whole package :

1. First of all, you needs to install both components(zip package) TJ UCM & TJ Fields in your site “Extension >> Manage >> Install >> Upload Package File”.

2. After that, you needs to install TJ Strapper library in your application as it makes the buttons “Save”, “Save & Close”, “Save & New” & “Cancel” actionable at the time of adding / updating fields under particular “Type”

3. After successful installation, you can see TJ UCM component under “Components >> TJ- UCM”.

4. By Clicking TJ-UCM, you will redirect to respective component page where you will see the 2 submenu at left sidebar as Types & Items. By default, this component will show the list of all types added in the database.

5. If, you want to create new type then you needs to click on “New” menu & you will redirect to new page where you will see the form having fields title, alias, unique identifier(which is automatically created with combination of your component_name.type_alias), Allowed count(how many times user can add item for this UCM type), Allowed Draft Save(have a feature about to save form info for this type as a draft).

6. After successful addition of new type, it will listed out under types section. List contains the columns “Status”, “Id”, “Title”, “Alias” & “Manage”

	Manage column have an option as “Category”, “Fields Group”, “Fields”, “Create Item” & “Items”

	* You can create/edit/Publish/Unpublish/Trash/Archive Category

	* You can create/edit/Publish/Unpublish/Trash Field Groups

	* You can create/edit/Publish/Unpublish/Trash/Archive Fields

7. TJ UCM has filter option in the admin panel which contains the filter criteria as “Published / Unpublished / Archived / Trashed / All”


