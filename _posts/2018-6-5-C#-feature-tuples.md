---
layout: post
title: C# 7.0 Tuples
---

According to that [post](https://blogs.msdn.microsoft.com/dotnet/2017/03/09/new-features-in-c-7-0/) there are new features have been implemented in C# 7.0. I have sifted through them in order to highlight the most useful ones that deserve attention, so via this and subsequent articles, I would like to share my consideration on them.

### Tuples

I got the first experience of using tuples when I was learning Haskell. At first glance, I did not think that tuples are of any use but the more I used it, the more it grew convenient. In fact, I believe the most of internal data types in any language is mostly about convenience for developers, but there are a few data types, like tuples, that might require the passage of time to ascertain the convenience of them. This issue reminds me Kotlin and Java. Is not Kotlin built on JVM? Of course, it is, but Kotlin much more convenient than Java even at first sight. On the other hand, convenience does not make things obligatory because if a thing is convenient for one group, there is no guarantee that same thing will be convenient for another group.
Let’s look at the declaration of the method that uses tuples.

{% highlight cs linenos %}
(string, string, string) LookupName(long id)
{% endhighlight %}

What we see above is the method returns a tuple value. Actually, .Net Framework has already got `System.Tuple` class from 4.0 version onwards. The following is rewritten version of the method using `System.Tuple` class.

{% highlight cs linenos %}
System.Tuple<string, string, string> LookupName(long id)
{% endhighlight %}

Does the above return type look more verbose than the previous one or seem consuming any notable efforts of a developer to write it down than the first using tuples instead? I certainly will answer “no”. But, in that case, what kind of convenience do we get using tuples in C# 7.0? The convenience will become obvious when the new feature of C# 7.0 that is called Decomposition comes into play. Take a look at the following example.

{% highlight cs linenos %}
var (first, _, last) = LookupName(id1);
{% endhighlight %}

In the example above the variables first and last were assigned values through the decomposition, and the middle variable was ignored because the sign “_” denotes ignorance of an assignment. That is all there I consider important to say about tuples. For me, it is just about a more convenient way of development.
