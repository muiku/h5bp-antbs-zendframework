# The H5BP ant build script Zend Framework integration
[![Build Status](https://secure.travis-ci.org/muiku/h5bp-antbs-zendframework.png?branch=zfint)](http://travis-ci.org/muiku/h5bp-antbs-zendframework)

### Install

    cd my-zfproject/
    wget --no-check-certificate https://github.com/muiku/h5bp-antbs-zendframework/tarball/zfint -O - | tar -xvz --strip 1
    
## Development

Just edit any file as you have done before:

- Put your styles into public/css/style.css
- Add some images under public/img/
- JavaScript goes into public/js/scripts.js
- etc. etc.

## Deployment (Apache 2)

### Step 1. Run the H5BP build script

    cd public/build/
    ant # The H5BP build script will begin to run and compress your files.

This creates three directories:

- public/publish/
- public/intermediate/
- application/layouts/scripts/publish/

### Step 2. Edit application.ini

Change the production layouts folder in application.ini

    [production]
    phpSettings.display_startup_errors = 0
    phpSettings.display_errors = 0
    includePaths.library = APPLICATION_PATH "/../library"
    bootstrap.path = APPLICATION_PATH "/Bootstrap.php"
    bootstrap.class = "Bootstrap"
    appnamespace = "Application"
    resources.frontController.controllerDirectory = APPLICATION_PATH "/controllers"
    resources.frontController.params.displayExceptions = 0

    ; Published (H5BP build script generated) layouts
    resources.layout.layoutPath = APPLICATION_PATH "/layouts/scripts/publish"

    ; Ensure that view encoding is UTF-8 and that view helpers use HTML5
    resources.view.encoding = "UTF-8"
    resources.view.doctype = "HTML5"

    [staging : production]

    [testing : production]
    phpSettings.display_startup_errors = 1
    phpSettings.display_errors = 1
    resources.layout.layoutPath = APPLICATION_PATH "/layouts/scripts"

    [development : production]
    phpSettings.display_startup_errors = 1
    phpSettings.display_errors = 1
    resources.frontController.params.displayExceptions = 1
    resources.layout.layoutPath = APPLICATION_PATH "/layouts/scripts"

### Step 3. Change the site docroot to point to public/publish/

    <VirtualHost *:80>
        # ...

        SetEnv APPLICATION_ENV production

    	DocumentRoot /home/me/workspace/myproject/public/publish
        DirectoryIndex index.php

        <Directory /home/me/workspace/myproject/public/publish>
	    	AllowOverride All
		    Order allow,deny
    		Allow from all
	    </Directory>

        # ...
    </VirtualHost>

## Requirements

The H5BP build script requires ant version 1.8.2. Debian 6 repositories include only version 1.8 so you have to improvise.

    mkdir ~/local ~/bin && cd ~/local
    wget http://www.nic.funet.fi/pub/mirrors/apache.org//ant/binaries/apache-ant-1.8.2-bin.tar.gz
    tar -zxvf apache-ant-1.8.2-bin.tar.gz
    cd ~/bin
    ln -s ~/local/apache-ant-1.8.2/bin/ant
    
## Dig further

- [How to use HTML5 Boilerplate with Zend Framework](http://blog.muiku.com/2012/03/10/how-to-use-html5-boilerplate-with-zend-framework) - blog post
