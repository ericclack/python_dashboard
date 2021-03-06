# Python Dashboard

Scrape information from websites and show things on your own dashboard.

Tutorial for Brighton Coder Dojo.

What you will learn: 
- Using a real IDE such as Visual Studio Code
- How to install Python libraries
- How go get web content from a URL
- How to pick out info from the page using XPath expressions
- How to build a Graphical User Interface (a GUI)

## Set up

Check out this repo onto your computer:

```
git clone https://github.com/ericclack/python_dashboard.git
```

Install the packages we need:

```
pip3 install --user -r requirements.txt
```

## Test it out

```
python3 dashboard0.py
```

Go and look at the [source
code](https://github.com/ericclack/python_dashboard/blob/master/dashboard0.py)
for this file and to see how we get the web page and the title from
it.

Now try the next example:

```
python3 dashboard.py
```

This gets the title, plus the individual forecasts for each day. 

## Add your own information

Have a look in the `dashboard.py` file to see how we get weather data
from the BBC website. There are three steps to do this:

1. Use `requests.get(url)` to get the webpage
2. Turn the HTML into a tree structure, which makes it easier to find what we need, using `html.fromstring(page.content)`
3. Use `tree.xpath(path)` to pick out the data we need.

### How to write the XPath

First find the page you want to use in your web browser. Now find the
data you want on the webpage and select it with your mouse. Now you need
to see the HTML source code behind this content...

* In *Chrome*, right click your mouse and choose *Inspect*
* In *Safari* you first need to tick *Show Develop menu in menu bar* on the *Advanced* tab in the *Preferences* window, then you can right click and choose *Inspect Element*
* In *Edge* ???

Now the hard bit, you need to see the set of tags and attributes that will identify the information you need. Here's an example...

Let's say you want to display Lego news headlines from this page: https://www.thebrickfan.com/

When you inspect the source code, you see headlines coded like this...

```
<div class="item-details">
     <h1 class="entry-title"><a href="https://www.thebrickfan.com/new-2021-set-teased-for-lego-black-friday-showcase/" rel="bookmark" title="New 2021 Set Teased for LEGO Black Friday Showcase">New 2021 Set Teased for LEGO Black Friday Showcase</a></h1> <div class="meta-info">
     <div class="td-post-author-name"><div class="td-author-by">By</div> <a href="https://www.thebrickfan.com/author/tormentalous/">Allen "Tormentalous" Tran</a><div class="td-author-line"> - </div> </div> <span class="td-post-date"><time class="entry-date updated td-module-date" datetime="2020-11-25T07:11:34+00:00">November 25, 2020</time></span> <div class="td-post-comments"><a href="https://www.thebrickfan.com/new-2021-set-teased-for-lego-black-friday-showcase/#respond"><i class="td-icon-comments"></i>0</a></div> </div>
<div class="td-post-text-content">
     ...
```

So the headline is in an `<a>` tag, which is in an `<h1>` tag. Therefore we can try the XPath:

```
print tree.xpath('//h1/a/text()')
```

The first `//h1` means find all `<h1>` tags wherever they are in the document, and `text()` means get the text inside the tag, that is between <a> and </a>

### Problems with JavaScript

Many modern websites use JavaScript to render their content and this makes it hard to scrape. So you will often see something like this in the page title:

```
['One more step', 'Please turn JavaScript on and reload the page.']
```

What to do?
- You could find an RSS feed instead (see the next section)
- Or... watch this space! We are trying to discover a way to make this work. 

### Getting news from RSS feeds

RSS feeds can be easier to work with than HTML pages because their structure is simpler and therefore it is easier to make XPath expressions.

Have a look at [dashboard_rss.py](https://github.com/ericclack/python_dashboard/blob/master/dashboard_rss.py) to see a working example.

You might notice a slight difference compared with our HTML examples:
we use `etree` to make the tree instead of `html` on line 1 and 10.

## Adding a GUI

Take a look at the example `dashboard_gui_qt.py` to see the beginnings
of a GUI for our data.

And `dashboard_lego_gui.py` includes buttons for opening links.

## Finding out more about Qt

* http://zetcode.com/gui/pyqt5/ - Tutorial
* https://wiki.qt.io/Main - Qt Wiki
* 
