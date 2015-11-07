---
layout: post
title: How to host a professional static site for free.
date: 2015-11-07 2:19:20
categories: Cool
---

I recently setup rorycornell.com using [Jekyll](https://jekyllrb.com/), an outstanding static website generator that is designed for quickly building clean and professional blogs.  Jekyll is designed to hit the ground running.

Jekyll was created by the co-founder of GitHub, [Tom Preston-Werner](http://tom.preston-werner.com/) to blog using a simple static HTML website.


Jekyll takes content written in [Markdown](https://en.wikipedia.org/wiki/Markdown), passes it through templates and creates a complete static website that is ready to be hosted. 


GitHub Pages will conveniently serve the website directly for free from a repository.




##Why Go Static?
1. ###Simplicity
Jekyll eliminates complexity when trying to setup a blog

2. ###No database
Unlike WordPress and other content management systems (CMS), Jekyll does not have a database. All posts and pages are converted to static HTML. This is great for reducing loading times because no database calls are made.
3. ###No CMS
Create your content in Markdown, and Jekyll will run it through templates to generate a static website. GitHub can serve as a CMS because of the built in editor on their site.
4. ###Design control
Jekyll allows users to quickly create and customize their own theme.
5. ###Security
The vulnerabilities that affect platforms like WordPress do not exist because Jekyll has no CMS or database.  You do not have to spend time worrying about security patches.
6. ###Great Hosting
[GitHub Pages](https://pages.github.com/) will conveniently serve the website directly for free from a repository. 

GitHub reguarding bandwidth: 
>>>If your bandwidth usage significantly exceeds the average bandwidth usage (as determined solely by GitHub) of other GitHub customers, we reserve the right to immediately disable your account or throttle your file hosting until you can reduce your bandwidth consumption.

###TL:DR 
Don't get ddosed ;)


##Try it out

###You will need

* Ruby
* RubyGems
* Linux, Unix, or Mac OS X


Install Jekyll using `gem install jekyll`, create a new site using `jekyll new mysite`, and then serve it using `jekyll serve`.



You can also fork an existing Jekyll site that you like on Github.
Feel free to fork my [site](https://github.com/rorycornell/rorycornell.github.io).

To fork my site hit the “Fork” button in the top-right corner of the repository to copy it to your GitHub account.

You can now change your website’s name, description, and other options by editing the _config.yml file inside your copied repo. These settings are pulled into the theme when Jekyll runs.

Click the “Settings” button in the forked repository and change the repository’s name to yourusername.github.io, replacing yourusername with your GitHub user name.

Your website will be live immediately. You can check by going to http://yourusername.github.io.


Edit /_posts/2015-10-14-first-post.md, deleting the content and entering your own. If you are new to Markdown, check out [Markdown Cheetsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)

Jekyll requires posts to be named year-month-day-title.md.


##Use your domain

I recommend that you use the free [Cloudflare](https://cloudflare.com) to manage your domain.  Cloudflare is a content delivery network with a free plan that provides analytics, HTTPS, ddos mitigation, security, caching, and improves availability for a website or mobile application.  
###If you use cloudflare
Edit the CNAME file to say your domain (rorycornell.com) as an alias for rorycornell.github.io

To utilize Cloudflare's free SSL go to the Crypto tab and enable Flexible SSL and HTTP Strict Transport Security (HSTS) 




