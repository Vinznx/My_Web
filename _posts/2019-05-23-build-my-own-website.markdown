---
layout: post
title:  "Built My Own Website By Using Jekyll"
date:   2019-05-23 08:57:03 -0400
categories: Interesting-projects
author: Ningxiao Zhang
permalink: "/posts/build-my-own-website/"
---

I have been thinking of building my own website for years. However, I don't have much time to study Hyper Text Makeup Language (HTML) and use it to build my personal website. Luckily, I found a YouTube video from DevTips by a website designer, Travis Neilson, teaching "How to build a website from start to finish". Then, I figured it out that it is not that hard to build a website by other software. To understand more about how does the code in the video works, I studied to use Jekyll through its tutorial videos by Mike Dane. Here, I show you how I setup my first personal website:

[<img src="../../assets/img/Design+code.jpg">](https://www.youtube.com/watch?v=sJhhLvW-Xvg&list=PLqGj3iMvMa4KeBN2krBtcO3U90_7SOl-A&index=1)
[<img src="../../assets/img/Jekyll.jpg">](https://youtu.be/T1itpPvFWHI)

1.Introduction
=================

  "Jekyll is a simple, blog-aware, static site generator for personal, project, or organization sites." (Wikipedia) Jekyll is very similar to another software that I used in writing my papers -- Latex. You can apply a beautiful theme by one line command and then include your contents in a markdown formation which is very easy to use. There are thousands free Jekyll theme published online. And it is also easy to post your website on GitHub.  

2.Setting Environment (MacOS):
==================

  The requirement of Ruby is version 2.4.0 or above. My MacOS system installs Ruby as default. I opened a terminal on my laptop and use `ruby -v` to check the version. I found it does not meet the requirement. I used `brew install ruby` to update your Ruby to the newest version.

  Then, I installed bundler and jekyll by `sudo gem install bundler jekyll`.

3.Create A Website:
===================

  I created a folder (NZ_web) to save all the files for my website. Then, I ran `jekyll new My_Web` in the terminal to generate a new folder with initial website files. Moved into this new directory and made it available on my local server by `bundle exec jekyll serve`.

4.Host My Website On GitHub:
==================

  I created a new repository for my website and cloned it to my laptop. Copied and pasted all the website files into this local repository. Synced it with my origin repository and then generated my first website on <https://vinznx.github.io/My_Web/>.
