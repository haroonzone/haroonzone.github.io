---
title: Tellurium TestNG Archetype; An easy approach to get started with Tellurium
author: Haroon Rasheed
layout: post
permalink: /tellurium-testng-archetype-an-easy-approach-to-get-started-with-tellurium
comments: true
categories:
  - dsl
  - groovy
  - maven
  - tellurium
  - testng
tags:
  - maven archetype
---
  
I will be writing more about [Tellurium][1] in the near future. I have not been spending that much time on [Tellurium][1] these days as I would have liked to, and the team has done an incredible job in releasing [Tellurium 0.7.0][1] with many exciting new features.

It was easy to pick this topic as a starting point to familiarize myself with the new features in [Tellurium][1] as I go along exploring.

We will be creating a new [Tellurium][1] project by making use of the Tellurium TestNG Archetype. This is an easy and quick way to get yourself started with [Tellurium][1].

Tellurium includes two maven archetypes, i.e., tellurium-junit-archetype and tellurium-testng-archetype for Tellurium JUnit test project and Tellurium TestNG test project, respectively.

<!-- more -->

The first step is to modify or create your maven setting.xml file to allow you to automatically include [Tellurium][1] artifacts in your Maven project.
This should go in your_home/.m2/settings.xml file:

{% codeblock lang: xml%}
<settings>
    <profiles>
        <profile>
            <activation>
                <activeByDefault>true</activeByDefault>
            </activation>
            <repositories>
                 <repository>
                    <id>kungfuters-public-snapshots-repo</id>
                    <name>Kungfuters.org Public Snapshot Repository</name>
                    <releases>
                        <enabled>false</enabled>
                    </releases>
                    <snapshots>
                        <enabled>true</enabled> 
                    </snapshots>
                    <url>http://maven.kungfuters.org/content/repositories/snapshots</url>
                </repository>
                <repository>
                    <id>kungfuters-public-releases-repo</id>
                    <name>Kungfuters.org Public Releases Repository</name>
                    <releases>
                        <enabled>true</enabled> 
                    </releases>
                    <snapshots>
                        <enabled>false</enabled>
                    </snapshots>
                    <url>http://maven.kungfuters.org/content/repositories/releases</url>
                </repository> 
            </repositories>
        </profile>
    </profiles>
</settings>
{% endcodeblock%}

Then, run the following maven command to create your new project

    mvn archetype:create -DgroupId=your_group_id \
                         -DartifactId=your_artifact_id \
                         -DarchetypeArtifactId=tellurium-testng-archetype \
                         -DarchetypeGroupId=tellurium \
                         -DarchetypeVersion=0.6.0

Without adding the [Tellurium][1] Maven repository, you can specify it in the command line as

    mvn archetype:create -DgroupId=your_group_id \
                         -DartifactId=your_artifact_id \
                         -DarchetypeArtifactId=tellurium-testng-archetype \
                         -DarchetypeGroupId=tellurium \
                         -DarchetypeVersion=0.6.0 \
                         -DarchetypeRepository=http://maven.kungfuters.org/content/repositories/releases

But you still need to put the [Tellurium][1] repository into your settings.xml later since the Tellurium test project needs to download other [Tellurium][1] artifacts from the repository, for example, the [tellurium][1] core jar file.

After the project is created, you will find the following files

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

You should use your IDE to open the project, for example, in IntelliJ IDEA, do the following steps


**New Project > Import project from external model > Maven > Project directory > Finish**


You will open up a Maven project and make sure you are using Groovy 1.6.0 in your project. After that, compile the project and run the example test GoogleSearchTestCase.

If you want to run the tests in command line, you can use the following command

    mvn clean test


[1]: http://code.google.com/p/aost