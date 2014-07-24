---
title: Using apache commons configuration to manage properties
author: Haroon Rasheed
layout: post
permalink: /blog/2012/10/using-apache-commons-configuration-to-manage-property-files/
categories:
  - commons configuration
  - maven
  - java
  - properties
tags:
  - commons configuration
  - java
  - properties
---
Property files or environment configuration files are an essential part of any system. For test automation, these files play an important role when you want to externalise the information such as application url, database hostname, database username etc.. These property files hold the information as a kay value pair and can be accessed when needed for the tests.

These are also vital when you want to run the tests against different environments, in this case you can have per environment property file and when running the tests you can pass an argument to load the specific property file so that the information about the environment is available to the tests.

Once you have per environment property file then the next step is to make sure that you only hold information about the specific environment in the respective property file and do not repeat the same set of properties in all these files. For example the following three files represent three different environments where you want to run your tests, and each environment will have a distinct application url, a username , a password and we are assuming that these are Selenium or WebDriver functional tests and you want to usefor your tests. Therefore the property files hold the information about Sauce Labs.

<!-- more -->

**functional-test-local.properties**
	app.url = http://localhost:8080/
	username = local.username
	password = local.password
	sauce.url = http://ondemand.saucelabs.com:80/wd/hub
	sauce.username = sauceUsername
	sauce.client.secret = sauceSecretKey


**functional-test-ci.properties**
	app.url = http://ci.app.com/
	username = ci.username
	password = ci.password
	sauce.url = http://ondemand.saucelabs.com:80/wd/hub
	sauce.username = sauceUsername
	sauce.client.secret = sauceSecretKey

**functional-test-staging.properties**
	app.url = http://staging.app.com/
	username = staging.username
	password = staging.password
	sauce.url = http://ondemand.saucelabs.com:80/wd/hub
	sauce.username = sauceUsername
	sauce.client.secret = sauceSecretKey

When you run the tests, you pass an argument to your tests to load a specific property file. So far, everything seems to be fine except that you can see that the information about the Sauce Labs is repeated in each of the property file, a better approach would be to have a common property file that can have the properties that are common across all the environments. In this case this information is about Sauce Labs.

Many of the web frameworks provide this mechanism out of the box where you can use these mechanism to define the order of loading the properties. If your tests are part of the application under test then you can use this to manage the configuration required for the tests. But sometimes your tests are not part of the application, e.g. we recently started testing force.com based application using WebDriver. In this case the tests are not part of the application, and we did not want to add a dependency to the a big web framework for just managing our property files.

came to our rescue, it provides a generic configuration interface which enables a Java application to read configuration data from a variety of sources.

We can use the *CompositeConfiguration* and hence our new property files will look like as follows.


**functional-test-common.properties**
	sauce.url = http://ondemand.saucelabs.com:80/wd/hub
	sauce.username = sauceUsername
	sauce.client.secret = sauceSecretKey

**functional-test-local.properties**
	app.url = http://localhost:8080/
	username = local.username
	password = local.password

**functional-test-ci.properties**
	app.url = http://ci.app.com/
	username = ci.username
	password = ci.password

**functional-test-staging.properties**
	app.url = http://staging.app.com/
	username = staging.username
	password = staging.password

We have added a new property file and as the name implies it will hold properties that are common across all the environments, in our case these are properties for Sauce Labs.

This also provides the ability to override the predefined properties. The order of precedence while loading the properties is as follows.

1.  System Properties
2.  Environment Specific properties
3.  Common properties

You can see the code at my GitHub repo (http://github.com/haroonzone/commons-configuration)

I hope this helps you in organising the property files and will reduce the unnecessary duplication.