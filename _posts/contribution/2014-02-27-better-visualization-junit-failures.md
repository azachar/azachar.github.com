---
layout: post
category : contribution
title: Better Visualization of jUnit Failures
tagline: "open proposal"
tags : [Eclipse, JDT, jUnit, proposal]
---
{% include JB/setup %}
---

### Dear Providers of Testing Tools!

I am writing you in order to discuss and find a solution for a better visual experience of failed tests across the java testing community. Mainly I think that test driven development based on examining stack traces should be part of the history!

I wanted to provide a contribution for a better visual experience in Eclipse for failed tests and provide also a platform so other test libraries can contribute and visualize their failed tests in a better way too. Simply I want to achieve at least this:

[![Sample][sample]][sampleLink]

The problem of showing better tests results goes far behind the scope of one particular testing library and Eclipse. To visual any failed tests is a bit awkward now, because everything is based on a stack trace  and in some special cases on the exception ``org.junit.ComparisonFailure`` that provides some more data to visualize. That is why I have realized to contact you in order to find a solution that would be acceptable across the community.

### What’s the problem? 
Well, the problem is how to pass information about the failed test to an IDE or to a CI or any other tool. Currently the only data that are passed from the test framework to an IDE are exceptions, very typical the ``AssertionError`` or the ``org.junit.ComparisonFailure``.
So for example if there is a complex failure and you want to pass it to your IDE, then on the IDE side must be something that understands the passed exception and extracts the data from it. 

Well that sounds nice, but the problem that AssertionException’s message is not the right place to pass complex parameters…

I was thinking about the following model.

[![Model][model]][model]


Where ``AssertionExceptionModel`` holds important information that could be use to render any failed test in more self-explanatory way. So each AssertionException will contain this ``AssertionExceptionModel`` that would be easy to serialize and pass.

{% highlight java %}
class org.testing.DescriptiveAssertionException extends java.lang.AssertionException {
  private AssertionExceptionModel model;
  ..
}
{% endhighlight %}

```
AssertionExceptionModel

+ String exceptionId  [mandatory]
+ String data [mandatory]
+ String dataContentType [mandatory]
```

Where 

 - ``exceptionId`` specifies the unique identifier of failure problem across all testing libraries. For example based on this ‘ID’ your IDE can provide better visualization of your failed test.
 
 - ``dataContentType`` is either ’plain/text’ or ‘application/JSON’ and determine the content in the field ‘data’. 

Also I was thinking that each testing framework can provide an optional “database” / factory for your IDE or your TOOL that would provide the template how to render the given exceptionId;

{% highlight java %}
VisualTemplate
+ String templateForExceptionId  (mandatory)
+ String templateContentType  (mandatory)
+ String suggestedVisualTemplate  (mandatory)
{% endhighlight %}

Where
 
 -  ``suggestedVisualTemplate`` specifies a visual html template that could be used by an IDE or a tool for representing such an exception.

 -  ``templateContentType`` specifies a content type of visual template (for example mustache or velocity template or  http://jknack.github.io/handlebars.java or any other light framework ) in order to render data in a nice way in your IDE or your  TOOL.


### Why we cannot pass the exception with all data directly to the IDE? 
There is a huge issue with it:

Your tests are executed in a different virtual machines (VM) than your IDE or tool in order to provide isolation of test failures. So if your IDE wants to know what is happing, it receives messages from the testing VM about failures.
In order to setup a communication between 2 VM you need to use some kind of serialization / deserialization and of course there is an issue with dependencies of such a deserialized objects… 

So the easiest way how to pass any date between 2 VM is by using the plain text or JSON format. This is way  any TOOL  or IDE can understand exception with JSON data better and provide more descriptive visualization that could be extend by a 3rd party library.

Here is how it could look:

[![Passing Failures Data][passing]][passing]

### Preview Implementation
I created a sample implementation that demonstrate a visual power of failed message. Look at the github project

[Eclipse JDT Fork Aims to Provide Better Visual Experience of Failed Tests][sampleLink]

The implementation is based on advance handling of ``org.junit.ComparisonFailure``, so it can be used even today.

For some complex failures such as  ```assertThat(objectA).isEqualToComparingFieldByField(objectB);``` that do not support ``expected`` and ``actual`` values, is visualization more problematic. As an sample implementation I created a fork from AssertJ that provides a simple text based comparison using ``expected`` and ``actual`` values. This is just a temporary demo implementation. When this proposal will take some actions, it will be updated to a decent solution, hopefully it would be soon :)


  
### Questions
So open questions are:

1. What do you think about the proposal?
2. If you like it, who will be providing classes for ```DescriptiveAssertionException``` with ``AssertionExceptionModel``? It sounds to me that JAVA itself should contain such a dependency… But until this become part of Java should we create a maven artefact org.testing:api:1.0.0 that would provide these classes so any other tool or testing library can use these simple classes. For backward compatibility can we put it in the same package as ``java.lang.AssertionException``, e.g. into java.lang?
 
 
I look forward to hearing from you!

Kind regards,

Andrej Zachar

[sample]: https://f.cloud.github.com/assets/4659019/2266371/f737afde-9e96-11e3-9a73-202dac7e6520.png (Screenshot of Forked Eclipse JDT)
[sampleLink]: http://azachar.github.io/org.eclipse.jdt.ui/
[model]: http://yuml.me/fc9a8cb8 (Model)
[passing]: /assets/passing-data.jpeg (Passing Failures Model between JVM)
