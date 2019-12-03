+++
title = "Py Metis HTTP Monitor"
date = "2019-11-15T13:50:46+02:00"
tags = ["random","tech","python","coding"]
categories = ["Python"]

+++


Making your own HTTP monitor without breaking the bank can be difficult if you need to scale your monitoring up to thousands of URLs.  In most cases, you want to hit some URL every 5 minutes, sometimes every minute.  If you have a long list of URLs, then you run into a few constraints:

* The scan of the URLs has to complete before the next run - monitoring every 5 minutes means your ping and processing has to take less than 5 minutes
* Jobs can't overrun or you risk spamming your list with HTTP hits
* Processing the results into useful alerts can also constrain you, but there are multiple ways to approach alerting depending on your needs

I've seen implementations in Go that make it easier or some other language geared towards concurrency. However, I prefer Python and in 3.x async operations have really improved.  I recall in 2.x using multithreading/processing was sometimes tedious and not suited for network ops.

Now we live in the future with the aiohttp and asyncio libraries.  And I wanted to learn how they worked, since there are quite a few instances in my day-to-day where I'd want to perform concurrent tasks in Python.

After a bit of googling, I found [this link](https://pawelmhm.github.io/asyncio/python/aiohttp/2016/04/22/asyncio-aiohttp.html) and shamelessly took most of the code. A few updates later and I've got the basis for the entire monitoring system in the [Py Metis repo](https://github.com/martysohio/pymetis).  

Overall, I'm happy with how it came out. The code is short and the majority of the repo is the docker setup for additional services like Grafana and InfluxDB to visualize the results.