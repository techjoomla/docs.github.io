---
date: 2018-09-06
title: Integrating TJ Vendors with a CPG Plugin
description: REST API framework for Joomla
categories:
  - TJ Vendor
tags:
  - Joomla
  - vendor
type: Document
nav_ordering: 1
showSidebar: true
published: true
pageTitle: "Integrating TJ Vendors with a CPG Plugin"
permalink: tj-vendor/payment-plugin-integration.html
---

Integrating payment plugins with TJ Vendor will allow each vendor to configure their payment information which will be used for receiving payments. The vendor can add/edit his payment information from the vendor edit page. 

## Supporting vendor specific payment configuration
To integrate with any payment plugin for eg: paypal, create a jform xml in the path - `paypal/paypal/form/paypal.xml` which will have the fields that need to be filled in by the vendor for saving payment information.

When any extension implements multi-vendor then the extension will now provide the vendor's payment configuration values to the payment plugin code. 

## Vendor flow
When adding or editing a vendor profile, in the payment Information tab, you will see paypal in the list. Once you choose paypal, you will see the fields that were defined in the XML.

