
# Example: Currency API

```python
import requests

base_url = 'https://api.exchangeratesapi.io/latest'

response = requests.get(base_url)

print(response.ok)              # Returns a boolean
print(response.status_code)     # Returns 200 if ok


print(response.text)            # In text format
print(response.content)         # In bytes format

# But this API also provides a response in JSON format
jsonr = response.json()
print(type(jsonr))              # Returns "dict"
# Remember "dict" is a Python datastructure
```

It would be important to know how to handle json data. In order to make our lifes easier, let's import json, an in-built Python json package.

```python
import json

# Two most important JSON methods: loads and dumps
# - .loads(string): converts a JSON formatted string to a Python object
# - .dumps(obj): converts a Python object back to a string

# Presents data with indent of 4 spaces:
print(json.dumps(response.json(), indent=4))

# Tells you the keys to the JSON data:
response.json().keys()
# dict_keys(['rates', 'base', 'date'])
```

### Incorporating Parameters in the GET Request

Always read the API documentation to understand how to incorporate parameters to your request.

```python
param_url = base_url + '?symbols=USD,GBP'
response = requests.get(param_url)

data = response.json()      # This is a dictionary
data['base']                # 'EUR'
data['date']                # '2021-10-02'
data['rates']               # {'USD': 1.0914, 'GBP': 0.84058}

param_url = base_url + '?symbols=GBP' + '&' + 'base=USD'
data = requests.get(param_url).json()

usd_to_gbp = data['rates']['GBP']   # 0.7701850834
```

### Obtaining Historicall Exchange Rates

```python
historical_url = base_url + '/2016-01-26'
response = requests.get(historical_url)
data = response.json()
print(json.dumps(data, indent=4))       # The expected result
```

### Extracting data for a Time Period

```python
time_period = base_url + '/history' + '?start_at=2017-04-26&end_at=2018-04-26' + '&symbols=GBP'

data = requests.get(time_period).json()
print(json.dumps(data, indent=4), sort_keys=True)
# The sort_keys=True makes it into chronological order
```
