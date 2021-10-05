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

_It continues..._