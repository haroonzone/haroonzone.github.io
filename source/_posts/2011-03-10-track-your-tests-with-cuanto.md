---
title: Track your tests with Cuanto
author: Haroon Rasheed
layout: post
permalink: /track-your-tests-with-cuanto
comments: true
categories:
  - cuanto
  - testng
  - track your tests
  - selenium
tags:
  - cuanto
  - selenium-rc
  - testng
---
How often a developer has come to you and asked the following questions about a test failure. 


* When it started failing?
* Was it failing for the same reason before?


How do you then start looking at the previous test runs? Do you perform complex SQL to get this information out of a database if you are logging results there, or do you go through different builds in the CI tools to find this information?

<!-- more -->

In reality if you are doing extensive UI test automation using any tool like Selenium, Watir or Sahi. You will have some test failures that are either due to the environment being slow or some particular service not running in a complex system, and even sometimes the application wont respond in time no matter how intelligent your test scripts are without using the hard coded wait statements. 

Also Continuous integration tools are great and generate very good HTML reports but those HTML reports are tied to a build. It is not easy to navigate through each build and open up the test report to see the history of a particular test method, let alone verify the reason for the test failure. 

Beside this, once you have the HTML reports from the test run, you cannot store any analytical information about that test run anywhere until I found a very good tool for doing all this and much more. The name of the tool is [ Cuanto ][1]

I will go through the main features of Cuanto here and how we can use it to track TestNG tests. Cuanto supports TestNG, JUnit , NUnit and Manual test tracking, but I will go through the TestNG tests tracking. I have written a custom TestNG listener that uses Cuanto API to log the test results at the run-time. You can find more information about this listener on my [GitHub Repo][2]

When you run the tests using the TestNG listener, then you can analyse the test results using cuanto as follows.

Go to the Cuanto URL in my case it is http://localhost:8080/cuanto
You will see the test run information as below. 

{% img /images/test_run.png 1000 490 %}

This would show all the test runs for your project, now if you click on the first test run. It will open the view with all the tests that are run as part of this test run as shown below.

{% img /images/test_outcome.png 1000 350 %}

In this view, you have all the useful information about your tests such as test history, output, reason for the test failure. It also indicates if there is a new failure. 

When you click the History icon for any test method, then you will be able to view the historic information about the tests as how often this test have failed and what was the reason for the failure each time. 

You can sort the tests based on the duration and it would help you in identifying you slow tests easily then you can make informed decision about these tests. 

There is much more to Cuanto then this and it has the potential to become a very useful repository for the historical information about your tests.
You can find out more about Cuanto on their website. They are working on a new Cuanto 2.8.0 version which is in beta at the moment. I have not looked at the change log but it is encouraging that people are still working on it.

 [1]: http://www.trackyourtests.com/
 [2]: https://github.com/haroonzone/cuanto-testng-listener