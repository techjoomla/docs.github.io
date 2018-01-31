---
date: 2018-01-30
title: Databse Structure For TJ UCM & TJ Fields
categories:
  - TJ UCM
tags:
  - Joomla
  - TJUCM
  - TJFields
type: Document
---

# Database Tables

#### UCM Type Table (#__tj_ucm_types)

| Name              | Type           | Comments                         |
| ----------------- |----------------| -------------------------------- |
| id                | int(11)        |                                  |
| asset_id          | int(11)        |   id from #__tj_assets table     |
| ordering          | int(11)        |   Display order                  |
| title             | varchar(255)   |   UCM type                       |
| alias             | varchar(255)   |   UCM type alias                 |
| state             | tinyint(1)     |   Published/Unpublished          |
| type_description  | text           |                                  |
| unique_identifier | varchar(255)   |   combination of component name + ucm type alias name e.g.com_tjucm.application-form  |
| parent_id         | int(11)        |   in case if this ucm type contains another ucm type i.e. subform id (contained UCM type id as parent_id )                                                          |
| params            | varchar(255)   |   {"allowed_count":"100","allow_draft_save":"1"} |
| checked_out       | int(11)        |                                  |
| checked_out_time  | datetime       |                                  |
| created_by        | int(11)        |   Created By user id             |
| created_date      | datetime       |   Created date                   |
| modified_by       | int(11)        |   Modified By user id            |
| modified_date     | datetime       |   Modified date                  |

***

#### UCM Data Table (#__tj_ucm_data)

| Name              | Type           | Comments                         |
| ----------------- |----------------| -------------------------------- |
| id                | int(11)        |                                  |
| asset_id          | int(11)        |   id from #__tj_assets table     |
| ordering          | int(11)        |   Display order                  |
| state             | tinyint(1)     |   Published/Unpublished          |
| category_id       | int(11)        |                                  |
| type_id           | int(11)        |                                  |
| client            | varchar(255)   |                                  |
| checked_out       | int(11)        |                                  |
| checked_out_time  | datetime       |                                  |
| created_by        | int(11)        |   Created By user id             |
| draft             | tinyint(1)     |                                  |
| created_date      | datetime       |   Created date                   |
| modified_by       | int(11)        |   Modified By user id            |
| modified_date     | datetime       |   Modified date                  |

***

#### TJ Fields Table (#__tjfields_fields)

| Name              | Type           | Comments                         |
| ----------------- |----------------| -------------------------------- |
| id                | int(11)        |                                  |
| core              | int(11)        |                                  |
| label             | varchar(255)   |                                  |
| name              | varchar(255)   |                                  |
| type              | varchar(255)   |                                  |
| state             | tinyint(1)     | Published/Unpublished            |
| required          | varchar(255)   |                                  |
| readonly          | int(11)        |                                  |
| created_by        | int(11)        | Created By user id               |
| description       | varchar(255)   |                                  |
| js_function       | text           |                                  |
| validation_class  | text           |                                  |
| ordering          | int(11)        | Display order                    |
| filterable        | tinyint(1)     | 0 - For not filterable field. 1 for filterable field|
| client            | varchar(255)   |                                  |
| group_id          | int(11)        |                                  |
| shownlist         | int(11)        |                                  |
| params            | varchar(255)   |                                  |

***

#### TJ Fields Value Table (#__tjfields_fields_value)

| Name              | Type           | Comments                         |
| ----------------- |----------------| -------------------------------- |
| id                | int(11)        |                                  |
| field_id          | int(11)        | Field table ID                   |
| content_id        | int(11)        |                                  |
| value             | text           |                                  |
| option_id         | int(11)        |                                  |
| user_id           | int(11)        |                                  |
| email_id          | varchar(255)   |                                  |
| client            | varchar(255)   | client(eg com_jticketing.event)  |

***

#### TJ Fields Group Table (#__tjfields_groups)

| Name              | Type           | Comments                         |
| ----------------- |----------------| -------------------------------- |
| id                | int(11)        |                                  |
| ordering          | int(11)        | Display order                    |
| state             | tinyint(1)     | Published/Unpublished            |
| created_by        | text           | Created By user id               |
| name              | varchar(255)   |                                  |
| client            | varchar(255)   |                                  |

***

#### TJ Fields Options Table (#__tjfields_options)

| Name              | Type           | Comments                         |
| ----------------- |----------------| -------------------------------- |
| id                | int(11)        |                                  |
| field_id          | int(11)        |                                  |
| options           | varchar(255)   |                                  |
| default_option    | varchar(255)   |                                  |
| value             | varchar(255)   |                                  |

***

#### TJ Fields Client Type Table (#__tjfields_client_type)

| Name              | Type           | Comments                         |
| ----------------- |----------------| -------------------------------- |
| id                | int(11)        |                                  |
| client            | varchar        |                                  |
| client_type       | varchar(255)   |                                  |

***

#### TJ Fields Category Mapping Table (#__tjfields_category_mapping)

| Name              | Type           | Comments                         |
| ----------------- |----------------| -------------------------------- |
| id                | int(11)        |                                  |
| field_id          | int(11)        |                                  |
| category_id       | int(11)        |                                  |
