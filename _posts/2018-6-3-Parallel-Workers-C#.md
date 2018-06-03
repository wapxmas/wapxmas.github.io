---
layout: post
title: Parallel Workers C#
---

Before that day I had used to approach parallel tasks via regular .Net Framework classes, but eventually in one of my projects I stumbled across the need of a class that could provide me with means through which I could be able to address several parallel tasks.

### First task

Here I was given the list of proxies, and the prospective application had to get one proxy from the list, do some web work through it, and return back to that list. The essential condition on the application was working in a multithreaded way. That meant if the application was provided with a list of 3 proxies, it should be able to handle it in the aforementioned way even though the application was configured to work on such a number of threads that could be bigger than the list of proxies.
So, later on I came up with the [ParallelCircleQueue&lt;T&gt;](https://github.com/wapxmas/RikardLib.Concurrent/blob/master/ParallelCircleQueue.cs) class that can address that type of task. The following this is example of usage of this class.

{% highlight cs linenos %}
class Program
{
    static void Main(string[] args)
    {
        var pcq = new ParallelCircleQueue<string>(new string[] { "s1", "s2", "s3" }.ToList());

        var tasks = new[]
        {
            Task.Factory.StartNew(() => ExampleTask(pcq)),
            Task.Factory.StartNew(() => ExampleTask(pcq)),
            Task.Factory.StartNew(() => ExampleTask(pcq))
        };

        Task.WaitAll(tasks);
    }

    static void ExampleTask<T>(ParallelCircleQueue<T> pcq)
    {
        foreach(var _ in Enumerable.Range(1, 100))
        {
            using (var p = pcq.GetItem())
            {
                Console.WriteLine(p.Item);
            }
        }
    }
}
{% endhighlight %}

### Second task

I bet you can remember that when you are working with resources they mostly deny work with them in a multithreaded way. It is appropriate for resources to work with them in single thread. Then, what if you have to deal with resource in multithreaded way. Consequently, the following class - [ParallelGatherSingle&lt;T&gt;](https://github.com/wapxmas/RikardLib.Concurrent/blob/master/ParallelGatherSingle.cs)  - was written to cope with this issue. Here is example of the usage.

{% highlight cs linenos %}
class Program
{
    static void Main(string[] args)
    {
        var pgs = new ParallelGatherSingle<string>(ExampleGatherItems);

        var tasks = new[]
        {
                Task.Factory.StartNew(() => ExampleTask(pgs)),
                Task.Factory.StartNew(() => ExampleTask(pgs)),
                Task.Factory.StartNew(() => ExampleTask(pgs))
            };

        Task.WaitAll(tasks);

        Console.WriteLine("ExampleGatherItems proceedes in the background. Press enter to wait gathering is done.");
        Console.ReadLine();

        pgs.WaitGatherDone();
    }

    private static void ExampleGatherItems(string item)
    {
        Console.WriteLine(item);
        Thread.Sleep(500);
    }

    static void ExampleTask(ParallelGatherSingle<string> pgs)
    {
        var rnd = new Random();

        foreach (var n in Enumerable.Range(1, 10))
        {
            pgs.AddData($"{Thread.CurrentThread.ManagedThreadId} : {n}");
            Thread.Sleep(rnd.Next(100, 300));
        }
    }
}
{% endhighlight %}

### Third task

Another task I was given was to make application that could process literally millions of tasks in a multithreaded way so that application could consume that vast amount of work in a piece-by-piece way i.e. if there were million tasks, the application should be able to consume let’s say 10’000 tasks, and then by raising event of finishing could get another 10’000 tasks to process. That task also was beyond the boundaries of means provided by .Net Framework. Thus, [ParallelWorker](https://github.com/wapxmas/RikardLib.Concurrent/blob/master/ParallelWorker.cs) was born, and the following is the example of the usage.

{% highlight cs linenos %}
class Program
{
    static void Main(string[] args)
    {
        var pw = new ParallelWorker(threadsNum: 5);

        var tasks = new[]
        {
                Task.Factory.StartNew(() => ExampleTask(pw)),
                Task.Factory.StartNew(() => ExampleTask(pw)),
                Task.Factory.StartNew(() => ExampleTask(pw))
            };

        Task.WaitAll(tasks);

        Console.WriteLine("ParallelWorker proceedes in the background. Press enter to wait all worker threads are done.");
        Console.ReadLine();

        pw.WaitWorkDone();
    }

    static void ExampleTask(ParallelWorker pw)
    {
        var rnd = new Random();

        foreach (var n in Enumerable.Range(1, 100))
        {
            pw.AddWork(() => { Console.WriteLine($"{Thread.CurrentThread.ManagedThreadId} : {n}"); Thread.Sleep(1000); });
        }

        Thread.Sleep(3000);
    }
}
{% endhighlight %}
