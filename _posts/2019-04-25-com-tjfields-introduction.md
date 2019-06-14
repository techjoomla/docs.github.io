---
date: 2019-06-03
title: Introduction
description: Field manager for Joomla (TJ-Fields)
categories:
  - TJ - Fields
tags:
  - Joomla
  - Fields
  - TJ - Fields
type: Document
nav_ordering: 1
showSidebar: true
published: true
pageTitle: "Field manager for Joomla (TJ-Fields)"
permalink: tj-fields/com-tjfields-introduction.html
---

TJ-Fields is a horizontal extension used to extend the existing forms (Adding custom fields) of any client component.

### Why TJ-Fields?

TJ-Fields helps in mitigating the flexible business requirements by quickly customising the fields on the form of the client component.

Using TJ-Fields you can extend the existing form by adding custom fields in it with ease i.e without getting your hands dirty with code to save and render the extra custom fields data in the form.

Also, the data stored in the extra custom fields can be used for filtering the content of the client component. The extensions like Quick2Cart, JGive and JTicketing uses TJ-Fields to filter their respective content.

E.g Consider that we have a online shopping cart (Quick2Cart) system in which sellers can create products and sell those online.

Sellers can sell different types of products with different product attributes (Size, Colour, Height, Weight, Screen size and many more) and as an extension developer you cant provide all these differnt types of fields in product creation form as the product attributes differ for different products.

In this case you can integrate TJ-Fields with your product creation form which will provide you the flexibility to extend your product creation form by adding custom(Extra) fields powered by TJ-Fields.

You can create different fields for different categories of the client form (E.g Attributes like Screensize, RAM, ROM, Processor, Operating System etc for products of category Laptops and Mobiles and Attributes like Size, Colour, Matterial, Waist size etc for products of clothes category)

Also, you can use the added custom fields as the filters (Using TJ-Fields Filter Module) to filter the content of the client (E.g The product attributes Screensize, RAM, ROM etc can be used as filters to filter the products of laptop category)

### Available Field Types

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

### How to integrate client with TJ-Fields?


Here the **Client** represents the view (Product Creation Form) of the vertical extension (Shopping Cart Extension) where you want to use TJ - Fields to get support of custom/extra fields (Product Attributes). e.g com_quick2cart.product, com_tjlms.lesson etc

To integrate TJ-Fields with your client you need to add the code given below in respective files

#### 1) Model file of client (Product Model)

All the functions to retrieve form of custom/extra fields, form data and function to save data of fields are written in trait “TjfieldsFilterField”.

```php
// import the “Trait” file of TJ-Fields in the model file
JLoader::import('components.com_tjfields.filterFields', JPATH_SITE);

class [COMPONENT]Model[VIEW] extends JModelForm
{
	// Use imported Trait in model
	use TjfieldsFilterField;
}
```

#### 2) view.html.php file of view (view.html.php of product form view)

In view.html.php file you need to get the object of form of custom(Extra) fields created in TJ-Fields so that they can be displayed in the client view.

```php
// Call to form_exta function defied in TJ-Fields trait class

$input  = JFactory::getApplication()->input;

// Id of record to which the extra data is associated
$input->set("content_id", $id);
$model = $this->getModel();
$this->form_extra = $model->getFormExtra($data);
```
The function **getFormExtra()** will return the JForm object of extra fields.

Parameter to be passed to the function **getFormExtra()**

1. $data (Array)

```php
$data["category"] => // category of content
$data["clientComponent"] => // component name
$data["client"] => //[COMPONENT_NAME].[VIEW_NAME]
$data["view"] => [VIEW_NAME]
$data["layout"] => // Layout name
```

**Example**

```php
$this->form_extra = array();
$Quick2cartModelProduct = $this->getModel('product');

// Call to extra fields
$this->form_extra = $Quick2cartModelProduct->getFormExtra(
			array("category" => $this->itemDetail['category'],
				"clientComponent" => 'com_quick2cart',
				"client" => 'com_quick2cart.product',
				"view" => 'product',
				"layout" => 'new')
				);

$this->form_extra = array_filter($this->form_extra);
```
##### 3) Layout file of view

You need to add the layout file named **default_extrafields.php** in the **tmpl** folder of the view

Code to call the **default_extrafields** layout. (You need to paste this in default layout of the client form view)

```php
if (!empty($this->form_extra))
{
  echo $this->loadTemplate('extrafields');
}
```

Code for the **extrafields** layout

