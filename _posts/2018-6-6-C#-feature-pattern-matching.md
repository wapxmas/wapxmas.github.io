---
layout: post
title: C# Pattern Matching
---

An increased interest in aspects of functional programming has emerged in recent years, and according to [new features of C#](https://blogs.msdn.microsoft.com/dotnet/2017/03/09/new-features-in-c-7-0/), it seems that the team developed C# 7.0 has been no exception all that period.

From C# 7.0 onwards it has got ability to accomplish so-called ‘Pattern Matching’. I think it would not be any use to go any deep into historical reasons and backgrounds of Pattern Matching development. In my opinion (it draws on a few years of developing via Haskell), Pattern Matching just facilitates the way of programming conditionals i.e. instead of writing branches of “if” statements, you write only one line that does the same but just under the hood of programming language. How about beginning with example in Haskell, just because Pattern Matching of this language has greatly matured.

{% highlight haskell %}
data Animal = Cat String | Dog String | Another String Integer

animalName :: Animal -> String
animalName (Cat name) = name
animalName (Dog name) = name
animalName (Another name _) = name

animalAge :: Animal -> Integer
animalAge (Another _ age) = age
animalAge _ = error "This type has no age"
{% endhighlight %}

Accordingly, we have – in the interest of simplified meaning – the type Animal, and two functions animalName and animalAge. You can see that type Animal contains things Cat, Dog, Another. They are no more than just constructors but unfortunately, unlike Haskell, C# does not allow you to assign different names to different constructors, i.e. they – constructors – have to share the same name as its class type name and could only be distinguished by its signature. Fortunately, due to sophisticated means of object-oriented programming language C#, you could conceive Animal as base class whereas Cat, Dog, and Another as derived classes. I will revert to this issue later on here in this article.
As you can see above we used Pattern Matching for returning - according to a constructor that could be Cat, Dog, or Another - name and age from type animal. Why did we use Pattern Matching? We did it because of the way how functional programs should be done. Unlike object-oriented programming, with functional programming you can't get the value of a type field via operator that could be similar to period “.” like you do it in C# for example. But, what does Pattern Matching do within object-oriented language C#? Well, in case of C#, Pattern Matching simply facilitates the way of the development.

Let's consider difference between Pattern Matching and Conditional branches by the following example:

{% highlight cs %}
static void Main(string[] args)
{
    Animal[] animals =
    {
                new Cat { Name = "Kitty", Age = 1 },
                new Dog { Name = "Doggo", Age = 2 },
                new Another { Name = "Undefined", Age = -1 }
            };

    ForeachWithPatternMatching(animals);

    ForeachWithConditionals(animals);
}

private static void ForeachWithConditionals(Animal[] animals)
{
    foreach (var animal in animals)
    {
        if (animal is Cat)
        {
            Cat cat = (Cat)animal;

            Console.WriteLine($"Cat: {cat.Name}, {cat.Age}");
        }
        else if (animal is Dog)
        {
            Dog dog = (Dog)animal;

            Console.WriteLine($"Dog: {dog.Name}, {dog.Age}");
        }
        else
        {
            Console.WriteLine($"I'm not discovered yet: {animal.Name}, {animal.Age}");
        }
    }
}

private static void ForeachWithPatternMatching(Animal[] animals)
{
    foreach (var animal in animals)
    {
        switch (animal)
        {
            case Cat cat:
                Console.WriteLine($"Cat: {cat.Name}, {cat.Age}");
                break;
            case Dog dog:
                Console.WriteLine($"Dog: {dog.Name}, {dog.Age}");
                break;
            default:
                Console.WriteLine($"I'm not discovered yet: {animal.Name}, {animal.Age}");
                break;
        }
    }
}
{% endhighlight %}

As you can see, Pattern Matching enables you to write code in less verbose, more concise, and brief manner. Since C# 7.0 was released, there are a few more features implemented in order to advance its Pattern Matching aspect. In the view of the previous example, Pattern Matching deserves to be widely adopted in every contemporary project and should be treated as the usual means of developing.
