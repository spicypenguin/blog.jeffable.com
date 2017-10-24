+++
date = "2017-10-23T21:19:00-07:00"
draft = false
description = ""
title = "Todo-ello: Turning trello into a GSD machine"

+++

A quick blog post to introduce something that I've been working on lately.

It had always bothered me that while [Trello](https://www.trello.com) was super flexible, and I could use it to manage my todo list pretty nicely - it was a little overwhelming and not exactly targeted for my usecase.

It provided you a lot of options on how to manage your cards:

* You could create a "done" column and move tickets to it when they were done
* You could archive cards and move them off your board
* You could use a calendar powerup which would drop them out of view as time passed on
* You could use a checklist and tick off things on an individual ticket

These options all worked to a degree, but they didn't give me what I craved - a sense that I was getting through my work and a visible indication of my progress.

So what started as a 30 minute throwaway project is now something that I'm hosting on a basic [s3/cloudfront](https://aws.amazon.com) static app that you can use too.

# Introducing [Todo-ello](http://todoello.ididthis.xyz/)

Todo-ello is an app with one purpose - make it easier to manage your TODO list inside [Trello](https://www.trello.com).

It lets you pick 3 lists from an existing board, and then pulls in all cards from that board for display.

It then will give you an option to check each item off your list, as well as providing a mechanism to see what you've done in the 24 hours.

Simple!

I'll be continuing to refine and make the app less shit, but it's currently in MVP state and is useable.
You can find the source code at [Bitbucket](https://bitbucket.org/jsinclair_aus/todo-ello/overview) - feel free to submit a PR if you have any changes that you would suggest.