---
date: 2017-01-15
title: TJ Reports - Integration Guide
categories:
  - TJ Reports
type: Document
---

# Installing the Default report configuration
The reporting plugin only offers the code needed for the report. For the report to be usable, it needs to be configured. The extension developer should install the reports they need when they install the extension

# Linking TJ reports from your own extension to show extension specific reports
Just like other infrastructure extensions, you can pass the client context to show reports related to your extension. For instance ‘ index.php?option=com_tjreports&client= tjlms’ would give reports contextual to Shika LMS. A Plugin will also have a context defined. For Plugins that have cross extension reports, an ‘All’ Client can be passed. 