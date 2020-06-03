---
title:       TJFields Introduction
description: Introduction to TJFields (com_tjfields), a Field manager for Joomla
path:        blob/master/docs/tj-fields
source:      tjfields-introduction.md
hero:        TJFields - Introduction
date:        2020-04-22
categories:
  - TJFields
tags:
  - Joomla
  - Fields
  - Fields Manager for Joomla
  - com_tjfields
---

TJFields is a horizontal extension used to extend the existing forms (Adding custom fields) of any client component.

## Why TJFields?

TJFields helps in mitigating the flexible business requirements by quickly customising the fields on the form of the client component.

Using TJFields you can extend the existing form by adding custom fields in it with ease i.e without getting your hands dirty with code to save and render the extra custom fields data in the form.

Also, the data stored in the extra custom fields can be used for filtering the content of the client component. The extensions by Techjoomla, like Quick2Cart, JGive and JTicketing uses TJFields to filter their respective content.

E.g Consider that we have a online shopping cart (Quick2Cart) system in which sellers can create products and sell those online.

Sellers can sell different types of products with different product attributes (Size, Colour, Height, Weight, Screen size and many more) and as an extension developer you cant provide all these differnt types of fields in product creation form as the product attributes differ for different products.

In this case you can integrate TJFields with your product creation form which will provide you the flexibility to extend your product creation form by adding custom(Extra) fields powered by TJFields.

You can create different fields for different categories of the client form (E.g Attributes like Screensize, RAM, ROM, Processor, Operating System etc for products of category Laptops and Mobiles and Attributes like Size, Colour, Matterial, Waist size etc for products of clothes category)

!!! note
	Also, you can use the added custom fields as the filters (Using TJFields Filter Module) to filter the content of the client (E.g The product attributes Screensize, RAM, ROM etc can be used as filters to filter the products of laptop category)

## Available Field Types

- Text
- Radio
- Checkbox
- List
- Single Select
- Multi Select
- Sql
- Textarea
- Textareacounter
- Calendar
- Editor
- Email
- File
- Spacer
- Subform
- Image
- Audio
- Video
- Number
- Hidden
