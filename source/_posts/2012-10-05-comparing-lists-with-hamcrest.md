---
title: Comparing lists with hamcrest
author: Haroon Rasheed
layout: post
permalink: /comparing-lists-with-hamcrest
comments: true
categories:
  - hamcrest
  - java
  - lists
tags:
  - assertions
  - hamcrest
  - java
  - lists
---
We have been using hamcrest matchers for some time now, and it is awesome to have more readable assertions in your tests. Recently we had a requirement to match lists for equality. This was simple enough, we used the following matcher to compare that the contents of the list are the same.

{% codeblock lang: ListMatcher1.java%}

@Test
public void listTestsWithOrder(){
  List<String> list1 = new ArrayList<String>();
  List<String> list2 = new ArrayList<String>();
  
  list1.add("red");
  list1.add("green");
  list1.add("orange");
  
  list2.add("red");
  list2.add("green");
  list2.add("orange");
  assertThat("List equality without order", list1, equalTo(list2));
 
}
{% endcodeblock%}

<!-- more -->

It worked nicely but then like anything in the project lifecycle, our requirement changed and we had to assert the contents of the two lists where the order does not matter, at this time we were using version 1.1 of Hamcrest. After a bit of digging I found that the new version (1.3) of Hamcrest has some new matchers specifically for collections, and changing our tests to following worked like a charm.

{% codeblock lang: ListMatcher2.java%}

@Test
public void listTestsWithoutOrder(){
  List<String> list1 = new ArrayList<String>();
  List<String> list2 = new ArrayList<String>();
  
  list1.add("red");
  list1.add("green");
  list1.add("orange");
  
  list2.add("green");
  list2.add("orange");
  list2.add("red");
  
  assertThat("List equality without order", list1, containsInAnyOrder(list2.toArray()));
}
{% endcodeblock%}

Notice the method *containsInAnyOrder*, this would compare the items in the list irrespective of the order. and if the order is important then it counterpart is *contains*. This would compare the items and would check for order as well.

Also like any other matcher of Hamcrest, if your test fails then you get a very descriptive error message as follows.


	java.lang.AssertionError: List equality without order
	Expected: iterable over ["green", "orange", "red", "blue"] in any order
	but: No item matches: “blue” in ["red", "green", "orange"]
	at org.hamcrest.MatcherAssert.assertThat(MatcherAssert.java:20)
	at com.ft.specs.ListTests.listTestsWithoutOrder(ListTests.java:32)