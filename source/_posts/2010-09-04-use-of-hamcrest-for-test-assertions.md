---
title: Use of hamcrest for test assertions
author: Haroon Rasheed
layout: post
permalink: /blog/2010/09/use-of-hamcrest-for-test-assertions/
categories:
  - hamcrest
  - testng
tags:
  - hamcrest
  - testng
---
I was recently introduced to [hamcrest][1] (an open source matcher library). I was really impressed by the declarative nature of the match rules. We use TestNG to drive the Selenium Tests and naturally we are using TestNG assertions within our tests. Some of the TestNG assertions in our tests are as follows.

	assertEqual (thePage.getLinkCount(), 10, "Link count on the page");
	assertTrue (thePage.isLinkPresent(), "Link 1 is present.")

The equivalent assertions using hamcrest would be as follows.

	assertThat("Link count on the page", thePage.getLinkCount(), equalTo(10));
	assertThat("Link 1 is present", theBiscuit.isLinkPresent(), is(true));

You can see that the hamcrest matchers are more expressive and readable in the code. Another benfit of using hamcrest is that you can write custom matchers easily. e.g. you want to write an assertion in the test that checks that you have more than 20 items in your list and the item count is an even number.

	assertThat ("Country item count", countryList.getCount(), allOf(greaterThan(20), evenNumber()));

It is easy to write and much more readable in the tests and if the test fails then it gives a clear description of the failure. Another benefit of using hamcrest is that you as a test developer does not have to worry about the different in the method signature for TestNG and JUnit. 
 
 [1]: http://code.google.com/p/hamcrest/wiki/Tutorial