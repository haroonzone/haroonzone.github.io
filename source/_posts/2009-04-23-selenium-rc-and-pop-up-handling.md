---
title: Selenium RC and Pop-up handling
author: Haroon Rasheed
layout: post
permalink: /selenium-rc-and-pop-up-handling/
comments: true
categories:
  - selenium
  - testng
  - selenium & popups
tags:
  - selenium and popups
  - selenium-rc
---

Recently I have been asked by one of the readers about the pop-up handling in Selenium in general and Selenium RC in specific. As per my understanding Selenium IDE does not support recording on the pop-ups because If the popup is a new window, i.e., not in the html body of the main winodw, Selenium IDE, Dom Inspector, and other Firefox plugins do not work. The reason is there are multiple document variables to handle and it might not be a good idea to record any popup window which make the event listener more complicated.

<!-- more -->

You can use Selenium RC to handle the pop ups. There are different methods available to deal with different types of pop ups. Following are some methods that I have used to handle JavaScript alerts using Selenium RC java client.

	selenium.chooseCancelOnNextConfirmation();
	selenium.chooseOkOnNextConfirmation();

These method calls should be executed before the event that generates the JavaScript alerts. e.g. if clicking a Delete button pops up a java script alert with OK and Cancel button then your functions call would be. 

	selenium.chooseOkOnNextConfirmation();
	selenium.click(buttonLocator);

This would instruct selenium to click OK on the pop up that appears as a result of clicking the delete button. You can also assert on the pop up in the next statement. There is also a function available to verify that the alert is present.

	isAlertPresent() - this would return a boolean (true/false) based on the alert existence

I have not used Selenium RC to verify the login dialogs but there are some functions available to handle pop ups like these.

e.g The following code will use the selectWindow, windowFocus and close functions to work on a pop up/dialog
	selenium.selectWindow("window id");
	selenium.windowFocus();
	selenium.type("username", "user name");
	selenium.type("password", "password");
	selenium.click("logingButton");
	selenium.waitForPageToLoad("30000");
	selenium.close();

Sometimes you might need to assert on the pop ups. e.g. you might want to assert that on deleting a button a pop up appears. You can use waitForPopUp to make sure that your test script waits for the pop up to appear and does not give you a time out error.

assertEquals("Delete Confirmation", selenium.getAlert());
selenium.waitForPopUp("deleteWindowLocator", "30000");
selenium.selectWindow("deleteWindowLocator");

If you have custom defined pop-ups then you might want to use  Java Robot to do deal with custom define pop-ups. The Robot class is defined in java.awt package. You can control the mouse and keyboard events using Robot class. It is not an elegant solution but it works.

You can also use some other tools (AutoIt) to keep listening for certain events such as SSL security certificates or download dialogs and click on OK whenever these type of pop up appear.

There is another way of dealing with the file download pop ups. You can define a custom firefox profile and disable the file download pop up, so that it never appears during the test execution but still downloads the stuff on the hard disk which you can verify for testing purposes. You then launch selenium server with this custom firefox profile.

There can be many ways to acheive the same task. e.g. you can define a JavaScript function to handle the pop ups and make that available to Selenium RC via user extensions. This is the beauty of open source tools that you have source code available to you and you can tweak things around according to your own requirements.

I hope that it will help you in dealing with pop ups. Please feel free to comment and ask questions.