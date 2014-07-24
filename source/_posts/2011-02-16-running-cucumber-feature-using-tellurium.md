---
title: Running cucumber feature using Tellurium
author: Haroon Rasheed
layout: post
permalink: /blog/2011/02/running-cucumber-feature-using-tellurium/
categories:
  - bdd
  - cucumber
  - dsl
  - groovy
  - maven
  - tellurium
---
Recently there was a question posted to the Tellurium user group about support for any of the BDD framework. We already have provided the support of running easyb stories as Tellurium tests. As I worked on the Cucumber and Selenium integration, I was intrigued to try using Cucumber with Tellurium. It turned out to be a pretty straight forward job. 

I created a new tellurium maven project using one of the maven archetypes available. Once we have the project skeleton, I added the dependencies for Cucumber cuke4duke plugin, that will be used to run the Tellurium test.

<!-- more -->

Following is my pom.xml

{% codeblock lang: pom.xml%}

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.pragmaticqa</groupId>
    <artifactId>bdd</artifactId>
    <version>1.0-SNAPSHOT</version>
    <name>Tellurium BDD Project</name>

    <repositories>
        <repository>
            <id>caja</id>
            <url>http://google-caja.googlecode.com/svn/maven</url>
        </repository>
        <repository>
            <id>kungfuters-public-snapshots-repo</id>
            <name>Kungfuters.org Public Snapshot Repository</name>
            <releases>
                <enabled>false</enabled>
            </releases>
            <snapshots>
                <enabled>true</enabled>
            </snapshots>
            <url>http://kungfuters.org/nexus/content/repositories/snapshots</url>
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
            <url>http://kungfuters.org/nexus/content/repositories/releases</url>
        </repository>
        <repository>
            <id>kungfuters-thirdparty-releases-repo</id>
            <releases>
                <enabled>true</enabled>
            </releases>
            <snapshots>
                <enabled>false</enabled>
            </snapshots>
            <url>http://kungfuters.org/nexus/content/repositories/thirdparty</url>
        </repository>
        <repository>
            <id>openqa-release-repo</id>
            <releases>
                <enabled>true</enabled>
            </releases>
            <snapshots>
                <enabled>false</enabled>
            </snapshots>
            <url>http://archiva.openqa.org/repository/releases</url>
        </repository>
        <repository>
        <id>cukes</id>
        <url>http://cukes.info/maven</url>
        </repository>
    </repositories>

    <pluginRepositories>
        <pluginRepository>
            <id>cukes</id>
            <url>http://cukes.info/maven</url>
        </pluginRepository>
    </pluginRepositories>

    <dependencies>
        <dependency>
            <groupId>org.hamcrest</groupId>
            <artifactId>hamcrest-all</artifactId>
            <version>1.1</version>
        </dependency>
        <dependency>
            <groupId>org.codehaus.groovy.maven</groupId>
            <artifactId>gmaven-mojo</artifactId>
            <version>1.0-rc-5</version>
            <exclusions>
                <exclusion>
                    <groupId>org.codehaus.groovy.maven.runtime</groupId>
                    <artifactId>gmaven-runtime-1.5</artifactId>
                </exclusion>
            </exclusions>                      
        </dependency>
        <dependency>
            <groupId>org.picocontainer</groupId>
            <artifactId>picocontainer</artifactId>
            <version>2.10.2</version>
            <scope>test</scope>
        </dependency>

        <dependency>
            <groupId>org.codehaus.groovy.maven.runtime</groupId>
            <artifactId>gmaven-runtime-1.6</artifactId>
            <version>1.0-rc-5</version>
        </dependency>

        <dependency>
            <groupId>tellurium</groupId>
            <artifactId>tellurium-core</artifactId>
            <version>0.6.0</version>
            <scope>compile</scope>
        </dependency>

        <dependency>
            <groupId>org.seleniumhq.selenium.server</groupId>
            <artifactId>selenium-server</artifactId>
            <version>1.0.1-te</version>
            <!--classifier>standalone</classifier-->
        </dependency>

        <dependency>
            <groupId>org.seleniumhq.selenium.client-drivers</groupId>
            <artifactId>selenium-java-client-driver</artifactId>
            <version>1.0.1</version>
            <exclusions>
                <exclusion>
                    <groupId>org.codehaus.groovy.maven.runtime</groupId>
                    <artifactId>gmaven-runtime-default</artifactId>
                </exclusion>
                <exclusion>
                    <groupId>org.seleniumhq.selenium.core</groupId>
                    <artifactId>selenium-core</artifactId>
                </exclusion>
                <exclusion>
                    <groupId>org.seleniumhq.selenium.server</groupId>
                    <artifactId>selenium-server</artifactId>
                </exclusion>
            </exclusions>
        </dependency>

        <dependency>
          <groupId>org.openqa.selenium.grid</groupId>
          <artifactId>selenium-grid-tools</artifactId>
          <version>1.0.2</version>
        </dependency>


        <dependency>
            <groupId>org.codehaus.groovy</groupId>
            <artifactId>groovy-all</artifactId>
            <version>1.7.0</version>
        </dependency>
        <dependency>
            <groupId>caja</groupId>
            <artifactId>json_simple</artifactId>
            <version>r1</version>
        </dependency>
        <dependency>
            <groupId>org.stringtree</groupId>
            <artifactId>stringtree-json</artifactId>
            <version>2.0.10</version>
        </dependency>
    </dependencies>

    <build>
        <resources>
            <resource>
                <directory>src/main/groovy</directory>
            </resource>
            <resource>
                <directory>src/main/resources</directory>
            </resource>
        </resources>
        <testResources>
            <testResource>
                <directory>src/test/groovy</directory>
            </testResource>
            <testResource>
                <directory>src/test/resources</directory>
            </testResource>
        </testResources>

        <pluginManagement>
            <plugins>
                <plugin>
                    <artifactId>maven-compiler-plugin</artifactId>
                    <version>2.0.2</version>
                </plugin>
                <plugin>
                    <artifactId>maven-surefire-plugin</artifactId>
                    <version>2.4.3</version>
                </plugin>
                <plugin>
                    <artifactId>maven-surefire-report-plugin</artifactId>
                    <version>2.4.3</version>
                </plugin>
                <plugin>
                    <artifactId>maven-jar-plugin</artifactId>
                    <version>2.2</version>
                </plugin>
                <plugin>
                    <artifactId>maven-source-plugin</artifactId>
                    <version>2.0.4</version>
                </plugin>
                <plugin>
                    <artifactId>maven-javadoc-plugin</artifactId>
                    <version>2.4</version>
                </plugin>
                <plugin>
                    <artifactId>maven-site-plugin</artifactId>
                    <version>2.0-beta-7</version>
                </plugin>
                <plugin>
                    <groupId>org.codehaus.groovy.maven</groupId>
                    <artifactId>gmaven-plugin</artifactId>
                    <version>1.0-rc-5</version>
                </plugin>
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-jxr-plugin</artifactId>
                    <version>2.1</version>
                </plugin>
            </plugins>
        </pluginManagement>
        <plugins>
            <plugin>
                <artifactId>maven-compiler-plugin</artifactId>
                <configuration>
                    <source>1.5</source>
                    <target>1.5</target>
                </configuration>
            </plugin>
            <plugin>
                <artifactId>maven-surefire-plugin</artifactId>
                <configuration>
                    <includes>
                        <include>**/*_UT.java</include>
                       <include>**/*TestCase.java</include>
                    </includes>
                </configuration>
            </plugin>
            <plugin>
                <artifactId>maven-jar-plugin</artifactId>
                <executions>
                    <execution>
                        <goals>
                            <goal>test-jar</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
            <plugin>
                <artifactId>maven-source-plugin</artifactId>
                <executions>
                    <execution>
                        <phase>package</phase>
                        <goals>
                            <goal>jar</goal>
                            <goal>test-jar</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
            <plugin>
                <artifactId>maven-javadoc-plugin</artifactId>
                <executions>
                    <execution>
                        <goals>
                            <goal>jar</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
            <plugin>
                <groupId>org.codehaus.groovy.maven
                </groupId>
                <artifactId>gmaven-plugin</artifactId>
                <executions>
                    <execution>
                        <goals>
                            <!-- The generateStubs goals are not yet working for enums and generics -->
                            <!-- <goal>generateStubs</goal>-->
                            <goal>compile</goal>
                            <!--<goal>generateTestStubs</goal>-->
                            <goal>testCompile</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>

            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-antrun-plugin</artifactId>
                <version>1.3</version>
                <dependencies>
                    <dependency>
                        <groupId>org.apache.ant</groupId>
                        <artifactId>ant</artifactId>
                        <version>1.7.1</version>
                    </dependency>
                    <dependency>
                        <groupId>org.apache.ant</groupId>
                        <artifactId>ant-launcher</artifactId>
                        <version>1.7.1</version>
                    </dependency>
                    <dependency>
                        <groupId>org.apache.ant</groupId>
                        <artifactId>ant-nodeps</artifactId>
                        <version>1.7.1</version>
                    </dependency>
                </dependencies>
                <executions>
                   <execution>
                        <id>compile-java</id>
                        <phase>generate-sources</phase>
                        <configuration>
                            <tasks>
                                <path id="compile.classpath">
                                    <path refid="maven.compile.classpath"/>
                                    <pathelement location="target/classes"/>
                                </path>
                                <mkdir dir="target/classes"/>
                                <javac srcdir="src/main/groovy" debug="${javac-debug}"
                                       destdir="target/classes" classpathref="compile.classpath">
                                    <include name="**/*.java"/>
                                </javac>
                            </tasks>
                        </configuration>
                        <goals>
                            <goal>run</goal>
                        </goals>
                    </execution>                    
                    <execution>
                        <id>compile-java-test</id>
                        <phase>test-compile</phase>
                        <configuration>
                            <tasks>
                                <path id="testcompile.classpath">
                                    <path refid="maven.compile.classpath"/>
                                    <path refid="maven.test.classpath"/>
                                    <pathelement location="target/classes"/>
                                    <pathelement location="target/test-classes"/>
                                </path>
                                <javac srcdir="src/test/groovy" debug="${javac-debug}"
                                       destdir="target/test-classes" classpathref="testcompile.classpath">
                                    <include name="**/*.java"/>
                                </javac>
                            </tasks>
                        </configuration>
                        <goals>
                            <goal>run</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
            <plugin>
                <groupId>cuke4duke</groupId>
                <artifactId>cuke4duke-maven-plugin</artifactId>
                <configuration>
                    <jvmArgs>
                        <jvmArg>
                            -Dcuke4duke.objectFactory=cuke4duke.internal.jvmclass.PicoFactory
                        </jvmArg>
                        <jvmArg>-Dfile.encoding=UTF-8</jvmArg>
                    </jvmArgs>
                    <cucumberArgs>
                        <cucumberArg>--backtrace</cucumberArg>
                        <cucumberArg>--color</cucumberArg>
                        <cucumberArg>--verbose</cucumberArg>
                        <cucumberArg>--format</cucumberArg>
                        <cucumberArg>pretty</cucumberArg>
                        <cucumberArg>--format</cucumberArg>
                        <cucumberArg>html</cucumberArg>
                        <cucumberArg>--out</cucumberArg>
                        <cucumberArg>target/cucumber-reports.html</cucumberArg>
                        <cucumberArg>--require</cucumberArg>
                        <cucumberArg>${basedir}/target/test-classes</cucumberArg>
                    </cucumberArgs>
                    <gems>
                        <gem>install cuke4duke --version 0.3.2</gem>
                    </gems>
                </configuration>
                <executions>
                    <execution>
                        <id>run-features</id>
                        <phase>integration-test</phase>
                        <goals>
                            <goal>cucumber</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
            <plugin>
        <groupId>org.codehaus.groovy.maven</groupId>
        <artifactId>gmaven-plugin</artifactId>
        <executions>
          <execution>
            <goals>
              <goal>testCompile</goal>
            </goals>
          </execution>
        </executions>
      </plugin>
        </plugins>
    </build>
    <reporting>
        <plugins>
            <plugin>
                <artifactId>maven-surefire-report-plugin</artifactId>
                <reportSets>
                    <reportSet>
                        <reports>
                            <report>report-only</report>
                        </reports>
                    </reportSet>
                </reportSets>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-jxr-plugin</artifactId>
            </plugin>
        </plugins>
    </reporting>
    <properties>
        <java-version>1.5</java-version>
        <groovy-version>1.6.0</groovy-version>
        <gmaven-version>1.0-rc-5</gmaven-version>
        <selenium-version>1.0.1</selenium-version>
        <javac-debug>true</javac-debug>
    </properties>

