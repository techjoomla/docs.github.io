---
date: 2020-04-22
title: Add new field Type 
description: Steps to add support of new field type
categories:
  - TJ - Fields
tags:
  - Joomla
  - Fields
  - TJ - Fields
type: Document
nav_ordering: 3
showSidebar: true
published: true
pageTitle: "Add new field Type "
permalink: tj-fields/com-tjfields-add-new-field-type.html
---

## How to add new field type?

TJ-Fields supports many of the core field types provided by Joomla and it provides a way to easily add new field type or any new custom field type as per the requirement. 

Follow the steps given below to add new field type support in TJ-Fields.

#### 1) Add entry of new field type in “tjfieldfields” field

This is to add the new field type in the list of available field types. The field “tjfieldfields” is a custom field in TJ-Fields which is used to generate the HTML of dropdown to show the list of available field types.

To add option of the new field type in the “tjfieldfields” field add the code given below in the function **getInput()** which is in the file on the path specified below. (Replace the ‘FIELD_TYPE' and ‘LANGUAGE_CONSTANT’ with field type name and language constant for the label of field type respectively.

```php
$options[] = JHtml::_('select.option', 'FIELD_TYPE', JText::_('LANGUAGE_CONSTANT'));
```

File Path

`YOUR_JOOMLA/administrator/components/com_tjfields/models/fields/tjfieldfields.php`

#### 2) Add XML to define attributes of the new field type (Only if your field type has any attribute)

Attributes of the field are the properties of field type which defines it e.g (For input type text name, size, value, maxlength etc are the attributes)

To add attributes for the new field type create a new XML file with the name same as the name of the new field type in the folder given below.

Folder Path

`YOUR_JOOMLA/administrator/components/com_tjfields/models/forms/types/forms`

Sample XML File
```xml
<?xml version="1.0" encoding="utf-8"?>

<form>
	<fields name="params" label="COM_TJFIELDS_FIELD_BASIC_LABEL">
		<fieldset name="params">
			<field name="default" type="text" label="COM_TJFIELDS_FORM_LBL_FIELD_DEFAULT" description="COM_TJFIELDS_FORM_DESC_FIELD_DEFAULT"/>
			<field name="size" type="text" label="COM_TJFIELDS_FORM_LBL_FIELD_SIZE" description="COM_TJFIELDS_FORM_DESC_FIELD_SIZE"/>
			<field name="maxlength" type="text" label="COM_TJFIELDS_FORM_LBL_FIELD_MAXLENGTH" description="COM_TJFIELDS_FORM_DESC_FIELD_SIZE"/>
		</fieldset>
	</fields>
</form>
```

Add all the required attributes of new field under **fieldset** named **params**.

!!! note
	There is **no need** to add basic attributes like “name”, “type”, “label” and “description” in the XML file.

#### 3) Create form field file (Only if you are creating custom field which is not supported by Joomla)

Create the form field file in the folder on path given below to define the rendering of the field on the form.

Folder Path

`YOUR_JOOMLA/administrator/components/com_tjfields/models/fields`

Sample field file (Textarea Counter)

```php
<?php
/**
 * @package    TJFields
 * 
 * @author     Techjoomla <extensions@techjoomla.com>
 * @copyright  Copyright (c) 2009-2019 TechJoomla. All rights reserved
 * @license    GNU General Public License version 2, or later
 */

defined('JPATH_PLATFORM') or die;

JLoader::register('JFormFieldTextarea', JPATH_BASE . '/libraries/joomla/form/fields/textarea.php');

/**
 * Form Field Textareacounter class
 * Supports a multi line area for entry of plain text with count char
 *
 * @since  11.1
 */
class JFormFieldTextareacounter extends JFormFieldTextarea
{
	/**
	 * The form field type.
	 *
	 * @var    string
	 * @since  11.1
	 */
	protected $type = 'Textareacounter';

	/**
	 * The number of rows in textarea.
	 *
	 * @var    mixed
	 * @since  3.2
	 */
	protected $rows;

	/**
	 * The number of columns in textarea.
	 *
	 * @var    mixed
	 * @since  3.2
	 */
	protected $columns;

	/**
	 * The maximum number of characters in textarea.
	 *
	 * @var    mixed
	 * @since  3.4
	 */
	protected $maxlength;

	/**
	 * The minimum number of characters in textarea.
	 *
	 * @var    mixed
	 * @since  3.4
	 */
	protected $minlength;

	/**
	 * The minimum number of characters in textarea countertext.
	 *
	 * @var    mixed
	 * @since  3.4
	 */
	protected $countertext;

	/**
	 * The textarea input field id.
	 *
	 * @var    int
	 * @since  3.4
	 */
	protected $id;

	/**
	 * The class for textarea counter input field
	 *
	 * @var    mixed
	 * @since  3.4
	 */
	protected $class;

	/**
	 * Method to attach a JForm object to the field.
	 *
	 * @param   SimpleXMLElement  $element  The SimpleXMLElement object representing the <field /> tag for the form field object.
	 * @param   mixed             $value    The form field value to validate.
	 * @param   string            $group    The field name group control value. This acts as as an array container for the field.
	 *                                      For example if the field has name="foo" and the group value is set to "bar" then the
	 *                                      full field name would end up being "bar[foo]".
	 *
	 * @return  boolean  True on success.
	 *
	 * @see     JFormField::setup()
	 * @since   3.2
	 */
	public function setup(SimpleXMLElement $element, $value, $group = null)
	{
		$return = parent::setup($element, $value, $group);

		if ($return)
		{
			$this->countertext = isset($this->element['countertext']) ? (string) $this->element['countertext'] : '';
			$this->countertext = JText::_($this->countertext);
			$this->maxlength = isset($this->element['maxlength']) ? (int) $this->element['maxlength'] : 0;
			$this->minlength = isset($this->element['minlength']) ? (int) $this->element['minlength'] : 0;
			$this->class .= ' tjfieldTextareacounter';

			if (!empty($this->minlength))
			{
				$this->class .= ' validate-min' . $this->minlength;
			}
		}

		return $return;
	}

	/**
	 * Method to get the textarea field input markup.
	 * Use the rows and columns attributes to specify the dimensions of the area.
	 *
	 * @return  string  The field input markup.
	 *
	 * @since   11.1
	 */
	protected function getInput()
	{
		$html = parent::getInput();
		$html .= $this->getCounterMask();

		// Using the .tjfieldTextareacounter selector
		$doc = JFactory::getDocument();
		$doc->addScriptDeclaration('
			jQuery(document).ready(function() {

				jQuery(".tjfieldTextareacounter").each(function() {
					let usedcharlength = parseInt(jQuery(this).val().length);
					let maxlength = parseInt(jQuery(this).siblings("span").find(".charscontainer_maxlength").text());
					jQuery(this).siblings("span").find(".charscontainer_filled").text(usedcharlength);
				})

				jQuery("#' . $this->id . '").on("keyup", function() {
					jQuery("#usedchars_' . $this->id . '").text(jQuery("#' . $this->id . '").val().length);
					jQuery("#filledchars_' . $this->id . '").text(jQuery("#' . $this->id . '").val().length);
				});

				document.formvalidator.setHandler("blank-space", function(value, element) {
					if (value.trim() == "") {
						return false;
					}
					return true;
				});
			});
		');

		if (!empty($this->minlength))
		{
			$doc->addScriptDeclaration('
				jQuery(document).ready(function() {
					document.formvalidator.setHandler("min' . $this->minlength . '", function(value) {
						return (value.trim().length >=' . $this->minlength . ');
					});
				});
			');
		}

		return $html;
	}

	/**
	 * Method to get the textarea field char count container.
	 *
	 * @return  string  The field input markup.
	 *
	 * @since   11.1
	 */
	private function getCounterMask()
	{
		$text = '<span class="charscontainer pull-left" id="charscontainer_' . $this->id . '">';

		if (!empty($this->maxlength))
		{
			$text .= $this->countertext;

			$text = str_replace('{used}', '<span class="charscontainer_used" id="usedchars_' . $this->id . '">0</span>', $text);
			$text = str_replace('{filled}',
								'<span class="charscontainer_filled" id="filledchars_' . $this->id . '">' . $this->maxlength . '</span>',
								$text
								);
			$text = str_replace('{maxlength}',
							'<span class="charscontainer_maxlength" id="maxlength_' . $this->id . '">' . $this->maxlength . '</span>',
							$text
							);
		}

		$text .= '</span>';

		return $text;
	}
}
```
