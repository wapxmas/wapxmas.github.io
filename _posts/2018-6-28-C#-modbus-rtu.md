---
layout: post
title: Modbus RTU And Timer Resolution With Respect To C#
---

Modbus protocol is widely used nowadays to operate industrial facilities or specific bundle of equipment. There are many approaches to implementation of the protocol among vendors but this article is devoted to one specific issue you may (or may not) encounter while working with Modbus over Serial Line (Modbus RTU).

When you send Modbus messages through a serial line (Modbus RTU), you have to make a delay between them for an operating device could recognize that there is a new message coming. This is opposite higher level implementations where you send one or more special bytes to distinguish between end of the one message and beginning of the other.

The following is the scheme of the aforementioned matter from the [specification]( http://www.modbus.org/docs/Modbus_over_serial_line_V1_02.pdf):

![Modbus RTU](/images/modbus-framing.png "Modbus RTU framing specification")

The scheme suggests that you have to make a delay between messages at least 3,5 characters. The one character is composed of 11 bits. The only one thing you should do before a pause is to measure its length in milliseconds. You can use the following formula: (11 * 3,5) / (baud_rate * 1000) = amount of milliseconds. Also, according to the specification, if the baud rate is equal or bigger than 19200 Bps, a delay between messages should always be 1.750ms.

Subject matter may (or may not) occur when an application tries to perform a delay less than 10ms (3 or 5ms for example) it transpires that, in spite of this, a delay may last 10ms. If this is your issue and you must not reduce a rate of messages, you should leverage the following Windows API: TimeBeginPeriod and TimeEndPeriod in the following manner:

{% highlight cs %}
WinApi.TimeBeginPeriod(1);
Thread.Sleep(2);
WinApi.TimeEndPeriod(1);
{% endhighlight %}

Also, you can check the timer resolution of your environment via [ClockRes](https://docs.microsoft.com/en-us/sysinternals/downloads/clockres).

