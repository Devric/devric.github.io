---
title: Bloging again with wintersmith
author: Devric
date: 2014-04-14 22:00
template: article.jade
---

I was using co.cc a couple years ago and a simple wordpres blog, but since co.cc shuts down, and haven't really backed up my files, I stopped blogging, until Wintersmith!

Here's how you set up one with github pages.


<span class="more"></span>

I had my domain name devric.io for almost 2 years but it seems theres an issue forwarding the address with CNAME to use with openshift host from Gandi register, issues submited to their technicians but seems takes ages for them to respond.

So i stumbbled on to wintersmith a static site generator like jekyll but for nodejs. AWESOME! I can just use github pages as my personal blog now! Its free and its easy to modify, and no more database backup needed, everything is static via github hosting.

Here's how you do it:

#### Setting up

1 - create a new repo on your github, repo name:  yourGithubAccount.github.io

2 - clone it to your local

3 - npm install wintersmith -g

4 - cd to your local repo folder

5 - wintersmith new admin

6 - cd in to admin folder that you just created

7 - add a line output : "../" in the config file, can be the first option of the json object

8 - now you can preview your site by typing wintersmith preview

9 - you should be now able to see it on your browser with localhost:8080

10 - generate your static stite to the root folder type wintersmith build

11 - commit your new stuff and push, you should see your site on yourGithubAccount.github.io within 4-8 minutes

#### Adding articles

All the post with wintersmith are within contents/articles folder, and its written with markdown, you can look through their samples articles.

To create a new article, you just need to create a new folder with index.md to start with. The folder name does not matter, I use date-title to name these folders.

Copy the first part of the config from other articles to your index.md, this config is how wintersmith index your article title.

this is my config for this post

```
---
title: Bloging again with wintersmith
author: Devric
date: 2014-04-14 22:00
template: article.jade
---
```


#### Add comment box

Since this is a static site, theres no backend. But we can still add our comment system with disqus, simply go into the template folder and add the universal disqus code into the article.jade template file, you will need to learn jade lang a bit, don't worry its really easy! Just follow the instruction on disqus and add it into a script. tag within your template right under your article.


I'll start to theme it soon, and put out guides on theming wintersmith :)

