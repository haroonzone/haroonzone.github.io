---
title: Tellurium and test automation process
author: Haroon Rasheed
layout: post
permalink: /tellurium-automation-process
comments: true
categories:
  - tellurium
  - TestNG
---
  
Today I will focus on how Tellurium fits into test automation of a large scale project and how easy and effective it is to use Tellurium.
Test automation with any testing tool involves following main stages.

**Planning Tests -> Developing Tests -> Running Tests -> Analysing Results**

**Planning Tests**

As a test automation engineer, this is the foremost thing you would do. We should treat any test automation project in the same way as we would treat a development project. We should keep all aspects of software engineering in mind such as Readability, Re usability, Modularity and Maintainability.

At this point you should define clear objectives for test automation, you also need to analyse the application under test and determine that which objects and operations are used by the business processes that you are going to automate. You also need to define the data set up and tear down so that your tests starts from a known state of the database and after the test execution tear down the data that it has created during test execution, so that you can run these tests on different environments. This would also ensure that your tests do not leave the database in inconsistent state.

<!-- more -->

There are many techniques available to do the database operations. I have been using DBUnit for this purposes and it is really a good open source tool to do the job.

The next step is to decide on organizing the tests. We have decided on using Tellurium as the tool of our choice for our web application, because Tellurium provides an easy and practical way to organize the web UI tests out of the box and we can focus on writing the new tests using the features available. Tellurium is a generic test automation framework that can be utilized on any web application test automation project. It means that we do not have to worry about writing a new framework for every application/project.

You can either use JUnit or TestNG with Tellurium to run the tests. We will be developing Tellurium tests using TestNG.

**Developing Tests**

You can create a new Tellurium and TestNG project using the instructions on the following URL.
<http://code.google.com/p/aost/wiki/TelluriumMavenArchetypes> 


Now you will have a project structure where you can build upon the features provided by Tellurium. The project structure is as follows. 

	pom.xml
	src
	src/main
	src/main/groovy
	src/main/resources
	src/test
	src/test/groovy
	src/test/groovy/module
	src/test/groovy/module/GoogleSearchModule.groovy
	src/test/groovy/test
	src/test/groovy/test/GoogleSearchTestCase.java
	src/test/resources
	TelluriumConfig.groovy

You can use the IDE of your own choice and import this as a maven project and it will download the Maven dependencies from the Tellurium repository.

This structure is extremely useful in organizing your tests for a large scale project. You can abstract the interactions with the UI of the application within ** src/test/groovy/module **. You define the UI of the application with object’s properties and define the common operations that you would do on those UI components.

We can divide writing the UI modules in two different operations.

1. Define the UI of the application.
2. Define common functions/business scenarios based on the UI module.

You can define the UI of the application using Firebug or IE Developer Toolbar to inspect the DOM structure of the application under tests and identify the unique properties which would help Tellurium in identifying the UI component at the run-time. Alternatively you can use Trump to generate the UI module of the application. Trump is available as a firefox plugin to Tellurium users and is of great help in defining the UI of the application and saving them as groovy files.

You can get more information about Trump on the following URL.
<http://code.google.com/p/aost/wiki/TrUMP>

To define the common reusable functions or business scenarios, you need to have discussions with the business analysts and product owners or refer to the requirements document.

e.g. UI module for GoogleSearchModule.groovy is as follows.

{% codeblock lang: GoogleSearchModule.groovy%}
class GoogleSearchModule extends DslContext{
	public void defineUi() {
		ui.Container(uid: "google_start_page", clocator: [tag: "td"], group: "true"){
		InputBox(uid: "searchbox", clocator: [title: "Google Search"])
		SubmitButton(uid: "googlesearch", clocator: [name: "btnG", value: "Google Search"])
		SubmitButton(uid: "Imfeelinglucky", clocator: [value: "I'm Feeling Lucky"])
		}
	}	

	def doGoogleSearch(String input){
		type "searchbox", input
		pause 500
		click "googlesearch"
		waitForPageToLoad 30000
	}	

	def doImFeelingLucky(String input){
		type "searchbox", input<br />
		pause 500
		click "Imfeelinglucky"
		waitForPageToLoad 30000
	}
}
{% endcodeblock%}
If you have noticed that as a test automation engineer you are not writing XPath expressions to define the UI of the application under tests, but rather you are using the unique properties of the UI objects to define the UI module and then defining reusable functions that you can use accross your tests and if there is any change in either the UI of the application or the behaviour of a search you just change this class file and this change will be available to your tests.

Next step is to create tests where you will use the UI module and common functions to do the actual testing. As stated earlier you have the option of using either JUnit or TestNG as test execution framework.

{% codeblock lang: GoogleSearchTestCase.groovy%}
public class GoogleSearchTestCase extends TelluriumTestNGTestCase {

	protected static GoogleSearchModule gsm;
	
	@BeforeClass
	public static void init() {
		gsm = new GoogleSearchModule();
		gsm.defineUi();
	}

	@Test
	public void testGoogleSearch(){
		connectUrl("http://www.google.com");
		gsm.doGoogleSearch("tellurium selenium Groovy Test");
	}

	@Test
	public void testGoogleSearchFeelingLucky(){
		connectUrl("http://www.google.com");
		gsm.doImFeelingLucky("tellurium selenium DSL Testing");
	}
}
{% endcodeblock%}
Once you have written the basic scenario of the test, then you can add assertions to the test to verify the behaviour of the application under test. To broaden the scope of your tests, you can replace the fixed values within the tests with the parameters and assert the behaviour of your application against different values. Tellurium provides the data driven testing out of the box.

**Running Tests**

There are multiple ways in which you can run Tellurium tests, some of them are listed below.

1. Use the TestNG plugin for different IDEs to run the tests.
2. Define a TestNG suite to run a group of tests.
3. Use maven (mvn test) goal to run the tests.
4. DSL executor, if your tests are in .dsl files.
5. You can use ANT to define the tasks to run the tests and then use these tasks.

**Analyzing Results**

The results generated by Tellurium depends on the option you select for running the tests. In case of maven you will get a surefire report with the failures.

TestNG plugin has its own mechanism of displaying the failed, passed and skipped tests.