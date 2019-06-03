---
layout: post
title:  "Built My Own Website By Using Jekyll"
date:   2019-05-23 08:57:03 -0400
categories: Interesting-projects
author: Ningxiao Zhang
permalink: "/posts/build-my-own-website/"
---

I have been thinking of building my own website for years. Hyper Text Makeup Language (HTML) is the standard markup language for creating web pages. In addition to HTML, you also need to learn Cascading Style Sheets (CSS) and JavaScript to form a functional website. This is a big learning challenge for me who just want to learn how to make a functional personal website. Recently, there are some good websites like "Wix" helps users to create Websites through the use of online drag and drop tools. This makes it easy and quick to build up a website just like making PowerPoint slides. However, the fact that few free themes and limited adjustments does not meet my requirements.

Luckily, I found a website design YouTube tutorial for beginners by Mike Dane. He used Jekyll to program the website. Jekyll is a simple, blog-aware, static site generator for personal, project, or organization sites. Jekyll is very similar to another software that I used in writing my papers -- Latex. You can apply a beautiful free theme by one line command and then include your contents in a markdown formation which is friendly for including math equations in my research. There are thousands free Jekyll themes published online. And it is also easy to adjust the theme for your personal use. Moreover, it is easy to post your website on GitHub for free. Jekyll is kind of a compromise way between building website all by yourself and using a simple limited template.

Griaffe Academy has a series of videos that will help you to learn the basics of using Jekyll. If you want to have a fully understanding of Jekyll including installing, setting, and using complex features, here is the link of the YouTube tutorial:

[<img src="../../assets/img/Jekyll.jpg">](https://youtu.be/T1itpPvFWHI)

If your are just looking for a simple and free way to build your own customized website, here are some steps that I used to setup my website on my MacOS system with the terminal commands.

1.Setting Environment (MacOS Version 10.14.5):
==================

  You need to use Ruby to install Jekyll. The MacOS system usually installs Ruby as default. The requirement of Ruby for Jekyll is version 2.4.0 or above.  Type `ruby -v` in the terminal to check the version of the Ruby on your laptop.

  I found it does not meet the requirement. I used `brew install ruby` to update my Ruby to the newest version (ruby 2.6.3p62).

  Then, install bundler and jekyll by using `sudo gem install bundler jekyll`.

2.Create A Jekyll Website:
===================

  Find a place in your computer to put your website folder. Move into the folder through the terminal. Then, created a new folder (I named it "My_Web".) to save all the files for your website by using `jekyll new My_Web`. This is a command initialling website files for you and putting them in this new folder.

  To edit this website, you need to move into this new folder. The home page is saved in the "index.md" file. The blogs are saved under "posts" folder with markdown formate. You can edit them in any document editor on your computer (I used Atom).

  To check your website locally on your own computer, you need to move into the folder you just created in the terminal. And type in `bundle exec jekyll serve`. Then you can check your website locally on the Safari with the server address showing in your terminal, something like "127.0.0.1:4000/My_Web/".

3.Host A Website On GitHub:
==================

  It is easy and free to publish a website which is created by Jekyll if you already have a GitHub account. You just need to create a new repository in you GitHub and clone it to your computer. Then, copy and paste all the website files into this local repository. Sync your local repository with the remote repository and your will find your website address in the setting of your GitHub repository.

If you have any question about building your website, please feel free to ask me through my email. Good luck and have fun!
