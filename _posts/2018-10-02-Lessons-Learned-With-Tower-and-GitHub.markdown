---
layout: post
title:  "Lessons Learned with Tower and GitHub"
date:   2018-10-02 13:21:49 -0400
categories: tower github
---
So I've started looking into using Tower as a more fully-featured replacement for GitHub Desktop.   I like how it provides a more visual representation of branching/merging.   One thing I've run into is that when I initially set it up, I was able to clone/fetch my GitHub repos, but for some I was unable to push commits.  I kept getting the following error:

```
Pushing to https://rterakedis@github.com/vmwaresamples/AirWatch-samples.git
remote: Permission to vmwaresamples/AirWatch-samples.git denied to rterakedis.
fatal: unable to access 'https://rterakedis@github.com/vmwaresamples/AirWatch-samples.git/': The requested URL returned error: 403
```

Still working on why I can't seem to push commits to GitHub through [Tower](https://www.git-tower.com/mac)...  If I figure it out, I'll update the blog!