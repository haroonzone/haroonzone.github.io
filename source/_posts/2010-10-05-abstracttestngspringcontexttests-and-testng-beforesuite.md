---
title: AbstractTestNGSpringContextTests and TestNG @BeforeSuite
author: Haroon Rasheed
layout: post
permalink: /blog/2010/10/abstracttestngspringcontexttests-and-testng-beforesuite/
categories:
  - selenium
  - spring
  - testng
tags:
  - spring 2.5
  - testng
---
  
We are using Spring 2.5, TestNG and Selenium RC for our functional tests. In out test base class we are using @BeforeClass to get the new browser session from Selenium Server. Today I wanted to replace @BeforeClass with @BeforeSuite annotation, so that we only launch browser before running the test suite and this browser session would be shared among the tests thus reducing the test execution time.

I thought it to be a straight forward task by just replacing the @BeforeClass with the @BeforeSuite annotation, because these annotations are already provided by TestNG.

But when I did that I got null pointer exceptions all over the place, the code was complaining about the classes instantiated via Spring Application Context being null, and when I looked at the AbstractTestNGSpringContextTests implementation I found that AbstractTestNGSpringContextTests’s springTestContextPrepareTestInstance() method is currently annotated with @BeforeClass, which does not allow @BeforeSuite methods in subclasses from interacting with the ApplicationContext.

To overcome this problem, you can override the springTestContextPrepareTestInstance() method in your subclass as follows.

{% codeblock lang: java%}
@BeforeSuite(alwaysRun = true)
@BeforeClass(alwaysRun = true)
@BeforeTest(alwaysRun = true)
@Override
protected void springTestContextPrepareTestInstance() throws Exception {
        super.springTestContextPrepareTestInstance();
}
{% endcodeblock%}