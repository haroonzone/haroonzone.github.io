---
title: List directory structure in Unix/Linux
author: Haroon Rasheed
layout: post
permalink: /list-directory-structure-in-unixlinux
comments: true
categories:
  - directory structure
  - linux
---
  
Today I found a really useful combination of commands to list the directory structure in Unix/Linux operating systems as follows.

{% codeblock lang: bash%}
ls -R | grep “:$” | sed -e ‘s/:$//’ -e ‘s/\[^-\]\[^\/\]*\//–/g’ -e ‘s/^/ /’ -e ‘s/-/|/’
{% endcodeblock%}

The result of this command is as below. It is listing the directory structure under the functionalTests directory.

{% codeblock lang: bash%}
functionalTests haroon$ ls -R | grep ":$" | sed -e 's/:$//' -e 's/[^-][^\/]*\//--/g' -e 's/^/   /' -e 's/-/|/
   |-src
   |---main
   |-----java
   |-------com
   |---------pragmaticqa
   |-----------tests
   |---test
   |-----java
   |-------com
   |---------pragmaticqa
   |-----------tests
   |-target
   |---classes
   |-----com
   |-------pragmaticqa
   |---------tests
   |---surefire-reports
   |-----Command line suite
   |---test-classes
   |-----com
   |-------pragmaticqa
   |---------tests
{% endcodeblock%}

You can also use Unix tree command to list the directory structure, as of writing this one I was not aware of the tree command.