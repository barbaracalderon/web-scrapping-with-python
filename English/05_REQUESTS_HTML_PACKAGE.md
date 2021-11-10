# Introducing the Requests-HTML Package

The combo "Beautiful Soup" + "Requests" we have been using so far can actually be inadequate for big projects of Javascript websites. There's another interesting library called "Requests-HTML" **created by the author of the Requests Library**. It was created with the puporse to replace and simplify the combo. And what's even more neat: **full Javascript support** - it can execute JS code and scrap the resulting page.

Here is the [Requests-HTML documentation](https://docs.python-requests.org/projects/requests-html/en/latest/).

Check the notes below.

```python
from requests_html import HTMLSession

session = HTMLSession()
r = session.get(url='https://pt.wikipedia.org/wiki/The_Football_Association')
r.status_code               # 200

parsed_object = r.html                      # Here is the parsed object

# Obtaining all the (relative) links from the page:
urls = r.htmls.links
type(urls)              # set

# Obtaining all the (absolute) links from the page:
full_path_urls = r.htmls.absolute_links
type(full_path_urls)    # set

# PS: Neither return a list, they both (above) return a 'set'

# Searching elements
links = r.html.find('a')      # Returns a list of elements (it behaves like the 'find_all()' method from bs4)
links[4]                      # <Element 'a' class=('mw-jump-link',) href='#mw-head'>
links[4].html                 # <a class="mw-jump-link" href="#mw-head">Jump to navigation</a>
links[4].text                 # 'Jump to navigation'
links[4].attrs                # {'class': ('mw-jump-link',), 'href': '#mw-head'}

# Searching for tags that containg a certain 'string'
# Find all tags that contain the string 'wikipedia':
r.html.find('a', containing='wikipedia')

# Find all text associated with tags that contain the string 'wikipedia',
# all stored inside a list:
list_of_tags = [tag.text for tag in r.html.find('a', containing='wikipedia')]

r.html.find('p', first=True)        # Similar to method 'find()' from bs4
```

## Searching for Text Based on a Pattern

If you need to find part of a text that is stuck between certain phrases, you can do that in Requests-HTML. That is a method called search that does just that.

As an example, let's try to find part of a text that is just after the word "known" and before the word "soccer". 

```python
# HTML.search
r.html.search("known{}soccer")[0]
# Curly brackets are saying "we are looking for a text that
# when substituted for them matches our string".
# To get the text itself, we choose the first element -> returns a valid string

# HTML.search_all
r.html.search_all("known{}soccer")[0]
# Returns a list of texts that matches our string.
# PS: Sometimes it may return some bits of tags, that's because
# the search and search_all parse the raw HTML
```

## CSS Selectors

This is the Cascading Style Sheets. The CSS selectors are just text that refer to one or more elements in the HTML document. 

```python
# 1) SELECT ELEMENTS BASED ON ID (#) 
# (They're case sensitive!)
r.html.find("#Name")    # -> Returns a list
r.html.find("#name")    # -> Returns a list
r.html.find("#Duration_and_tie-breaking_methods", first=True)   # -> Returns an element

# 2) SELECT ELEMENTS BASED UPON CLASS (.)
# (You can search for a class inside a class)
r.html.find(".mw-headline")                 # -> Returns a list
r.html.find(".mew-headline.plainlinks")     # -> Returns a list
# The immeadite above line finds a class (plainlinks) inside another
# class (mw-headline)

# 3) SELECTING BASED ON OTHER ATTRIBUTES
# A. [attribute]: selects all tags that have defined the attribute
# B. [attribute=value]: selects all tags with that particular value of the attribute
# C. [attribute*=value]: attribute contains the SUBSTRING 'value'


# A. 
r.html.find("[target]")                 # -> Returns a list

# B.
r.html.find("[role=note]")              # -> Returns a list

# C.


```