</project>
{% endcodeblock%}

The project structure is as follows.

{% img /images/project_structure.png %}

The contents of the feature file “GoogleSearch.feature” are:


    Feature: Google Search Feature
                 In order to ensure that Google Search works
                 I want to run a quick Hello World search.
            Scenario: Hello World Scenario
                  Given The search is Hello World
                  When The Search is performed
                  Then The browser title should have Hello World


Now we need to provide the implementation of the Steps file, that will drive Tellurium to test the application. For this I am using the GroovyDSL pattern provided by the cukes4duke plugin. I have created the file “GoogleSearchSteps.groovy” as shown below.

{% codeblock lang: groovy%}

package test

import module.GoogleSearchModule
import org.tellurium.test.groovy.TelluriumGroovyTestCase


this.metaClass.mixin(cuke4duke.GroovyDsl)

GoogleSearchModule gsm;

Before(){
    TelluriumGroovyTestCase.newInstance().with {
        setUpForClass()
        connectUrl "http://www.google.com"
    }
}

Given (~/The search is Hello World/){
    gsm = new GoogleSearchModule()
    gsm.defineUi()
}

When (~/The Search is performed/){
    gsm.doGoogleSearch "Hello World"
}

Then (~/The browser title should have Hello World/){
    org.hamcrest.MatcherAssert.assertThat("Browser title: ", gsm.getBrowserTitle(),
            org.hamcrest.Matchers.containsString("hello world"));
}

After(){
    TelluriumGroovyTestCase.newInstance().with {
        tearDownForClass()
    }
}
{% endcodeblock%}

Once you are done with the set up above, now run the integration-test goal.

    mvn clean integration-test

This would run the feature file and drive Tellurium. Once the build is finished, you can see the test results under the /target/cucumber-reports.html. 

{% img /images/test_results.png %}

Green is good!!!!!!!