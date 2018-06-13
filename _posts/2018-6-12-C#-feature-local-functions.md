---
layout: post
title: Local Functions In C# 7.0
---

The value of an implementation of functional programming features in object-oriented programming languages has significantly grown last years; consequently, local functions have got a deserving emphasis and were implemented in C# 7.0 among other features. Local functions as such are widely being used in Haskell, so that it hard to conceive of this programming language with the absence of them.

You can consider a usefulness of local functions by the following example:

{% highlight cs %}
static void Main(string[] args)
{
    Cat[] cats =
    {
                new Cat { Name = "Kitty", Age = 1 },
                new Cat { Name = "Kitty2", Age = 2 },
                new Cat { Name = "Kitty3", Age = 3 },
            };

    Dog[] dogs =
    {
                new Dog { Name = "Doggo", Age = 1 },
                new Dog { Name = "Doggo2", Age = 2 },
                new Dog { Name = "Doggo3", Age = 3 },
            };

    ForeachWithLocalFunctions(cats, dogs);
}

private static void ForeachWithLocalFunctions(Cat[] cats, Dog[] dogs)
{
    foreach (var cat in cats)
    {
        printAnimal(cat);
    }

    foreach (var dog in dogs)
    {
        printAnimal(dog);
    }

    void printAnimal(Animal animal)
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

As you can see from the example above, we used functions printDog and printCat as local functions. Therefore, since they are local functions we don’t have to worry about their privacy i.e. access modifiers, and the class looks more concise without unnecessary methods that would have been included into its body if C# 7.0 hadn’t had the local functions feature.
Of course, it is obvious that you shouldn’t make function local if its logic has to be shared among other methods.

The fundamental purpose of the local functions is to move logic that belongs to only one method inside that method and also to enable a developer to remove a code redundancy (if any) inside the method without affecting the body of the class.
In my opinion, there is one thing to spare that is worth to say about. According to the example of local function usage from [the MSDN Blog post](https://blogs.msdn.microsoft.com/dotnet/2017/03/09/new-features-in-c-7-0/), it seems that we can use local functions in a recursive manner i.e. without affecting a function call stack. That behavior of function calling has name “tail recursion”.

However, there is neither an official article nor a creditable evidence that C# is always following such behavior, and so it was [proven](https://stackoverflow.com/questions/37768876/is-this-tail-recursive-and-could-it-cause-a-stack-overflow-c-sharp-net) that in one case MSIL was generated properly but in the other, it wasn’t. Therefore, even though [that op code exists](https://msdn.microsoft.com/en-us/library/system.reflection.emit.opcodes.tailcall(v=vs.110).aspx), it is only guaranteed that it will be applied while developing under F#. Accordingly, just don’t rely on such examples as in the MSDN article until that behavior is officially guaranteed for C#.

