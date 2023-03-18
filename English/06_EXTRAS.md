# Extra Notes

These are my study notes from the course ["Web Scraping and Crawling with Python: Beautiful Soup, Requests & 
Selenium"](https://www.udemy.com/course/web-scraping-with-python-beautifulsoup/)
by instructor Waqar Ahmed.

## Writing to and Reading from CSV Files

First of all, a workbook is the same as opening a CSV file with all the tabs it includes (that is, if there's any). 
Second, a sheet is the same thing as a tab inside the workbook. You can have multiple sheets in the same workbook.

### Writing: xlsxwriter

This is the name of the module. You should import it and work with it. Like this:

```python
from xlsxwriter import Workbook

# The object itself
workbook = Workbook("first_file.xlsx")

# A sheet inside the workbook
worksheet = workbook.add_worksheet()

worksheet.write(0, 0, 'zero row and zero column')
worksheet.write(0, 1, 'zero row and one column')
worksheet.write(1, 0, 'one row and zero column')
worksheet.write(1, 1, 'one row and one column')

workbook.close()
```

If you wrote the above, and made the correct installment of xlsxwriter module, you should've been able to create a 
XLSX file and saved it with pieces written on it. 

But you can do a little bit more: you can play with loops in your favor. Like this:

```python
from xlsxwriter import Workbook

workbook = Workbook("second_file.xlsx")

for row in range(0, 20):
    worksheet.write(row, 0, "Row Number")
    worksheet.write(row, 1, row)

workbook.close()

# The above code should create an .xlsx file and the loop is responsible for writing on the first 20 rows.
# Each row contains the string "Row Number" on first column, and on second column the row number (int)
```

### Reading: xlrd

For the reading, we should use the module xlrd. It looks like below:

```python
import xlrd

workbook = xlrd.open_workbook("second_file.xlsx")

# Gets a particular sheet (first one) inside the workbook
worksheet = workbook.sheet_by_index(0)

 # It prints the number of written rows in the worksheet
print(worksheet.nrows) 

# It reads rows and its value (row, number)
for row in range(rows):
    first_column, second_column = worksheet.row_values(row)
    print(f'{first_column}    {second_column}')
```

## Seeting up a Fake User-Agent

### fake_useragent

User-agent is a software that acts on behalf of a user. To summarize it, we tell the server that a browser is doing 
the work on the server, not a piece of code.

```python
from fake_useragent import UserAgent
import requests

user_agent = UserAgent()

header = {'user_agent': user_agent.chrome}

# Timeout tells the code not to wait longer than timeout seconds for a response
page = requests.get('https://www.google.com', headers=header, timeout=6)
print(page.content)
```

# Regular Expressions (REGEX)

It's useful to match characters, finding tags or data through the parse tree (as in Beautiful Soup, for example).

The good effect is it is very fast and efficient, as long as you know what you are looking for and how to build your 
expression. The not so great effect is... the more complex your expression is, the more complex the string will 
become - which reduces readability.

I found this Corey Schafer tutorial video incredibly useful and easy to understand - [Corey Schafer - Regular 
Expressions (Regex) Tutorial: How to Match Any Pattern of Text.](https://www.youtube.com/watch?v=sa-TUpSx1JA) I'm a 
big fan of Mr. Schafer and his teaching talents, you should consider subscribing to his channel and taking some 
minutes digging gold there.

The [documentation for the regular expressions operations (re)](https://docs.python.org/3/library/re.html) can be found here.

This can be very useful if worked upon carefully. I suggest taking a look at Mr. Schafer videos.

# The cURL converter

*These notes are from working with the tools.*

Instead of working your way into gathering information on headers, cookies, data, json, params and more... in order to 
generate a request to a server, use the `curl` command into your favor. 

Here's how it works.

Let's assume we want to reach the example website of pudim.com.br (nothing big here, just a picture of a pudim). How 
do we translate this successful action into a Python code using cURL?

1. Open your browser and hit F12 to open the dev tools
2. Type the URL and hit Enter
3. Go to the Network tab, inside the dev tools, and search for the status code 200 and html document
4. Right click on it and `copy as cURL`
5. Go to the [cURL converter](https://curlconverter.com/python/) and check for Python
6. Paste what's in your clipboard
7. See the piece of Python code that reaches the expected response
8. Done

This is a beautiful shortcut.