---
title: Liberating Your Goodreads Reviews
author: ''
date: '2020-10-18'
slug: []
categories:
  - books
  - data-science
tags: []
math: no
meta: yes
toc: false
---

One of my main objectives with this site was to have a place I own to post content that I create. When you post reviews on Goodreads, you don't really own them--Amazon does. And if they decide to shut down the site, even if you've posted hundreds of reviews like I have, welp, it's just too bad for you. That's why one of my first steps was to write some code to retrieve all of my existing reviews from Goodreads and output them into markdown files in blog post format. 

Here, I'm including a short description of how I went about doing this, and a link to [my code on Github](https://github.com/nklagge/omnilegence). As a programmer I mainly use the R language, so the code is implemented as an R package. Other R users should be able to use it fairly seamlessly.

The code relies on the Goodreads application programmer interface, or API. An API is just a structured set of connection points that allows computer programs to interact with content posted on the web. To use the Goodreads API, you have to register to get an [API key](https://www.goodreads.com/api/keys). This is free, but allows Goodreads to keep track of whether you are abusing their service. My R package code has to send my API key to Goodreads every time I make a request, but because it's supposed to be a private key, it's not included in the package code itself--rather, I keep it in my personal .Rprofile file.

The Goodreads API has pretty good [documentation](https://www.goodreads.com/api/index), which describes all of the ways that you can programmatically request data from it. My code only really relies on two methods:{{% marginnote %}}My code includes functions using a couple of other API methods, but none of them are needed for the task of extracting reviews.{{% /marginnote %}} ```user.show``` and ```review.list```. Essentially, my R code asks Goodreads to tell me how many books are on my "Read" shelf, then asks it to dump all of those reviews to me. The main R packages I rely on to do this are [httr](https://cran.r-project.org/web/packages/httr/) to send HTTP messages back and forth with the API, and [xml2](https://cran.r-project.org/web/packages/xml2/) to take the API's responses (which are in XML format) and pull out the specific information I'm looking for. {{% marginnote %}}I also pulled in cover images from Indiebound; the Goodreads API does return cover image URLs for each book, but they're smaller than I wanted.{{% /marginnote %}}Everything else is pretty much just getting the information into one nice clean dataset. 

Once you have the information in one nice dataset in R, you could do all kinds of things with it, like track trends in your reading habits or analyze what factors correlate with the positivity of your reviews. When I was first writing this code, I was interested in doing this kind of stuff, but part of my goal with the Madrone Library is to relate to books in a bit less of a quantitative way! So the only other step I took was to dump the text contents of my reviews into a bunch of plaintext markdown files, written in a format that Hugo would recognize as a blog post.{{% marginnote %}}This is also why it looks like this website has existed since 2009 even though I created it in 2020--each post created this way is dated by my Goodreads review date.{{% /marginnote %}} This was mostly just done by iteration, hand-creating a valid post using review data and then writing general code that would produce it.

Back to the topic of owning the content you create--as you might expect, you don't truly "own" anything you write on Goodreads, even if you extract it and post it somewhere else. To use their API, you have to abide by their [Terms of Service](https://www.goodreads.com/api/terms). For the most part, the TOS are not that onerous because they're geared toward people using the API as part of apps that they create and distribute, not people doing "home cooking" with their own reviews. Still, for the purposes I was putting it to, there are two relevant provisions:{{% marginnote %}}I guess you wouldn't be bound by these terms if you manually copy-pasted content instead of using the API? I'm sure they have some other rule covering that though. {{% /marginnote %}} you have to "Clearly display the Goodreads name or logo on any location where Goodreads data appears," and "Link back to the page on Goodreads where the data data [*sic*] appears." In my initial code, I hadn't done either of these things. Now, I'm not too worried that violating these TOS would actually be an issue, both because I don't think Goodreads is going to go after a personal website, and because I think the worst that would happen is that they would revoke my API key. Still, I'm a by-the-book kind of guy and these terms didn't seem too bad. I had already been displaying in plaintext my "number of stars" for each book, so I just made this a link to the original review and included "Goodreads" in the linked text.

Go forth and liberate!