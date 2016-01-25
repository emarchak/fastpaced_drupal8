# Fast Paced Drupal 8

_An overview of using composer, console and services to speed up development_

[github.com/emarchak/fastpaced-drupal8](http://github.com/emarchak/fastpaced-drupal8)
[github.com/emarchak/fastpaced-drupal8fastpaced_videos.git

[Myplanet](http://myplanet.com)

[@emarchak](http://twitter.com/emarchak)

# Why Change for D8?

1.  Speed up your development process
2.  Play well with others

## Overview

1. Build using Composer
2. Install using Console
3. Create a Module using Console
4. Export Content Types using Console
5. Export Views using Console
6. Create a Service using Console
7. Import Nodes using Guzzle
8. Create Admin Form using Console
9. Include your module using Composer

## Dependencies

* [Composer](https://getcomposer.org)
	* Pependency manager for PHP
* [Drupal Console](http://drupalconsole.com)
	* Extends Symfony console for boilerplates and interations.
* [Guzzle (in core)](http://docs.guzzlephp.org/)
	* Guzzle is a PHP HTTP client.
* Familiarity with Drupal 7 development process


# 1. Build using Composer

Download Drupal to a docroot so we can control the config.
Use console to install `composer_manager` which lets us use composer for drupal dependencies

	$ composer create-project drupal/drupal fastpaced/docroot 8.0.2
	$ cd fastpaced/docroot
	$ drupal module:download composer_manager
	$ drupal module:install composer_manager
	$ php modules/contrib/composer_manager/scripts/init.php


Add to installer paths to let us download modules to ./docroot/modules/contrib

	$ composer require composer/installers:~1.0 --update-no-dev
    
    "installer-paths": { "modules/contrib/{$name}": ["vendor/package”] }

Install some dependencies!

	$ composer require composer_manager:8.1.0-rc1
	$ composer require drupal/admin_toolbar:8.1.11
	$ composer require drupal/video_embed_field:8.1.0-rc3

# 2. Install using Console

	$ drupal site:install
	$ drupal module:install composer_manager
	$ drupal module:install admin_toolbar
	$ drupal module:install video_embed_field

Change sync directory in settings.php to export config to ./docroot/config/sync

	$config_directories['sync'] = '../config/sync';

# 3. Create a Module using Console

Let's create a fast-paced module...

	$ drupal generate:module
	
	Enter the new module name:
	> Fast paced videos 

.. and a speedy content type!	
   
    $ drupal generate:entity:bundle
    
    Enter the module name [admin_toolbar]:
	> fastpaced_videos

	Enter the machine name of your new content type [default]:
	> video
	
	$ drupal module:install fastpaced_videos
	
# 4. Export Content Types using Console
Log into the site and configure the content type as needed, before you export it.

	$ drupal config:export:content:type
	> video

	Export content type in module as an optional configuration (yes/no) [yes]:
	> no
	
Can we generate some example content? Yes we can.
	
	$ drupal create:nodes

# 5. Export Views using Console
Sadly, you still have to point-and-click your way through views.

But once that's done, we can export it in a flash!
	 
	$ drupal config:export:view
	View to be exported [Archive]:
	> Videos

	Export view in module as an optional configuration (yes/no) [yes]:
	> no

	Include view module dependencies in module info YAML file (yes/no) [yes]:
	> y


# 6. Create a Service using Console
We'll be importing videos using guzzle, so we'll need to create a service to save them.

	$ drupal generate:service
	Enter the service name [fastpaced_videos.default]:
	> fastpaced_videos.import

	Enter the Class name [DefaultService]:
	> ImportService

	Create an interface (yes/no) [yes]:
	> no

	Do you want to load services from the container (yes/no) [no]:
	> yes

	Enter your service [ ]:
	> entity.query
	> manager

Refer to the repo for further changes to fastpaced_videos/src/ImportServiceInterface.php

# 7. Import Nodes using Guzzle
Some parts of Drupal 8 are still procedural like 7. hook_theme and hook_cron are an example. 

Add a fastpaced_videos.module and fastpaced_videos_cron().

Refer to the repo for further changes to fastpaced_videos.module

# 8. Create Admin form using Console

	$ drupal generate:form:config
	
	Enter the Form Class name [DefaultForm]:
	> ImportSettingsForm	
	
	Do you want to generate a form structure? (yes/no) [yes]:
	> y
	
	Type:          number
	Input label:   Import Max
	Description:   Maximum amount of nodes to import pre cron run
	Default value: 10
	
	Type:          textfield
	Input label:   Search Terms
	Description:   Feed to import from
	Default value: macaframa
	
	Update routing file (yes/no) [yes]:
	> yes	
	
	
	Generate a menu link (yes/no) [yes]:
	> yes
	
	A title for the menu link [ImportSettingsForm]:
	> Fast Paced Import Settings
	
	Menu parent [system.admin_config_system]:
	> system.admin_config_services

	
# 9. Include your module using Composer


# Put the pedal to the medal

[@emarchak](http://twitter.com/emarchak) / [@myplanetHQ](http://twitter.com/myplanetHQ)

[github.com/emarchak/fastpaced-drupal8](http://github.com/emarchak/fastpaced-drupal8)

[github.com/emarchak/fastpaced-videos](http://github.com/emarchak/fastpaced-videos)
