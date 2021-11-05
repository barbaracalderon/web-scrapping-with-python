# Tables and Pandas

It's time we learn how to store our data in structured form. For this, we'll use the **pandas** library, a very neat and visual way to structure tabula data. You can find the [pandas documentation here](https://pandas.pydata.org/docs/).

_The examples in this section are from the website Rotten Tomatoes, as shown in the course._

Suppose we have a list for each movie information: a list for the movies titles (`movie_names`); a list for the movies years (`years`); a list for the movies scores (`scores`); another for the movies adjusted scores (`final_adj`); a list for the movies directors (`final_directors`); a list for the movies synopsis (`synopsis_text`); a list for the movies casts (`cast`) and another list for the movies consensus (`consensus_text`).  

 We'll create a pandas DataFrame to store the information on these movies - this means 'populating' it. This, in turn, will present itself as a table. The lists will be "columns" and the "keys" are the table head.

```python
import pandas as pd
pd.set_option('display.max_colwidth', -1)

movies_info = pd.DataFrame()
movies_info["Movie Title"] = movies_names
movies_info["Year"] = years
movies_info["Score"] = scores
movies_info["Adjusted Score"] = final_adj
movies_info["Director"] = final_directors
movies_info["Synopsis"] = synopsis_text
movies_info["Cast"] = cast
movies_info["Consensus"] = consensus_text
```

_PS: pandas abreviate any text that does not fit into the screen. To make it not "truncate" it, we have to set an option: `pd.set_option('display.max_colwidth', -1)`_

Now, data analysis can be carried on. It is desirable to expor the data to CSV (_Comma Separated Values_) or Excel, so we have it as file. And pandas provide simple methods to convert dataframes into these formats: `.to_csv() ` and `.to_excel()`. They will create a file located in the same folder of your python code.

This is how it's done in our case. Check below.

```python
# Exporting the DataFrame to a CSV file:
movies_info.to_csv("movies_info.csv", index=False, header=True)

# Exporting the DataFrame to an Excel file:
movies_info.to_excel("movies_info.xlsx", index=False, header=True)
```