```php
<?php
/**
 * @package    TJ-Fields
 * 
 * @author     Techjoomla <extensions@techjoomla.com>
 * @copyright  Copyright (c) 2009-2019 TechJoomla. All rights reserved.
 * @license    GNU General Public License version 2 or later.
 */

// No direct access
defined('_JEXEC') or die;

$fieldsets_counter = 0;
$layout  = JFactory::getApplication()->input->get('layout');

if ($this->form_extra)
{
	// Iterate through the normal form fieldsets and display each one
	$fieldSets = $this->form_extra->getFieldsets();

	foreach ($fieldSets as $fieldset)
	{
		if (count($fieldSets) > 1)
		{
			if ($fieldsets_counter == 0)
			{
				echo JHtml::_('bootstrap.startTabSet', 'extrafields_myTab');
			}

			$fieldsets_counter++;

			if (count($this->form_extra->getFieldset($fieldset->name)))
			{
				foreach ($this->form_extra->getFieldset($fieldset->name) as $field)
				{
					if (!$field->hidden)
					{
						$tabName = JFilterOutput::stringURLUnicodeSlug(trim($fieldset->name));
						echo JHtml::_("bootstrap.addTab", "extrafields_myTab", $tabName, $fieldset->name);
						break;
					}
				}
			}
		}
		?>
		<div class="row">
			<?php
			// Iterate through the fields and display them
			foreach ($this->form_extra->getFieldset($fieldset->name) as $field)
			{
				if (!$field->hidden)
				{
					?>
					<div class="col-xs-12 col-md-6">
						<div class="form-group">
							<div class="col-sm-4 control-label">
								<?php echo $field->label; ?>
							</div>
							<div class="col-sm-8">
								<?php echo $field->input; ?>
							</div>
							<?php
							// TODO :- Check and remove
							if ($field->type == 'File')
							{
								?>
								<script type="text/javascript">
									jQuery(document).ready(function ()
									{
										var fieldValue = "<?php echo $field->value; ?>";
										var AttrRequired = jQuery('#<?php echo $field->id;?>').attr('required');
										if (typeof AttrRequired !== typeof undefined && AttrRequired !== false)
										{
											if (fieldValue)
											{
												jQuery('#<?php echo $field->id;?>').removeAttr("required");
												jQuery('#<?php echo $field->id;?>').removeClass("required");
											}
										}
									});
								</script>
							<?php
							}
							?>
						</div>
					</div>
				<?php
				}
			}
			?>
		</div>
		<?php

		if (count($fieldSets) > 1)
		{
			if (count($this->form_extra->getFieldset($fieldset->name)))
			{
				foreach ($this->form_extra->getFieldset($fieldset->name) as $field)
				{
					if (!$field->hidden)
					{
						echo JHtml::_("bootstrap.endTab");
						break;
					}
				}
			}
		}
	}

	if (count($fieldSets) > 1)
	{
		echo JHtml::_('bootstrap.endTabSet');
	}
}
else
{
	?>
	<div class="alert alert-info">
		<?php echo JText::_('COM_TJFIELDS_NO_EXTRA_FIELDS_FOUND');?>
	</div>
	<?php
}
```

##### 4) Model file of client (Form/View)

Till now we have rendered the custom(Extra) fields in the client form and now we will add the code to save the data of custom(Extra) fields in TJ-Fields tables. (Store prodcut's attribute values)

Add the line given below in the save() function of model file of client to save the data of custom fields.

```php
  $this->saveExtraFields($data)
```

Parameter to be passed to the function **saveExtraFields()**

1. $data (Array)

```php
	Array
	(
	    [content_id] => 1
	    [client] => com_quick2cart.product
	    [fieldsvalue] => Array
        		(
	            [com_quick2cart_product_color] => Array
            	    (
	                    [0] => red
            	    )

         		[com_quick2cart_product_Screensize] => 122
	            [com_quick2cart_product_size] => m
        		)
  )
```

##### 5) Model file of client (Form/View)

We are done with storing the data of custom fields, now we need to add the code to delete the data of custom fields when the parent record for the extra data is deleted. (When we delete the product we need to delete the extra fields data associalted with the product from the TJ-Fields tables)

Add the line given below in the delete() function of model file of client to delete the data of custom fields.

```php
$this->deleteExtraFieldsData($content_id, $client);
```

Parameters to be passed to the function **deleteExtraFieldsData()**

1) $content_id (Int) e.g Product Id
2) $client (String) e.g com_quick2cart.product

##### 6) Model file of client (Form/View)

If you want to use the **TJ-Fields Filter Module** to filter out the content of the client then you need to add the code given below

```php
JLoader::import('components.com_tjfields.helpers.tjfields', JPATH_SITE);

$tjFieldsHelper = new TjfieldsHelper;
$contentIds = $tjFieldsHelper->getFilterResults();
$client = JFactory::getApplication()->input->get('client', '', 'string');

if (!empty($client))
{
	if ($contentIds != '-2')
	{
		$query->where("a.item_id IN (" . $tjfieldItem_ids . ")");
	}
}
```
