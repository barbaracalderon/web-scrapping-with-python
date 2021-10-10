# HTML Data Extraction

In order to extract data from HTML documents, we need to inspect the HTML source code. It will tell you **where** that data is located inside the HTML. This is important because we need to know to which tag and attribute it is related to. Tags in HTML tell us the category of a data, like the `<img>` tag... it tells us it is the image category. But that alone might not be enough: we might need to specify more information in that tag. That's what attributes are for. It comes in handy to know more about HTML, because that's where we'll need to inspect in order to find where our data is located. We'll use this to extract it.

The most prominent tag attributes for Web Scraping are **class** and **id**. The attribute "class" is used to group elements of the same category to be manipulated once; while the attribute "id" is unique to each element.

Here are some tags we'll encounter with frequency.

* `<head>`
* `<title>`
* `<metadata>`
* `<body>`
* `<div>`*
* `<span>`*
* `<a href="">`
* `<iframe>`
* `<img src="" alt="">`
* `<ul>`
* `<ol>`
* `<li>`
* `<table>`
* `<tr>`
* `<th>`
* `<td>`
* `<style>`
* `<script>`

Unfortunately, some contents of a web page might be only generated through Javascript. This means it can sometimes present itself as a roadblock when scraping... but there's a way around it.

### Character Encodings

We deal with character encodings standards. One of the first standards was ASCII, but it was inadequate to represent most symbols and other languages. Today the most popular standard is Unicode, but still memory could be wasted due to unused symbols. That's why we use UTF-8. Here's the thing about encodings: you gotta be mindful about it because some documents are used with some specific encodings and it affects our readability of the document. We should be able to change encodings so our tools can "read" the symbols in adequate manner.

# Beautiful Soup

So now we know what web scraping is: it's the process of automatic data extraction from websites. How does it do that? By obtaining the source code of a page and processing it to get the different elements. **Beautiful Soup is a Python library for data extraction for an HTML document**. It's one of the simplest and easiest tools to do this.

Here's the documentation for [Beautiful Soup](https://www.crummy.com/software/BeautifulSoup/bs4/doc/).

So, what's the workflow of Web Scraping? What are the steps to extract data from the web? We've got five steps. Besides the first step, all the rest is done by creating a Python code.

1. Inspect the page
2. Obtain the HTML
3. Choose a parser
4. Create a Beautiful Soup object
5. (Optional) Export the HTML to a file

### 1. Inspect the Page

This is the number one step. By using the browser's developer's tools, we gotta inspect the web page in order to know **where** our data is located in the HTML. Remember the tags and attributes? To which tags and attributes our data is related to? This is why we inspect the web page.

But be careful.

The browser does a lot of things for us, users. Sometimes, it receives the contents from the server and the developer's tools automatically runs the Javascript code (if there's any) which, in turn, presents us with something different from the "original HTML" content sent from the server. When we analyse things, we're dealing the raw response from the server, which means no Javascript will be run automatically. That's why the difference.

To **inspect the page is to grab the general idea of how the page is structured**, which is essential to extract the data we want.

### 2. Obtain the HTML

This is similar to the calls to APIs in file `01_WORKING_WITH_APIs.md` in this repository. This means we'll need to send a `request` to the server using the **requests library**. 

Check below.

```python
import requests                         # This is the requests library

url = 'https://www.google.com'          # Just an example

# Now we send a GET request to the server located at the URL
# and it will give us a response, which we store in the "response" variable
response = requests.get(url)            

# We used the GET method because we're not trying to log into anything
# If we were, we should use the POST method... it alters the state.
# But GET method does not, it only gets something from the server...

# There's lots of information inside that "response" variable
print(response.status_code)         # Returns 200 if everything's ok
print(response.ok)                  # Returns a boolean
print(response.content)             # It prints the document HTML

# But manipulating the entirety of the HTML in one single string is...
# not fun at all. That's why parsing it is required: it splits the 
# string into syntactical components. This means it will identify
# all HTML components, their relationships to one another, their
# attributes and contents. 

# And then something great happens: a parse tree is created.

# That's why we parse the response.content
```

### 3. Choose a Parser

You can think of the parse tree as an inverted family tree of HTML elements. Tree as a data structure tree. 

Who creates these trees?

The parser. It is a program designed to parse, to create the parse tree. The Beautiful Soup does not parse the code, it actually uses an external parser... and we have to indicate which parser to use. There are three: `html.parser; lxml; html5lib`.

Here's how Beautiful Soup views each of these parsers: it ranks the html.parser as the worst, even though it is a built-in Python parser. It's considered the worst because it sometimes does not know how to handle errors. The best one is "lxml", which is the fastest and deals with errors. The middle one is "html5lib", even though it is the slowest - it can handle errors and it parsers the HTML the same way a browser does.

In case you don't choose a parser, Beautiful Soup will choose one for you. But this isn't cool - it's better that you always explicitly choose the parser. 

### 4. Create a Beautiful Soup object to manipulate and extract data

With the HTML in hands and the parser tree, a Beautiful Soup object can be created. This object will manipulate and extract the data we want inside that HTML document.

