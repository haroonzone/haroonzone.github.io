---
title: Setting up a test project with Selenium, TestNG and Maven
author: Haroon Rasheed
layout: post
permalink: /blog/2010/05/setting-up-a-test-project-with-selenium-testng-and-maven/
categories:
  - maven
  - selenium
  - selenium-rc
  - TestNG
tags:
  - selenium-rc
---
  
It is useful to Selenium and TestNG using an IDE, but in the real world projects are not restricted to the use of IDE. Projects need build tools like ANT or MAVEN to compile, install, deploy and run the application.

I am involved in couple of projects that use Maven as the build tool, although Maven is more than just the build tool. It covers the complete life-cycle of the project and testing is a pivotal part in project life-cycle especially for projects that tend to follow TDD and Acceptance Test Driven Development.

In order to achieve this you need to install java and maven2 on your machine. Make sure that you have set the JDK\_HOME and M2\_HOME directory correctly.

Once you have done this, you can open a terminal on your screen and go to your default workspace or the directory where you want to create the project.

To create a maven project, execute the following.
  mvn archetype:create -DgroupId=com.pragmaticqa.tests -DartifactId=functionalTests`

You will notice maven doing its magic and creating a directory structure for you. Once it is complete do a list in the current working directory and you will notice a directory called functionalTests in your workspace. The directory structure will be as follows.


    |-functionalTests
    |--src
    |----main
    |------java
    |--------com
    |----------pragmaticqa
    |------------tests
    |----test
    |------java
    |--------com
    |----------pragmaticqa
    |------------tests


There is a debate going on these days about Convention over Configuration. Maven off course support Convention rather than project specific configuration. The structure of a maven project will always be similar to the structure shown above. All the application specific code resides in the src/main directory and the tests will be in the src/test directory with their respective resources.

In any maven project POM plays an important part in organizing the project phases and project dependencies. The contents of the sample POM in the functionalTest project are as follows.

I would highly recommend that you familiarize yourself with some basic concepts of maven like packaging, artifacts and version information. Clearly from the POM file below, the version of our application is 1.0-SNAPSHOT and the artifiact ID is functionalTests.

We are interested in the dependencies, in this section you can define the external modules or other projects that you need to make available to your project. In our case we are interested in adding TestNG and Selenium RC support to our project. Therefore we will remove the JUnit dependency and add TestNG and Selenium RC. After doing this our POM contents will be as follows.

{% codeblock lang: pom.xml%}
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.pragmaticqa.tests</groupId>
  <artifactId>functionalTests</artifactId>
  <packaging>jar</packaging>
  <version>1.0-SNAPSHOT</version>
  <name>functionalTests</name>
  <url>http://maven.apache.org</url>

  <dependencies>
    <dependency>
      <groupId>org.testng</groupId>
      <artifactId>testng</artifactId>
      <classifier>jdk15</classifier>
      <version>5.11</version>
    </dependency>
    <dependency>
      <groupId>org.seleniumhq.selenium.client-drivers</groupId>
      <artifactId>selenium-java-client-driver</artifactId>
      <version>1.0.3</version>
    </dependency>
  </dependencies>

    <build>
        <plugins>
            <plugin>
                 <groupId>org.apache.maven.plugins</groupId>
                 <artifactId>maven-compiler-plugin</artifactId>
                <configuration>
                    <source>1.5</source>
                    <target>1.5</target>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.codehaus.mojo</groupId>
                <artifactId>selenium-maven-plugin</artifactId>
                <executions>
                    <execution>
                        <phase>pre-integration-test</phase>
                        <goals>
                            <goal>start-server</goal>
                        </goals>
                        <configuration>
                            <background>true</background>
                        </configuration>
                    </execution>
                    <execution>
                          <id>stop-selenium</id>
                          <phase>post-integration-test</phase>
                          <goals>
                              <goal>stop-server</goal>
                          </goals>
                      </execution>  
                </executions>
            </plugin>

            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-surefire-plugin</artifactId>
                <configuration>
                    <!-- Skip the normal tests, we'll run them in the integration-test phase -->
                    <skip>true</skip>
                </configuration>

                <executions>
                    <execution>
                        <phase>integration-test</phase>
                        <goals>
                            <goal>test</goal>
                        </goals>
                        <configuration>
                            <skip>false</skip>
                        </configuration>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>
</project>
{% endcodeblock%}

Modify AppTest.java to make it a valid Selenium – TestNG test as shown below.

{% codeblock lang: AppTest.java%}
package com.pragmaticqa.tests;

import com.thoughtworks.selenium.DefaultSelenium;
import org.testng.annotations.AfterClass;
import org.testng.annotations.BeforeClass;
import org.testng.annotations.Test;

import static org.testng.Assert.assertTrue;
import static org.testng.Assert.assertEquals;


/**
 * Unit test for simple App.
 */
public class AppTest
{
    private DefaultSelenium selenium;

    @BeforeClass
    public void setUp()
    {
        selenium = new DefaultSelenium("localhost",4444,"*firefox","http://www.google.com/");
        selenium.start();
    }


    @Test
    public void testApp()
    {
        selenium.open("/");
        selenium.type("q","Selenium");
        selenium.click("btnG");
        assertEquals(selenium.getTitle(), "Google");
    }

    @AfterClass
    public void tearDown()
    {
        selenium.stop();
    }
}
{% endcodeblock%}

The setup is complete and you can test the setup by opening up a terminal and go the functionalTests directory. Once you are in the directory do the following.

  mvn integration-test

You will see maven running the tests and once the maven process is complete you will have an output like this.

    Tests run: 1, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 16.382 sec
    Results :
    Tests run: 1, Failures: 0, Errors: 0, Skipped: 0
    [INFO] ------------------------------------------------------------------------
    [INFO] BUILD SUCCESSFUL
    [INFO] ------------------------------------------------------------------------
    [INFO] Total time: 23 seconds
    [INFO] Finished at: Thu May 20 21:38:45 BST 2010
    [INFO] Final Memory: 25M/80M
    [INFO] ------------------------------------------------------------------------

I hope this helps people in getting started with Maven, Selenium and TestNG.

Happy testing!!!!