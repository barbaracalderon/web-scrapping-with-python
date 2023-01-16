# Application Programming Interface (API)

It specifies how a software component should interact. It's like a contract between a client and a server: if a client request a specific format, the server will always respond in a documented format or initiate a defined action.

We deal with web-based APIs.

They are extensively used to incorporate a third-party functionality to your service. APIs can be free or paid. All APIs should have some form of documentation.

You connect to an API through an HTTP request. First things, first.

"HTTP" is protocol. More specifically, it's the **HyperText Transfer Protocol**. It basically specifies **how** requests and responses (between client and server) are to be formatted and transmitted. This is how all the surface Internet works. Everything you see on the surface web is a response from a server, which is a computer machine responsible for storing files. Your computer is the client.

When your computer makes a request to that server. The server gives you a response: it sends all the files to your computer... which, in turn, downloads it temporarily and builds the webpage with the help of the browser. The browser, actually, connects all the files together in order to give the purposed visual representation of them. The web page.

The two most popular HTTP requests are **GET** and **POST**.

| GET                                      | POST                                         |
|------------------------------------------|----------------------------------------------|
| Obtain data from a server                | Alter state or send confidential information |
| Can be bookmarked                        | Parameters are added in a separate body      |
| Parameters are added directly to the URL | `r = requests.post(url, params=data)`        |
| _Not used for sensitive information_     | -                                            |

If everything is fine with your request, you should get a 200 status code. If things go wrong, you get the 404 status code error of page not found.

# JSON

The name comes from "JavaScript Object Notation" but nowadays it is language-independent. It's got nothing to do with Javascript anymore.

JSON is the standard format for data exchange in the web.

When the client sends an API request to the server, it responds by sending the data in the JSON format. It also works with POSTs requests - the response is in the JSON format.

The JSON format is built upon two fundamental programming structures: **dictionary** and **list**.

JSON files can grow into a chaos of data BUT, that's why the API documentation is important: you'll basically only get a glimpse of all the volume of information. That's because you'll be searching for very specific things, and you can rely on the documentation to find it.

# Pandas Library

At this point, it is very useful to get to know Pandas. This is a fast, powerful and flexible open-source library built on top of Python programming language that is good for data analysis and manipulation. In other words, Pandas deal very well with dictionaries and lists.

Here's an [introduction to Pandas](https://pandas.pydata.org/pandas-docs/stable/user_guide/10min.html).

The two most important objects in Pandas are **DataFrame** and **Series**. The DataFrame object can be seen as the table; and the Series object can be seen as a column.

# API Pagination

When you send a request to Google for a search, its result contains (sometimes) more than a million pages. Your computer will not download all million results at once. Instead, Google will place some results on page 1, then another bit on page 2, another bit on page 3, and so on... when you click the page, it loads the results of that page.

This is an important concept: the API pagination. It is the segmentation of the content into pages.

Let's use "The GitHub Jobs API" as an example on this matter.

```python
import requests
import json

base_site = 'https://jobs.github.com/positions.json'
info = {
    "description": "data science", 
    "location": "los angeles"
}
r = requests.get(base_site, params=info)

r.status_code   # 200
r.json()        # Returns a list with a dict in it
len(r.json())   # 4 results

# The search must've been too exclusive so it only got 4 results
# Let's fix this by removing the params we put

r = requests.get(base_site)
r.ok                # True
len(r.json())       # 50 (lots of results!)

# Now, let's search on the "next page", since this API 
# work with "pagination"

r = requests.get(base_site, params={"page": 2})
r.ok                # True
len(r.json())       # 50 (but on page 2!)

# If we search on a page that does not exist, most APIs will 
# return an empty list

# Let's extract results from MULTIPLE PAGES
results = []
for i in range(5):
    r = requests.get(base_site), params={"page": i+1})
    if len(r.json()) == 0:
        break
    else:
        results.extend(r.json())

len(results)        # 250
```

## EDAMAM API Example

Sign up at the EDAMAM API and get your ID and KEY of access. The following example is on this API.

```python
import requests
import json

APP_ID = "34539879384029U"                      # Just an example
ADD_KEY = "c984y39c3984539470jwiu392739"        # Just an example

api_endpoint = 'https://api.edamam.com/api/nutrition-details'
url = api_endpoint + '?app_id=' + APP_ID + '&app_key=' + APP_KEY

# The documentation states a few things, which we'll use
# If there's something you don't get, it's probably the API documentation

headers = {
    "Content-type": "application/json"
}

recipe = {
    "title": "Cappucino", 
    "ingr": ["18g ground espresso", "150ml milk"]
}

r = requests.post(url, headers=headers, json=recipe)
r.status_code       # 200

capp_info = r.json()
print(json.dumps(capp_info, indent=4))     # Returns a JSON
capp_info.keys()
print(json.dumps(capp_info["totalNutrients"], indent=4))    # Quantity info on different compounds

# Let's turn it into tabula form
import pandas as pd

pd.DataFrame(capp_info["totalNutrients"])
# The above line shows a table format, very neat to read... but very wide
# Let's fix it

# It shows in a more vertical table:
capp_nutrients = pd.DataFrame(capp_info["totalNutrients"]).transpose()

# Export to CSV file:
capp_nutrients.to_csv("Cappucino_nutrients.csv")
```