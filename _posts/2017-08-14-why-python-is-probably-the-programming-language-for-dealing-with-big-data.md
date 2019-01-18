---
layout: post
title: "Why Python is probably the programming language for dealing with Big Data"
author: "Lasse Schultebraucks"
comments: true

---

Python is a very amazing programming language and is very popular these days, so you have probably heard about Python already, you have tried it out or you use it even as your main programming language at work. Therefore I want to talk in this blog post why Python is the language (or at least one great language) when it comes to dealing and working with Big data.

### Inspiration

My inspiration for this video actually came from a talk from Jake Vanderplas at PYCON 2017. Jake Vanderplas is a astronomer and talks in his talk about why and how astronomers use Python. You can of course watch the full talk, but in case you do not have the time, I have made a summary for you.

[![Jake Vanderplas - Keynote - PyCon 2017](https://i.ytimg.com/vi/ZyjCqQEUa8o/maxresdefault.jpg)](https://www.youtube.com/watch?v=ZyjCqQEUa8o "Jake Vanderplas - Keynote - PyCon 2017")

In the first few minutes of his talk he explains how astronomers work and what they do. So first he makes clear that astronomers don’t watch through classic telescopes like everybody would expect, they have big telescopes like the Hubble Space telescope or the Kepler telescope. These telescopes collect big data chunks with various features but they don’t make clear pictures of e.g. whole planets. To get information out this information, they write statistical code mostly based on Python. In the last year astronomers really explored the possibilities of Open Source. So e.g. the whole Kepler project code is online on [GitHub](https://github.com/KeplerGO). He also talks about the James Webb Space telescope, which should replace the Hubble Space telescope. But instead of watching the visible light it will watching infrared light, to get better images from exoplanets and get general more information about exoplanets. And like the code for Kepler, the code for the data analysis of the James Webb Space telescope is also mostly written in Python and is online at [GitHub](https://github.com/STScI-JWST). As a last telescope he mentioned the Large Synoptic Survey Telescope which is a ground based telescope with the largest digital camera ever created with the size of 3 GigaPixel. So the telescope will make two exposures every 30 seconds and creates 15-30 TerraByte per night with a final 10 year catalog of hundreds of PetaBytes of data. This is very impressive! And again, the software for the data analysis is mostly written in Python (and C++) and you can check it out again at [GitHub](https://github.com/lsst-ts). So according to Jake Vanderplas, there was currently a big revolution in the astronomy in the last ten years. Python got way more important to the scientific field in general, so e.g. Python seems to became the primary language for astronomers in the last 10 years.

![Software in Astronomy]({{site.url}}/assets/softwareAstronomy.jpg "Software in Astronomy")

### So why Python?

In his second part of his talk Jake Vanderplas talks about why Python is so effective in science. So basically Python was developed for learning to program and it was never intended as a primary language for programmers, but as you might recognized, Python got one of ten most important programming language in the last years. As reasons why especially scientist and astronomers use Python Jake Vanderplas list some key points:

1. Interoperability with Other Languages
Scientist work with a wide variety of systems, databases and interfaces. So instead of wasting the most of her time trying to fit all their components together, they use Python, because Python makes it very easy to put together different system components. “Python is a Glue”, so Jake Vanderplas.
2. “Batteries Included” + Third-Party Modules
As another reason he says that Python is also so important for scientists, because there are so many tools already included in Pythons Standard Library and the scientific stack in the last years increased heavily.

![The Science Stack of Python]({{site.url}}/assets/pythonStack.jpg "The Science Stack of Python")

### My view on Python

Jake Vanderplas mentions a lot of good points in his talk which make totally sense to me. First, he talks about the big data sets astronomers and probably many other scientists in different areas, astronomy is just a good example here, collect and analyze. In the past scientists used programming languages like Fortran, MatLab and maybe even C/C++. But the developing and analyze with this kinds of tools were hard, because the programming languages were not very intuitive. But Python is very intuitive and for everybody who can not program yet very easy to learn. It is Open Source and has therefore a great standard library and other tools like mentioned above to support easy and fast work for analyzing big data. So I would totally agree on his points here. As also mentioned in his talk, machine learning libraries like sci-kit or TensorFlow who are also intended to deal with huge data sets, are based on Python. So Python definitely seems to big the language who for dealing with Big Data. But what I asked myself after the talk was: What are the negative points on Python?
One point what should be clear and what Jake Vanderplas also have mentioned in his talk is that Python is not the fastest language compared to e.g. C/C++. That is of course a contra argument, especially if you want to write programs who should deal with a lot of data. But because the development process with Python is much faster than with C, it seems that the performance difference does not matter. So this is really nice, because developing with Python instead of C is much easier, because you do not have to care about pointers, memory management and other low-level stuff and instead you can concentrate about the real problems which you are facing. Another point are the design restrictions in the Python language. Python is dynamically typed and this leads to more testing and also possible errors. But as you seen in the web there is JavaScript, who is also dynamically typed and has huge success. So are dynamic typed languages better than static typed languages? This is probably difficult to answer and I would say it depends. On the hand side development time is faster, but on the other there could be created runtime exceptions, which already could have catched at the compile time with static typed languages. But this is a question that is definitely not easy to answer and I therefore can not answer here, also because there are many pro and contra arguments for and against it.

### Resume

As the last years showed as, Python became very important for the scientific community and the software development industry. Big companies like Google (YouTube), Facebook, Dropbox or Instagram use Python in different ways. It is getting more and more popular and it is by far not only a programming language for beginners and for students who want to learn their first programming language. There are great libraries out there and everybody who know how to program can pick it up and learn very fast. There are also great machine learning libraries like TensorFlow and sci-kit out there and I think the growth of machine learning in the next years will be another huge factor why Python will get even more important in the future.
