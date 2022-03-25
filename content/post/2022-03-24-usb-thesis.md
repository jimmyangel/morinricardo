---
title: Personal archaeology - A boiler controller using a microprocessor (1979-1980)
date: 2022-03-24
tags: ["rtos", "rmx-80", "microprocessor", "archaeology"]
---
And now for something completely different and out of the blue.

I couple of months ago, I started playing [Snap Circuits](https://en.wikipedia.org/wiki/Snap_Circuits) with my 6-year old grandson. These things are truly a terrific way for children to learn and get their hands into some electronic circuitry. Then I decided to start augmenting their capabilities by getting an [Arduino](https://en.wikipedia.org/wiki/Arduino) board and some sensors, so that we could further enhance our projects -- my plan is to gently introduce him to the concepts as we exhaust what is possible to do with just the Snap Circuits kits alone.

As I started playing with the Arduino board, I went into the rabbit hole of using a Real Time Operating System  [(RTOS)](https://www.freertos.org/) to program the microcontroller. And then poof, it was like traveling back in time to 1979 when I was working on my electronic engineering thesis project...

<!--more-->

<p align="center">
  <img src="/images/uploads/time-travel.gif">
</p>

Since I wanted to improve my time travel experience, I decided to dig out my old copy of the project report and, oh no!, the pages are actually fading! Well, I thought, if I leave it just like that, after a couple of years there would be nothing but blank pages left. No more time travel!

So I went ahead and scanned the old document and decided to post it on my here on my blog.

Here is the scanned and OCR'd PDF:
<p align="center">
  <a href="/images/uploads/tesis-ricardo-1980.pdf"><img src="/images/uploads/tesis-cover.jpg"></a>
</p>

Controlling an industrial boiler is not necessarily all that interesting. What is really remarkable about the project is that it used an RTOS in 1979! That was a pretty revolutionary concept way back then. And to be honest, it took us months to just get the OS up and running since there was no one in the entire university who had ever used one of these before.

On top of that, the electronic engineering program did not include any kind of advanced computer science concepts such as operating systems, concurrency, multi-tasking and such. Not even high level programming languages were used back then for this kind of thing... everything was done in assembly! So to figure this thing out was pretty painful to say the least.

I remember at some point I went to our advisor, Prof. Giulio Bevilacqua, and told him that we could not do this project with the RTOS... His answer was: "well then, you will not get a degree!" So, we had to figure it out. And we did!

Here are some project highlights:

* RMX/80 RTOS - Real-time executive operating system from Intel (yes, Intel!)
* Intel PL/M-80 - A high level language inspired on IBM's PL/1
* Intel iSBC 80/10 - Intel Single Board Computer based on the 8080 microprocessor
* Tektronix 4006/1 - Storage tube technology graphics terminal (the school had one of these laying around so we grabbed it and did some graphics with it!)

And here are some old pictures of the finished product (believe it or not, it **actually** worked!!):

<p align="center">
  <img src="/images/uploads/tesis-left-view-boiler-and-controller.jpg">
  <br>Boiler, controller, and graphics terminal
</p>

<p align="center">
  <img src="/images/uploads/tesis-front-panel-view.jpg">
  <br>Front panel
</p>

<p align="center">
  <img src="/images/uploads/tesis-left-connect-panel-view.jpg">
  <br>Left side, signal and power connections
</p>

<p align="center">
  <img src="/images/uploads/tesis-top-view-power-board.jpg">
  <br>Top view, power control board
</p>

<p align="center">
  <img src="/images/uploads/tesis-top-view-signaling-board.jpg">
  <br>Top/side view, signaling control board pulled out
</p>

Whenever we turned on the boiler to do some tests, everyone in the entire campus knew that we were working on it. It was soooo noisy when we released the pressure that everyone turned around looked up to the electronics lab to see what the heck was going on.

Ah, and the fancy box was build by my thesis partner's father Fernando Giménez. He was truly an awesome carpenter!

True story: twenty years later, in 2000, during my interview at Intel, one of the interviewers was Ralph Henkhaus. During my session with Ralph, I don't know how, but we ended up chatting about my thesis project, and when I told him that we used RMX/80 to build it, he almost fell off his chair. Ralph had been the engineering manager of the Intel RMX/80 project back in the 70's. What a small world! Ralph and I ended up working together for a few years, it was a real pleasure to work with him. We still stay in touch every now and then.

Well, that's my time travel project... Coming up in future posts, freeRTOS and Arduino. Until then,

Au revoir!
