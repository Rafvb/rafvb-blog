+++
title = "Blogging with Hugo on Azure"
description = "Setting up a new blog (again)."
tags = [
    "hugo",
    "azure",
    "blog",
]
date = 2021-03-01
+++

## The blog is back!

After a long break (my last blog post was from 2015) I decided to pick up blogging again.
And of course this meant I had an excuse to try something new 😁

My [first blog](https://rafvb.wordpress.com) was a wordpress blog, hosted by wordpress.
Then i switched to [a static site](http://rafvb.github.io) using Jekyll on github.

This time I am still going for a static website using [Hugo](https://gohugo.io/). Most of my professional work uses Azure, so I wanted to also host my blog there using a static web app.

## Hugo

I did not spend a whole lot of time picking a static site generator to be honest. I wanted something easy and fast and found [Hugo](https://gohugo.io/).

And, my god, it's fast...

{{< image src=hugo-logo.svg alt="Hugo logo" >}}

Written in GO, but that is an implementation detail when you use it, since you just install the binaries.

I installed the latest version using [Chocolaty](https://gohugo.io/getting-started/installing/#chocolatey-windows) on my windows machine.

## Setting up Hugo on Azure

On Azure I am using an [Azure Static Web App](https://docs.microsoft.com/en-us/azure/static-web-apps/overview). This is easy to integrate with GitHub so it can automatically trigger a new release on a commit to my git repo.

_Azure Static Web Apps is currently in preview, so no pricing info is known yet. For now it's free_

To set it up I followed [these steps](https://docs.microsoft.com/en-us/azure/static-web-apps/publish-hugo). As a [theme](https://themes.gohugo.io/) I went with GhostWriter.

Since the GhostWriter theme has a lot of settings you can tinker with I replaced my config.toml in the root of my blog with the config.yml provided in the themes/ghostwriter folder. I also copied the sample content pages over from the themes folder into my own content folder.

Using the command ```hugo server``` I verified all was working well locally.

And then I pushed my changed and watched how all the glorious machinery spun into action to deploy my fresh website.

## Issues

When I opened up my blog on azure I was greeted by an empty page.

The Developer Tools from my browser showed me my requests where blocked because they where [cross origin](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS). I forgot to change the baseurl in my config.yml, which wasn't a problem locally. Changing the setting and pushing to github triggered the glorious machinery again and my page was working! 

Then I started writing this blog post, but when i checked it out locally I could not see it.
Turns out I had set the date of the post to be in the future (because it was still a draft) which is why the post was still invisible.

## Adding a custom domain

In the next post we'll see how we can add a custom domain to our shiny new blog!