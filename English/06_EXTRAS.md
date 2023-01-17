# Writing to and Reading from CSV Files

These are my study notes from the course ["Web Scraping and Crawling with Python: Beautiful Soup, Requests & 
Selenium"](https://www.udemy.com/course/web-scraping-with-python-beautifulsoup/)
by instructor Waqar Ahmed.

First of all, a workbook is the same as opening a CSV file with all the tabs it includes (that is, if there's any). 
Second, a sheet is the same thing as a tab inside the workbook. You can have multiple sheets in the same workbook.

## xlsxwriter / writing

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

## xlrd / reading

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

# fake_useragent

```python
from fake_useragent import UserAgent
import requests

ua = UserAgent()

header = {'user_agent': ua.chrome}

page = requests.get('https://www.google.com', headers=header)
print(page.content)


```