# Tables and Pandas

It's time we learn how to store our data in structured form. For this, we'll use the **pandas** library, a very neat and visual way to structure tabula data. You can find the [pandas documentation here](https://pandas.pydata.org/docs/).

_The examples in this section are from the website Rotten Tomatoes, as shown in the course._

Suppose we have a list for each movie information: a list for the movies titles (`movie_names`); a list for the movies years (`years`); a list for the movies scores (`scores`); another for the movies adjusted scores (`final_adj`); a list for the movies directors (`final_directors`); a list for the movies synopsis (`synopsis_text`); a list for the movies casts (`cast`) and another list for the movies consensus (`consensus_text`).  

 We'll create a pandas DataFrame to store the information on these movies - this means 'populating' it. This, in turn, will present itself as a table. The lists will be "columns" and the "keys" are the table head.

```python
import pandas as pd

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

Now, data analysis can be done. 

