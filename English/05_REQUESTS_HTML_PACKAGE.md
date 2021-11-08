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