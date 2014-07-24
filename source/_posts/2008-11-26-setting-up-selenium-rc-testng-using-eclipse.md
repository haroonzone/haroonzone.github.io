---
title: 'Setting up selenium &amp; TestNG using eclipse'
author: Haroon Rasheed
layout: post
permalink: /setting-up-selenium-rc-testng-using-eclipse
comments: true
categories:
  - selenium
  - testng
  - eclipse
tags:
  - selenium
  - selenium-rc
---
 
I found that it is not easy for the beginners to setup Selenium RC and run the GoogleTest.java example after downloading it from <a href="http://www.openqa.org">www.openqa.org</a> . The purpose of this post is to help beginners (new users to Selenium RC) help setting up Selenium RC with TestNG using Eclipse.

This assumes that you have done the following steps.

Download and install Eclipse ([www.eclipse.org][1])

Download the latest TestNG ([www.testng.org][2])

Download Selenium RC ([www.openqa.org][3])

Install TestNG plugin for eclipse (<http://testng.org/doc/download.html>)

Please follow the step by step instructions to setup the test environment.

<!-- more -->

Launch Eclipse, you can setup any directory as your default workspace. For this example, my default workspace is as shown below.

{% img /images/001.jpg 600 243 %}

Click OK, Eclipse will be launched.

Click File > New

{% img /images/002.jpg 600 509 %}

Set the project name as Google.

{% img /images/0032.jpg 600 614 %}


Click Next.

On the Source tab leave the default.

Click on Libraries.

Click on Add External Jars

Add the jar files for TestNG and Selenium RC Java client as shown below.

{% img /images/0042.jpg 600 558 %}

Click Finish, now you will have a Java project created in Eclipse that is correctly set to use Selenium Client and TestNG jar files.

Now Right Click on the src > New > Class

{% img /images/005.jpg 600 492 %}

Name the class file as GoogleTest and Click Finish.

Now create a testng test within GoogleTest class as follows.

{% img /images/006.jpg 600 300 %}
 

The next step is to run the Selenium Server as

{% img /images/007.jpg 600 120 %}

Now Right Click on the test and run this as TestNG test.

{% img /images/008.jpg 600 400 %}

The test will be successful.

{% img /images/009.jpg 600 150 %}

 [1]: http://www.eclipse.org
 [2]: http://www.testng.org
 [3]: http://www.openqa.org