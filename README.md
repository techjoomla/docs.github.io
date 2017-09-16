
# Techjoomla Documentation Site

## Git Fork Setup on local machine

1. Fork the repo <a href="https://github.com/techjoomla/techjoomla.github.io" target="_blank">Techjoomla Documentation Repo</a>
2. Git clone your fork to your machine. Command Syntax - ```git clone fork-link```
3. Add remote upstream by command ```git remote add upstream git@github.com:techjoomla/techjoomla.github.io.git```
4. Check upstream with command ```git remote -v```
5. Make filemode off in config file. File Path - ```/techjoomla.github.io.git/.git/config``` . Make sure you can see the .git hidden folder by ```Ctrl + h```

## Installing requirements

1. Install Ruby - ```sudo apt-get install ruby-full```.
2. Check Ruby installed correctly by command - ```ruby -v```. It should be minimum 2.1.0
3. Install Bundler - ```sudo apt-get install bundler```.
4. Check Bundler installed correctly by command - ```bundle -v```. It should be minimum 1.15.3
5. Go to the root folder of project. i.e. ```/techjoomla.github.io```
6. Install Rubygems - ```sudo bundle install```.

## Basic Usage

### Adding a new post

1. Start the development server with command ```bundle exec jekyll serve```
2. It will give you the link where you can see the changes.
3. Inside _post folder create a new md file format - ```YYYY-MM-DD-filename.md```
4. Open the same file you created and add front matter -
     ``` ---
          date: 2017-01-15
          title: Introduction
          description: REST API framework for Joomla
          categories:
            - Joomla REST API
          type: Document
        --- 
     ```



