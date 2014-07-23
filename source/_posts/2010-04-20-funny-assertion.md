---
title: Funny Assertion!!!
author: Haroon Rasheed
layout: post
permalink: /blog/2010/04/funny-assertion/
categories:
  - assertions
---
  
Today I came across a piece of code while debugging some tests, that I thought of sharing with everyone. It was not easy to select a title for this post, but I am going to stick with “Funny Assertion”.

{% codeblock lang: java%}
int termsCount = termIDs.length;
assertEquals(termsCount, 4, "There should be three terms under Genres");
{% endcodeblock%}


	Result:
	java.lang.AssertionError: There should be three terms under Genres
	Expected :4
	Actual   :3


The tests expects the termsCount to be 4 in under Genres, but the comment in the assertion expects the term count to be 3. In a single line of code, you can see contradiction.

Although this looks funny, but it does present me the opportunity to highlight the problem of ignoring the comments in the assertion. Often we ignore the comments in the assertion while developing tests but it leads to misleading test results, and misleading test results then lead to time wasted on debugging non issues.

The comments in the assertion is a very powerful way to communicate the expected behaviour of the assertion or the test results to your fellow developer. The comments in the assertion can serve as documenting the test results.

If the assertion was written correctly then it was obvious that the test expects the termsCount to be 3 because the comment in the assertion is pointing to the fact that when the test was developed, the test developer expected that there will be 3 terms under a give condition. This would make it easy to debug the application.

As my personal preference, I see it really beneficial in having comments in the assertions. We should learn the lesson from this and give proper consideration to the comments in the assertion.