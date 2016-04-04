+++
date = "2016-04-04T13:28:13-07:00"
draft = false
description = ""
title = "Part 1: Zero-to-blog in less than 2 hours; Hugo for the win."

+++

I built this entire blog using 3 very basic pieces of technology, which are simple enough for any hack-a-day dev like myself to master very quickly.

* [Hugo](http://gohugo.io)
* [Bitbucket](http://bitbucket.org)
* [Aerobatic](http://www.aerobatic.com)

Using this stack, I was able to go from Zero-to-blog in about 2 hours (and most of this time was due to me not RTFM'ing). In this part of the series, I'll over how I got started with Hugo and what tripped me up when I was getting it going.

All about Hugo
----

Put simply, Hugo is awesome. It is a really simple, lightweight way to get started with building static websites. Instead of thinking about what HTML you want to form on either your portfolio, blog or landing pages, it let's you focus on what actually matters - the content.

Getting started with hugo is really simple:

```
brew update && brew install hugo
hugo new site my-static-website
cd my-static-website
hugo new post/my-first-post.md

```

With the 4 commands above, you've installed hugo, created a site called my-static-website, and created a blog post within that site called my-first-post. That's __awesome__. But how do you actually get to see the output?

Well to do that, you need to apply a theme. Thankfully Hugo has a shitload of those available that you can pick and choose from. For my purposes I chose Hikari.

```
mkdir themes && cd themes
git clone https://github.com/digitalcraftsman/hugo-hikari-theme.git

```

This is where I fucked up and ended up taking way longer to get this going than I expected. Basically, I chose not to RTFM and that resulted in me scratching my head a little bit trying to work out how to get things going. The important step that I missed was to copy the `config.toml` from within the themes `examplesite` folder; there was some important stuff in there that meant that when I went to create the public static site the build failed.

Lesson Learnt.

Once you've set that up, you can run the blog in dev by running the following command:

```
hugo server --buildDrafts
```

If that all looks good, then you can run:

```
hugo undraft content/post/my-first-post.md
hugo
```

This will create a folder for you called /public/ which is the static representation of what hugo has output (combinging your content with the selected theme). At this stage, if your site built correctly - you're now ready to get it deployed!

More details will be revealed on this in part 2, showing how you can go from here to deployed on a production website with just a few more commands and clicks.