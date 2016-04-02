+++
date = "2016-04-02T10:37:49-07:00"
draft = false
title = "Hotel - An easier way to manage local .dev environments"
category = "Dev"

+++

I just recently stumbled across [Hotel](https://github.com/typicode/hotel), a super simple way to manage local dev environments without having to resort to manipulating your /etc/hosts file and having lots of terminal windows open to spin up/down your various applications.

3 simple steps:

1. **Install hotel:** `npm install -g hotel`
2. **Start hotel:** `hotel start`
3. **Add your application:** `hotel add '<command to start your application> $PORT`

Running command 3 from the root of your applications folder, and passing in the **$PORT** variable will allow your application to be started on a random port from a slick UI.

Try it out!