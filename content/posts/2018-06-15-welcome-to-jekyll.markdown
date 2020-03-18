---
title: "Welcome to Hugo (from Jekyll)"
date: 2018-06-15
lastmod: 2020-03-16
draft: false
tags: ["jekyll", "hugo"]
url: /2018-06-15-welcome-to-jekyll/
---

Welcome to my wholly reworked website!  This time, I've dropped the complexity of Wordpress and opted for something significantly simpler:  ~~Jekyll~~ Hugo and GitHub.  

Having now hosted the site in GitHub Pages, here was the process I started with Hugo:

1. Created the Github Pages repo (rterakedis.github.io) -- this is where github pages looks for the blog's generated site files.
1. Created a 2nd Github repo:  rterakedis.github.io.hugo -- this repo holds the source files for Hugo to parse and generate the site.
1. Added the GH Pages repo (rterakedis.github.io) as a submodule for rterakedis.github.io.hugo
1. Edited the config.toml in the hugo files to include the following:
  * `baseURL = "https://blog.euc-rt.me/"`
  * `publishdir = "rterakedis.github.io"`
1. Add the CNAME file into the rterakedis.github.io repo and enable the custom name/https in the repo settings
1. Ensure the output from `hugo` builds into the rterakedis.github.io directory in my local rterakedis.github.io.hugo directory
1. Commit and push everything to GitHub



A few quick links that I found particularly helpful when I was working with Jekyll:
* [How I'm using Jekyll in 2016](https://mademistakes.com/articles/using-jekyll-2016/#fn-2)
* [Secure & Fast Github pages with CloudFlare](https://blog.cloudflare.com/secure-and-fast-github-pages-with-cloudflare/)
* [Utterances - Blog Commenting using GitHub Issues](https://utteranc.es/)
