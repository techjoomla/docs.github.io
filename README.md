# How to contribute to https://docs.techjoomla.com site

## Pre-requisites

1. This site runs on Python based mkdocs framework, to use that install Python, pip. [Refer for details](https://www.mkdocs.org/#manual-installation)

2. Check python and pip installed correctly by command -

`user@pc:~$ python --version` Python versions 3.5, 3.6, 3.7, 3.8 are supported

and

`user@pc:~$ pip --version`.


3. Create folder for Python virtual environments in lets say home folder.

`user@pc:~$ cd ~/`

`user@pc:~$ mkdir Python_environments`

4. Create a virtual environment in above folder.

`user@pc:~$ python3 -m venv ~/Python_environments/docs.github.io-mkdoks`

5. Activate this virtual environment by running command

`user@pc:~$ source ~/Python_environments/docs.github.io-mkdoks/bin/activate`

6. Install wheel, mkdocs on this virtual env.
`(docs.github.io-mkdoks) user@pc:~$ python3 -m pip install wheel`

`(docs.github.io-mkdoks) user@pc:~$ python3 -m pip install mkdocs`.

7. Check mkdocs installed correctly by command

`(docs.github.io-mkdoks) user@pc:~$ mkdocs --version`

8. Install mkdocs-material Theme by command

`(docs.github.io-mkdoks) user@pc:~$ python3 -m pip install mkdocs-material`

9. Install other plugins needed

`(docs.github.io-mkdoks) user@pc:~$ python3 -m pip install mkdocs-minify-plugin`

`(docs.github.io-mkdoks) user@pc:~$ python3 -m pip install mkdocs-git-revision-date-localized-plugin`

Steps to contribute documentation to the Techjoomla Site.

## Understanding repos

1. This repo containts the source code of the docs https://github.com/techjoomla/docs.github.io
2. This repo has the static files generated for the docs https://github.com/techjoomla/techjoomla.github.io

Repo 1. is where you will work on

## Fork and clone repos on local machine

1. Fork repo https://github.com/techjoomla/docs.github.io
2. Git clone your fork to your machine. Command Syntax - `git clone fork-link`
3. Add remote upstream by command `git remote add upstream git@github.com:techjoomla/docs.github.io.git`
4. Make filemode to false `git config core.filemode false`

## Basic Usage

### Start local development server
1. Open the terminal Go to the folder where the cloned project is stored.

2. Activate this virtual environment by running command

`user@pc:~$ source ~/Python_environments/docs.github.io-mkdoks/bin/activate`

3. Start the development server with command

`(docs.github.io-mkdoks) user@pc:~$ mkdocs serve`

4. It will give you the link where you can see the changes. In most of the cases it is `http://127.0.0.1:8000/`

### Understanding folder structure of docs.github.io.git

| **Folder Name** | Description                                      |
|:---------------:|:------------------------------------------------:|
| **docs**        | Contains documentation source files              |
| **site**        | Contains generated static files to render site   |
| **mkdocs.yml**  | Contains folders/Subfolders from docs dir        |

### To add a new post
1. Inside docs folder create a new folder and in that folder create md file format `filename.md`

lets say you added as
`~/repo/docs/tj-reports/tjreports-introduction.md`

4. Open the .md file you created and add front matter

```
---
title:       TJReports Introduction
description: Introduction to TJReports (com_tjreports), a reports manager for Joomla
path:        blob/master/docs/tj-reports
source:      tjreports-introduction.md
hero:        TJReports - Introduction
date:        2020-04-22
categories:
  - TJReports
tags:
  - Joomla
  - TJReports
  - com_tjreports
---

# Add your doc changes here
```

### To add a navaigation link

1. Open `mkdocs.yml` file.

2. Add Menu item -

For example

```
nav:
  - TJReports:
    - Introduction: tj-reports/tjreports-introduction.md
```
