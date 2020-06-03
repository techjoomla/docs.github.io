---
title:       Integrating TJ Vendors with a CPG Plugin
description: Integrating TJ Vendors with a CPG Plugin
path:        blob/master/docs/tj-reports
source:      tjvendors-integrating-with-cpg-plugin.md
hero:        TJVendors - Integrating TJ Vendors with a CPG Plugin
date:        2020-04-22
categories:
  - TJVendors
tags:
  - Joomla
  - TJVendors
  - com_tjvendors
---


Integrating payment plugins with TJ Vendor will allow each vendor to configure their payment information which will be used for receiving payments. The vendor can add/edit his payment information from the vendor edit page.

## Supporting vendor specific payment configuration
To integrate with any payment plugin for eg: paypal, create a jform xml in the path - `paypal/paypal/form/paypal.xml` which will have the fields that need to be filled in by the vendor for saving payment information.

When any extension implements multi-vendor then the extension will now provide the vendor's payment configuration values to the payment plugin code.

## Vendor flow
When adding or editing a vendor profile, in the payment Information tab, you will see paypal in the list. Once you choose paypal, you will see the fields that were defined in the XML.
