---
layout: post
title: "Creating a blog with GitHub pages"
author: "Lasse Schultebraucks"
categories: Random
---

I recently moved my blog. Again... WordPress (finally) died for me and now I can host my blog with an own domain, a SSL certificate totally free.

So there are a couple of different reasons why I left WordPress and bluehost hosting. 

First hosting a WordPress blog on bluehost costs around 2,95$ every month. For just a personal blog without business model that makes money it literally cost 2,95$ every month. So it is not really worth it, especially if you does not commit heavily to blogging.

Beside reasons of money there are also a couple of technical aspects like security and speed.

The mentioned aspects are solved by GitHub Pages. GitHub pages let you host a static website directly from your GitHub repository. I am general a big Git and GitHub fan, so GitHub Pages is literally the perfect solution for my needs. It is free, fast, highly customizable and with a CDN like Cloudflare you also can get an free SSL certificate for HTTPS. The SSL certificate is a very important aspects for me, because it creates trust between user and the site. Also in Chrome 68 all non HTTPS sites will warn the user more clearly.

As a static site generator I used Jekyll, which is a simple site generator which focus on blogs. Blog post like this I am currently writing are written e.g. in Markdown and then be rendered to HTML. The setup is very easy. There are a couple of requirements like Ruby, Ruby Gems, GCC and Make. You also have to setup your jekyll blog on either Linux or macOS. You also can install jekyll on Windows, but Windows is not official supported and you definetly have to enable the Bash on Ubuntu on Windows enabled. After you have installed all the requirements, you can install jekyll with

```commandline
gem install jekyll
```  

I first installed jekyll 2.7.5, but because I later had problems with this particular version, I downgraded to 2.7.4 by

```commandline
gem install jekyll -v '2.7.4'
```

which worked fine for me. So in case 2.7.5. or higher does not work for you, try out 2.7.4.

Next there are a couple of different reasons how you may to advance. You can either directly fork or clone an existing theme repository and customize it for you own needs (I did this) or you can create an jekyll site by typing

```commandline
jekyll new my-blog
```

You have to run this command not in sudo and enter the password manually if jekyll asks you after it. 

`jekyll new` will generate a blank static site for you. Now you can go on and change a theme. `minima` is the default theme installed. You can change it by editing the `_config.yml`.  There are a couple of themes out there that can be installed in that way and there are also many more out there which can be installed manually.

With `jekyll serve` you can serve your page locally.

After setting up your theme and configuring everything else you many want to go live and also setup your own custom domain and a SSL certificate. You just have to push everything to your Github repo which is named after the schema username.github.io, so for me it is lschultebraucks.github.io.

If you have a custom domain you have to do a couple of things to do to use it for your GitHub Pages. In your repository settings you have to enter your custom domain under Custom domain and click save. There should be a filed named `CNAME` in your directory repository now, which content is your domain. Next you have to create a DNS record at your web hosting provider. I use bluehost, so I did go to the domains, and then zone editor, selected my domain and added a new DNS record there. [This blog post](https://bryancshepherd.com/data-science/setting-bluehost-dns-github-jekyll-blog/) helped me setting everything up.

Next you probably also want a free SSL certificate, so you have to create a free Cloudflare account and enter your domain there. There will be 2 name servers which you have to change under your web provider, in my case bluehost. You also have to activate the the option Always use HTTPS in the Crypto section. This will redirect all requests with the scheme http to https. I first forgot these, but with the help of [Ryan Palo](https://twitter.com/paytastic) (Thanks again). With his help I managed to setting up everything correctly.

If you have set up Cloudflare, it will cache your site on different servers and provide a free SSL certificate for HTTPS. So through Cloudflare your GitHub pages will be faster and more secure.

Content can simply be created by markdown and HTML. Everything is highly customizable, which is also like. I have not customized the theme yet a lot, but I will probably do this in the next couple of weeks.

There are also other options beside jekyll to create static sites. I heard that harp.js is pretty solid, too.

If you have any questions or you are stuck at setting up your own jekyll site with GitHub Pages, you can reach me out at [Twitter](https://www.twitter.com/LSchultebraucks). I am glad to help.