```python
import requests
from bs4 import BeautifulSoup           # Import Beautiful Soup

url = 'https://www.google.com'
response = requests.get(url)

# We created the object "soup", a Beautiful Soup object
# First argument: the content of the response in bytes
# Second argument: the parser of our choice
soup = BeautifulSoup(response.content, 'lxml')

# We could have used other parsers:
soup2 = BeautifulSoup(response.content, 'html.parser')

# Or even:
soup3 = BeautifulSoup(response.content, 'html5lib')

# PS: Instead of response.content, we could've used
# response.text; the difference is that the first one is in bytes, 
# this second one is in plain text. For example:
soup4 = BeautifulSoup(response.text, 'lxml')

# Whenever in doubt: print things so you can "see" what they are
# For example, print any of these soup objects
print(soup)             # It returns the whole HTML document, its attributes
                        # and values
```

### 5. (Optional) Export the HTML to a file (highly recommended)

Even though it is optional, might be a good idea because:

a) The code in the browser may be different from the one sent to us. Remember the browser does a lot of automatic things to present us with a visual page.

b) The parser... may have not parsed it correctly. This is a chance for you to "see" it.

c) Future reference.

All of the above are good reasons. When we have a bug, to inspect the exported file might be a good option to debug the problem. It could save tons of time.

## Searching and Navigating the HTML Tree

It would be awesome if at this point you'd check the Beautiful Soup documentation.

Okay, by now we have created the Beautiful Soup object. We stored it inside the "soup" variable, it became our object. As said before, it contains the whole HTML and more: its attributes, values and more. Its format is the HTML **tree**. And we can navigate this tree in search for things.

### Through Methods: `.find()` and `.find_all()`

The two **most used methods** (methods are an object's functions) of a Beautiful Soup object are `.find()` and `.find_all()`. How do we use it? **The simplest way is to search for an element by its tag name**, so that is what goes inside parenthesis. 

The difference between between these two is: `.find()` searches for the element and it stops right when it finds the first result; if the tag name is non-existent in the document, it returns `None`. Meanwhile `.find_all()` creates a list and stores inside that list every element it finds that matches the tag. If the tag name is non-existent in the document, it returns an empty list. 

Also, instead of searching the whole document, we can "cut it" into portions so we can search only inside a portion of the page like, say, the table body.

```python
import requests
from bs4 import BeautifulSoup

url = 'https://www.wikipedia.org'
response = requests.get(url)

soup = BeautifulSoup(response.text, 'lxml')
# The above 'soup' variable represents the whole document, including knowledge
# about elements and their attributes (so it's not just the text)

anchor = soup.find('a')
anchors = soup.find_all('a')

# Let's create an object just of a portion of the page, like a table
# (Remember tbody tag is the body of the table)

table = soup.find('tbody')      
type(table)     # Returns 'bs4.element.Tag'
# The above tag type is treated in almost the same way as the 'soup' variable

# Children tag
print(table.contents)
# The above line shows us all the children elements of the object table 
# (these children are in a list)

# Parent tag
# The parent of a tag is the element in which this tag is placed into
print(table.parent)

# PS: a tag can have MANY children but only ONE parent, that's why 
# the parent is a tag but the children are a list
```

### Through attributes and values

Now you know how to navigate by using the previous methods (find and find_all). Now, we'll expand our knowledge by showing **how to search for tags containing specific attribute values**.

For example purposes, we'll come up with random tags.

We can search by the tag attributes and values names... or we can also place the attributes inside a Python dictionary and search for that dictionary.

```python
import requests
from bs4 import BeautifulSoup

url = 'https://www.wikipedia.org'
response = requests.get(url)
soup = BeautifulSoup(response.text, 'lxml')

# Suppose we want to find this tag: <div id="siteSub"> ... </div>
# We'll add a second argument to our find method. Check below.

soup.find('div', id='siteSub')

# In theory, we used "find" (instead of find_all) because there can
# be only one Id with that specific value. This is good practice and
# generally done this way.

# Now, suppose we want to find all links inside a certain class
# attribute. Like this: <a class="mw-jump-link"> ... </a>

soup.find_all('a', class='mw-jump-link') # --> ERROR

# "class" is a Python reserved word. Instead, we'll use "class_"

soup.find_all('a', class_='mw-jump-link') # --> FINE :)
# It returns a list with both anchor tags for the mentioned class:
# [<a class="mw-jump-link" href="#mw-head">Jumo to navigation</a>,
# <a class="mw-jump-link" href="#p-search">Jump to search</a>]

# Check out that list: we can include the 'href' value of the second
# link to our search. They each have different "href" values, so this
# attribute is unique to each item.

soup.find('a', class_'mw-jump-link', href='#p-search')
# Returns: <a class="mw-jump-link" href="#p-search">Jump to search</a>

# Placing the attributes in a DICT():
# keys --> name of the attributes
# values --> name of the attributes values
# {"attribute": "value", "keys": "values", ...}

soup.find('a', attrs={'class': 'mw-jump-link', 'href': '#p-search'})
# Returns: <a class="mw-jump-link" href="#p-search">Jump to search</a>
# (And because here 'class' is a string... it does not need the underscore.)
```




_It continues..._