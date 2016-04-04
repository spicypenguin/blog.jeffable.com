+++
date = "2016-04-04T14:20:08-07:00"
draft = false
description = ""
title = "Part 2: From zero-to-blog in less than 2 hours; Bitbucket + Aerobatic make it schwifty"

+++

Continuing on from my last blog outlining how to get a blog started locally using hugo and a handful of commands in a shell terminal, this blog I'm going to show how to use [Bitbucket](http://bitbucket.org) and [Aerobatic](http://aerobatic.com) to deploy that blog to a static website backed by a global CDN so it is super duper responsive.

One thing that you'll need to create to make this as simple as possible is a manifest file so that [Aerobatic](http://aerobatic.com) knows how to build & deploy your site. To do this you need to create a new file in the root of your site (where `config.toml` also lives) called `package.json`.

Paste the following code into the package.json file you created:

```
{
  "_aerobatic": {
    "build": {
      "engine": "hugo",
      "themeRepo": "https://github.com/digitalcraftsman/hugo-hikari-theme.git"
    }
  }
}
```

**Note:** `themeRepo` is optional; for this example we're going to include it which will result in the theme contents being automatically grabbed and deployed from the remote git repository, rather than the files that I have stored locally.

Once you've done this, you need to create a git repository. For this example I'm using [Bitbucket](http://bitbucket.org) - mostly because of it's excellent integration with [Aerobatic](http://aerobatic.com) thanks to [Atlassian Connect for Bitbucket](https://blog.bitbucket.org/2015/06/10/atlassian-connect-for-bitbucket-a-new-way-to-extend-your-workflow-in-the-cloud/), but partially because I am biased due to the fact I work for [Atlassian](http://www.atlassian.com).

So here are the high level steps involved:

1. Create an account at [Bitbucket](http://bitbucket.org) if you don't have one already
2. Create a new repository (named something appropriate for your site)
3. Grab the HTTPS URL reference for this repository (Clone -> Change to HTTPS -> Grab the full URL)

Once you've done the above and have a HTTPS reference to your git repo, you can get to work with deployment! In the folder where you created the blog in Part 1 of this post:

```
git init
git add .
git commit -m "My first commit to bitbucket for this awesome repository"
git remote add origin https://url-from-step-3-above
git push origin master
```

With those 5 commands, you've turned your folder into a git repo, added all the files into the index, made your first commit, linked it to the git repository that you created earlier and pushed your changes to [Bitbucket](http://bitbucket.org). **AWESOME!**

The last step is to setup the [Aerobatic](http://aerobatic.com) deployment, which is as simple as:

1. Pressing the "Aerobatic Hosting" button within your Repo on [Bitbucket](http://bitbucket.org)
2. Entering the name that you want for the website (if something other than the repo name)
3. Pressing deploy

In seconds, your blog will be available at http://reponame.aerobatic.io and accessible to all! Similarly, anytime you push updates to the repository, [Aerobatic](http://aerobatic.com) will pickup these changes and redeploy your website with the refreshed content. **DOUBLE AWESOME!!**

That's it for now; reach out with any questions!
