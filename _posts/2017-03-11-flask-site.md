---
layout: post
title: A webapp that scrapes and displays the frontage of news.ycombinator.com
date: 2017-03-11 12:19:20
categories: cool
---

A website created in [Flask](http://flask.pocoo.org/docs/0.12/), to display the frontpage of [HackerNews](https://news.ycombinator.com/news)


##Introduction

You will need Python 2.7 or newer on your computer to get started. The version of python that is included can be updated by using the [Homebrew](https://brew.sh/) package manager.

To install Homebrew open the Terminal and run
```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)
```

Once Homebrew is installed, open your ~/.profile file in your favorite text editor and add this line to the bottom.
```
export PATH=/usr/local/bin:/usr/local/sbin:$PATH
```

This will ensure that Homebrew packages are added to the System Path

Now, we can install Python 2.7:
```
brew install python
```

Install virtualenv to setup an isolated development environment. Virtualenv is a tool to create isolated Python environments. Virtualenv creates a folder which contains all the necessary executables to use the packages that a Python project would need. In Terminal run
```
sudo pip install virtualenv
```

Create a directory where you are going to be working from.
```
mkdir flask_python_hn_front_page_scraper
```

Navigate to the directory
```
cd flask_python_hn_front_page_scraper
```

Create a virtual environment for the project
```
virtualenv my_project
```

The name of the virtual environment in this case is "my_project." It can be anything.

To begin using the virtual environment, it needs to be activated:
```
source my_project/bin/activate
```

We will be using the flask micro framework to display the scraped content from news.ycombinator.com. To install flask:

```
pip install Flask
```

Create a file named app.py and open it in your text editor. I personally use [Atom](https://atom.io/).

```
touch app.py && atom .
```

Start off by creating “Hello, World” in flask:

```python
from flask import Flask
# first we imported the Flask framework

app = Flask(__name__)
# Next we create an instance of flask.

@app.route(‘/')
# We then use @app.route() to tell Flask what URL should trigger our function. In this case the root directory of the site.

def hello_world():
    return 'Hello, World!’
# The function is given a name which is also used to generate URLs for that particular function, and returns the message we want to display in the user’s browser.

if __name__ == '__main__':
    app.run(port=5000, debug=True)
# We are telling the script to run on localhost port 5000 and in debug mode to show error messages.
```

Save the file.

In the terminal window run: 
```
python app.py
```

Open your web browser and head over to http://127.0.0.1:5000/, and you should see your hello world greeting.

You can now stop running the script by holding “ctrl c” in the terminal window. Keep the terminal window open to avoid the  inconvenience of navigating to the directory and starting the virtual environment up again.

##Scraping

We will be using the urllib2 library to access the page and BeautifulSoup to pull the titles off of the page.

The urllib2 library is included in python.
To install BeautifulSoup run in the terminal window:
```
pip install bs4
```

Create and open a new file named scraper.py
```python
import json # We will be using json to output the data from the website
import urllib2 # The library that accesses the internet and downloads the webpage data
from bs4 import BeautifulSoup # The library that searches the webpage data for html tags

def gethnfrontpage():  # A python function to return the stories off of the front page
   response = urllib2.urlopen("https://news.ycombinator.com/news") # urllib2 opens the website address and downloads the html
   stories = [] # An empty list of stories that will be filled once BeautifulSoup locates them
   soup = BeautifulSoup(response, "html.parser") # Tell BeautifulSoup to read the html that was downloaded by urllib2
   posts = soup.select(".storylink")
   '''
   Tell BeautifulSoup to select the html tag with the class storylink.
   I determined the html tag of the news.ycombinator links by viewing the source of the webpage.
   Here is a link that I pulled off of the page to use as an example. Notice the html class?
   <a href="https://www.nytimes.com/2017/03/10/opinion/sunday/can-sleep-deprivation-cure-depression.html" class="storylink">Can sleep deprivation cure depression?</a>
   '''
   for post in posts: # Now that the posts have been found by BeautifulSoup, they can now be outputted as json.
       stories.append({"title":post.text, "link":post["href"]}) # The post title as well as the link to the post will be included in the list of stories that we created earlier.
   return json.dumps(stories) # The function that we created returns the stories as json which will make them easy for us to display later.


print gethnfrontpage() # We can temporarily output the function to our terminal window to make sure that it works.
```

Now we will be combining scraper.py and app.py.
```python
from flask import Flask, render_template, request # We will want to create a pretty html view on our own so render_template and request need to be imported
import json # I have included json from scraper.py
import urllib2 # I have included urllib2 from scraper.py
from bs4 import BeautifulSoup # I have included BeautifulSoup from scraper.py
app = Flask(__name__) # Create an instance of flask.

def gethnfrontpage(): # I have pasted the function from scraper.py and removed the line print gethnfrontpage(). We will be instead sending the output of the function to flask
    response = urllib2.urlopen("https://news.ycombinator.com/news")
    stories = []
    soup = BeautifulSoup(response, "html.parser")
    posts = soup.select(".storylink")
    for post in posts:
        #title = post.text
        #link = post["href"]
        stories.append({"title":post.text, "link":post["href"]})
    return json.dumps(stories)

@app.route('/') # The root directory of the site.
def get_hn(): # The function to generate content
    frontpage = gethnfrontpage() # Running the gethnfrontpage function
    return render_template("hn.html", hn=json.loads(frontpage)) # Outputting the json from gethnfrontpage to a file named hn.html

if __name__ == '__main__':
    app.run(port=5000, debug=True)
# We are telling the script to run on localhost port 5000 and in debug mode to show error messages.
```
Create a directory inside the flask_python_hn_front_page_scraper titled templates

Inside that directory create a html file named hn.html
```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>HN NEWS</title>
    <!--A simple html page that includes Bootstrap for css styling -->
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css" integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u" crossorigin="anonymous">
  </head>
  <body><div class="row">
    <div class="col-md-4">Scrape HN</div><!--Putting the title on the left side of the content-->
    <!-- a for loop to output the content from the gethnfrontpage function. For each link from the gethnfrontpage function output its direct link and title in the form of an html link. -->
    <div class="col-md-4">{% for news in hn %}
    <b><a href="{{news['link']}}">{{news['title']}}</a></b><br/>
    <hr />
    {% endfor %} <!-- end the loop -->
  </div>
    <div class="col-md-4">frontpage</div> <!-- Second half of the page title on the right side of the page -->


</body>
</html>
```

Save the file.

In the terminal window run: 
```
python app.py
```

Open your web browser and head over to http://127.0.0.1:5000/
