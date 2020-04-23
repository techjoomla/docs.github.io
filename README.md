
# Techjoomla Documentation Site

Steps to contribute documentation to the Techjoomla Site. 

## Git Fork Setup on local machine

1. Fork the repo <a href="https://github.com/techjoomla/techjoomla.github.io" target="_blank">Techjoomla Documentation Repo</a>
2. Git clone your fork to your machine. Command Syntax - ```git clone fork-link```
3. Add remote upstream by command ```git remote add upstream git@github.com:techjoomla/techjoomla.github.io.git```
4. Check upstream with command ```git remote -v```
5. Make filemode off in config file. File Path - ```/techjoomla.github.io.git/.git/config``` . Make sure you can see the .git hidden folder by ```Ctrl + h```

## Installing requirements

1. Install python, pip.
2. Check python and pip installed correctly by command - ```python --version and pip --version```. It should be ```minimum 2.7```
3. Install mkdocs- ```$ sudo pip3 install mkdocs```.
4. Check mkdocs installed correctly by command - ```mkdocs --version```. 
5. Install mkdocs Theme by command -```$ sudo pip3 install mkdocs-material```.


## Basic Usage

### Adding a New Post

1.  Open the terminal Go to the folder where the cloned project is stored. 
1. Start the development server with command ```mkdocs serve```
2. It will give you the link where you can see the changes. In most of the cases it is ```http://127.0.0.1:8000/```
3. Inside docs folder create a new folder and in that folder create md file format ```filename.md```
4. Open the .md file you created and add front matter 
     ``` 
        ---
        date: 2017-01-15
        title: Post Title
        description: Post Description
        categories:
          - CategoryName
        Tags:
 - Tag 1
            - Tag 2
        type: Document
        nav_ordering: 2
        showSidebar: true
        published: true
        pageTitle:”Page Title”
        permalink:foldername/filename.html

        --- 
     ```
    title & description are used as meta data of page and also used on homepage and related post data. showSidebar adds page on homepage and sidebar, in case of its absence or false value it will not be added. nav_ordering is the ordering value of the menu under its category. 
5. This will add a section on homepage with Category name and a post under it. The post will be generated with url - ```domainname.com/categoryname/filename```

 ### Adding a Menu Item

1. Open ```mkdocs.yml``` file.
2. Add Menu item - 
    ```
	Nav:
		MenuTtitle: foldername/.md file name
    ```
## Understanding Folder Structure

| **Folder Name** | Description                                                 |
|:----------------------:|:-------------------------------------------------------:|
| **docs**              | Contains documentation source files         |
| **site**                | Contains all folders                                     |
| **mkdocs.yml**   | Contains folders/Subfolders from docs dir |




