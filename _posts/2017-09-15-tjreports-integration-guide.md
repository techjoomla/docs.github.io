---
date: 2017-01-15
title: Integration Guide
categories:
  - TJ Reports
type: Document
---

# TJ Reports Integration Guide

## Installing the default instances
The reporting plugin only offers the code needed for the report. For the report to be usable, it needs to be configured. An extension developer who wishes to create default 'instances' for the plugins can do that by adding the below method in the installation script.

## Linking to TJ reports
Just like other infrastructure extensions, you can pass the client context to show reports related to your extension. For instance Shika uses `index.php?option=com_tjreports&client=tjlms` for reports contextual to Shika LMS. The client is be specified when creating instances.