---
title: BDD using Selenium 2, Maven and cucumber-jvm
author: Haroon Rasheed
layout: post
permalink: /bdd-using-selenium-2-maven-and-cucumber
comments: true
categories:
  - bdd
  - cucumber
  - cucumber-jvm
  - jruby
  - maven
  - selenium
  - selenium-rc
tags:
  - bdd
  - cucumber
  - selenium-rc
---
Recently I started looking at ways of writing Selenium functional tests for a set up using BDD. There are different tools available to write the tests such as easyb, scalatest & cucumber. This blog post will focus on setting up a test project using Selenium 2, maven and Cucumber.

Once we have set up the Maven project then we need to set up the POM file so that all the required jar files for Selenium and Cucumber are available to the project.

<!-- more -->
  
Following are the contents of the pom.xml file for my project.

{% codeblock lang: pom.xml%}
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>cucumberDemo</groupId>
    <artifactId>cucumberDemo</artifactId>
    <version>1.0-SNAPSHOT</version>

    <pluginRepositories>
        <pluginRepository>
            <id>cukes</id>
            <url>http://cukes.info/maven</url>
        </pluginRepository>
    </pluginRepositories>

    <dependencies>
        <dependency>
            <groupId>info.cukes</groupId>
            <artifactId>cucumber-picocontainer</artifactId>
            <version>1.1.1</version>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>info.cukes</groupId>
            <artifactId>cucumber-junit</artifactId>
            <version>1.1.1</version>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.10</version>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.hamcrest</groupId>
            <artifactId>hamcrest-all</artifactId>
            <version>1.1</version>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.seleniumhq.selenium</groupId>
            <artifactId>selenium-htmlunit-driver</artifactId>
            <version>2.25.0</version>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.seleniumhq.selenium</groupId>
            <artifactId>selenium-server</artifactId>
            <version>2.25.0</version>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.0</version>
                <configuration>
                    <source>1.7</source>
                    <target>1.7</target>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-surefire-plugin</artifactId>
                <version>2.12.2</version>
                <configuration>
                    <useFile>false</useFile>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>
{% endcodeblock%}

The project structure that I have created is as follows.

{% img /images/project_structure1.png %}

Now create a “GoogleSearch.feature” file under the features directory as.

	Feature: Google Search Feature
	In order to ensure that Google Search works
	I want to run a quick Hello World search.

	Scenario: Hello World Scenario
	Given The search is Hello World
	When The Search is performed
	Then The browser title should have Hello World


Now create a class file under the following package.

	src/test/java/cucumberdemo/GoogleSearchFeature.java

If you have not installed Cucumber before then for the first time you run this project you need to specify the following command.

	mvn clean integration-test

Following is the out put of doing this command.


	[INFO] Feature: Google Search Feature
	[INFO] In order to ensure that Google Search works
	[INFO] I want to run a quick Hello World search.
	[INFO]
	[INFO] Scenario: Hello World Scenario # features/GoogleSearch.feature:5
	[INFO] Given The search is Hello World # features/GoogleSearch.feature:6
	[INFO] When The Search is performed # features/GoogleSearch.feature:7
	[INFO] Then The browser title should have Hello World # features/GoogleSearch.feature:8
	[INFO]
	[INFO] 1 scenario (1 undefined)
	[INFO] 3 steps (3 undefined)
	[INFO] 0m0.334s
	INFO] You can implement step definitions for undefined steps with these snippets:
	[INFO]
	[INFO] @Given ("^The search is Hello World$")
	[INFO] @Pending
	[INFO] public void theSearchIsHelloWorld() {
	[INFO] }
	[INFO]
	[INFO] @When ("^The Search is performed$")
	[INFO] @Pending
	[INFO] public void theSearchIsPerformed() {
	[INFO] }
	[INFO]
	[INFO] @Then ("^The browser title should have Hello World$")
	[INFO] @Pending
	[INFO] public void theBrowserTitleShouldHaveHelloWorld() {
	[INFO] }


Now copy the step definitions from the command line interface and add these steps to the GoogleSearchFeature class file as follows. If you notice that there is an annotation @Pending, this would make sure that if the feature has not been implemented then Cucumber will ignore these tests. In the following class, I have removed the @Pending annotation and add Selenium 2 code.


{% codeblock lang: GoogleSearchSteps.java%}
import cucumber.api.java.en.Given;
import cucumber.api.java.en.Then;
import cucumber.api.java.en.When;
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.htmlunit.HtmlUnitDriver;

import static org.hamcrest.MatcherAssert.assertThat;
import static org.hamcrest.Matchers.containsString;


public class GoogleSearchSteps {

    WebDriver driver;

    @Given("^The search is Hello World$")
    public void theSearchIsHelloWorld() {
        driver = new HtmlUnitDriver();
        driver.get("http://www.google.co.uk");
        driver.findElement(By.xpath("//input[@name='q']")).sendKeys("Hello World");
    }

    @When("^The Search is performed$")
    public void theSearchIsPerformed() {
        driver.findElement(By.xpath("//input[@name='btnG']")).click();
    }

    @Then("^The browser title should have Hello World$")
    public void theBrowserTitleShouldHaveHelloWorld() {
        assertThat ("Browser title:",driver.getTitle(), containsString("Hello World"));
        driver.quit();
    }
}
{% endcodeblock%}

If you go the terminal again and run the following maven command again.

	mvn clean integration-test


You will see the tests running and once the tests are finished you can see the following test results in the project’s target directory.

{% img /images/cucumber_results.png %}
