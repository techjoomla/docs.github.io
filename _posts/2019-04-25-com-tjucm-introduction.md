---
date: 2019-06-03
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

Using UCM, users you can easily create fully customizable forms (content/UCM Type) in Joomla.

## Features of Backend Users

#### 1)UCM Type 

Backend Users can create an UCM type.
To create new type click on “New”.

The form contains fields as follows: 
- **Title:** Give a title for UCM type   
- **Alias:** It is an unique short piece of text that represent the title of certain items in a machine-friendly format   
- **Description:** Give short description about UCM type   
- **Is subform:** If the type is subform click on Yes   
- **Allowed count:** It is the count of records a user is allowed to add in a given UCM type.   
- **Allow draft save:** Allows user to save form as draft   
- **Allow auto save:** Saves form automatically after a field value gets changed   
- **Publish items by default:** If Yes, the records created gets published by default   
- **Enable timely autosave?:** Saves form automatically after a field value gets changed after given time interval 

#### 2)  Is Subform
If “Is subform” is set ON while creating type then the UCM type can be used as subform in another type.   
The auto save and draft save functionality is by default ON for subform UCM type.

#### 3)Allow draft save
Especially useful for lengthy forms, this feature allows saving form data in “draft” status. Form level field validations are bypassed in this case and users can come back later and complete the form.  The record can be moved in the published state only after all fields are valid.

Backend Users can configure their created form (content/UCM Type) to allow users to save the filled form in draft mode i.e the user can partially fill the form and complete as per their convenience.  
When UCM type is not a subform you can configure this feature. The behaviour of the subform type is the same as its main type. So no need to configure this feature for subform.

To configure this feature, Backend Users must set “Allow AutoSave=>No” while creating type from Backend Users.   
Then set  “Allow Draft Save=>Yes” and Save the Type.

#### 4) Allow Autosave
This config of UCM Type allows using Ajax-based autosave of form which will autosave the form data in draft mode when a user changes the value of any of the fields on the form.   
This ensures that the user does not lose the changes done if the form is accidentally closed or any other issue occurs while filling the form.   
To configure this feature, you have to set “Allow AutoSave=>Yes” while creating type from Backend.  

#### 5) Enable timely autosave?
It is functionality to auto save the form after a field value gets changed after a time interval which can be set by the Backend Users as per requirement.   
Backend Users have to enable this option while creating type. When it is enabled, add time interval in seconds.   

#### 6) Custom field UCM Subform
UCM subform is similar to subform.
To create UCM subform:   
 a) Create  a new type and set “Is subform” to Yes eg. Type2    
 b) Now create a field subform in type1.   
 c) Select subform from the list   
 d) Select layout:    
  Repeatable: It shows the form in div layout    
  Repeatable table: It shows form in table format. It is a recommended layout if the form layout is set “One input one row”.   
 e) If “Multiple” is set to true then the included form will be repeatable.   
 f) While using UCM subform as well as subform, there few limitations as follows:   
 g) For good user experience, keep the position of list field in subform first or second when subform layout is repeatable table.   
 h) If the fields in subform repeatable table layout are mandatory fields and not filled then the user gets blank error.   

#### 7) Flexibility to use different layouts
Backend users can create different form layouts for different UCM Types and configure the UCM Type to use those layouts.

#### 8) UCM Type and Field permissions
Backend users can control the access of UCM Types and Fields in them to show/hide UCM Types and Fields from users of different user groups. Following are the permission which you can use.

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

#### 9) Category for UCM Items
Backend Users can create different categories for a UCM Type and store the items of the UCM Type against them.
Eg. Consider a company has created a form to collect data of aspirants to join the company in different departments (Accounts, Testing, Development etc), The form for collecting the data will be same which will collect the same type of data but for different departments. In this case you can create UCM Type categories as "Accounts", "Testing", "Development" and add "UCM-Category" field in the form which will help in storing the data of people applying for different departments.

#### 9)  Export & Import
This feature is provided for Backend Users to export and import records from one site to another.
On the Backend, there are import and export buttons in the toolbar.   
   - **Export**  
       Backend Users has to select the type to be exported first from the type list.  
       Then click on “Export” Button.   
       A file with the ".json” extension will be downloaded.    
   - **Import**   
       Click on the “Import” button, and a pop-up will be displayed on screen.   
       Click on the “Choose a file” and select the file to be imported from your computer.   
       Click on the “Import” button to import the type.    
       The type gets added to the existing  type list.   


### Features for Frontend  Users 

#### 1) Frontend Views
UCM provides three views on site.   
  **1) Form View**   
     A form is displayed with custom fields created for a particular type.   
  **2) List View**   
     List view shows  the list of submitted items against the UCM Type.   
     An add button is provided on the top and bottom of list-view to add new records.    
     Along with add button, two new buttons are added at the bottom of list view    
     There is an action button set of view, edit, delete actions on the list view for each record.   
  **3) Detail View**   
     Detail  view shows  the details of the submitted item against the UCM Type.  
     Edit and delete actions are provided on detail view.   

#### 2)  Save As Draft
Especially useful for lengthy forms, this feature allows saving form data in “draft” status. Form level field validations are bypassed in this case and users can come back later and complete the form. The record can be moved in the published state only after all fields are valid.    
On Form view users can see the button “Save as Draft”.
The form will be saved as draft when users click on the button.


#### 3)  Autosave
This config of UCM Type allows using Ajax-based autosave of form which will autosave the form data in draft mode when a user changes the value of any of the fields on the form.
This ensures that the user does not lose the changes done if the form is accidentally closed or any other issue occurs while filling the form.   
On Form view, the form data will be saved in draft mode when an end-user changes the value of any of the fields on the form.


#### 4) Enable timely autosave?
It is functionality to auto save the form after a field value gets changed after a time interval which can be set by the Backend Users as per requirement.    
Backend Users has to enable this option while creating type. When it is enabled, add time in seconds.   
On form view, check the form gets auto saved after given time interval.   


#### 5) UCM Subform and Subforms
A type can have a subform field which will load another UCM type in a subform.    
The subform field can also be repeatable, so multiple sub-records can be added.    
On Form view, users can see a plus button or table with button set as below.    

![Webp net-resizeimage (2)](https://user-images.githubusercontent.com/58463266/75873719-6a20c980-5e36-11ea-84b0-61e996b08d32.png)


#### 6) Copy To Other
To make a copy of records from an UCM types to same/different UCM type, end user can use this feature.   
In Joomla, on the  Backend Users, there is a “Save as Copy” button to copy only one record at a time on edit view.   
Similar to this, “Copy to other” feature can copy multiple records at a time.   
On List view, there is a button at the bottom of the list: Copy to Other
First ussers need to select records from the list and click on the “Copy to Other” button.   
Select type in the pop-up displayed  and click on “Process” and refresh the list.   
Observe the created copy of selected record on the top of list.   


