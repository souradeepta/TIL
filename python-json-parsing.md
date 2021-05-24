# Extract Nested Data From Complex JSON

We're all data people here, so you already know the scenario: it happens perhaps once a day, perhaps 5, or even more. There's an API you're working with, and it's great. It contains all the information you're looking for, but there's just one problem: the complexity of nested JSON objects is endless, and suddenly the job you love needs to be put on hold to painstakingly retrieve the data you actually want, and it's 5 levels deep in a nested JSON hell. Nobody feels like much of a "scientist" or an "engineer" when half their day becomes dealing with key value errors.

Luckily, we code in ***Python!\*** (okay fine, language doesn't make much of a difference here. It felt like a rallying call at the time).

## Using Google Maps API as an Example

To visualize the problem, let's take an example somebody might actually want to use.  I think the **Google Maps API** is a good candidate to fit the bill here.

While Google Maps is actually a collection of APIs, the [Google Maps Distance Matrix](https://developers.google.com/maps/documentation/distance-matrix/start). The idea is that with a single API call, a user can calculate the distance and time traveled between an origin and an infinite number of destinations. It's a great full-featured API, but as you might imagine the resulting JSON for calculating commute time between where you stand and *every location in the conceivable universe* makes an awfully complex JSON structure.

### Getting a Taste of JSON Hell

Real quick, here's an example of the types of parameters this request accepts:

```python
"""Fetch and extract JSON data from Google Maps."""
import requests
from config import API_KEY

def google_maps_distance():
    """Fetch distance between two points."""
    endpoint = "https://maps.googleapis.com/maps/api/distancematrix/json"
    params = {
       'units': 'imperial',
       'key': API_KEY,
       'origins': 'New York City, NY',
       'destinations': 'Philadelphia,PA',
       'transit_mode': 'car'
    }
    r = requests.get(endpoint, params=params)
    return r.json
```

main.py

One origin, one destination. The JSON response for a request this straightforward is quite simple:

```json
{
  "destination_addresses": [
    "Philadelphia, PA, USA"
  ],
  "origin_addresses": [
    "New York, NY, USA"
  ],
  "rows": [{
    "elements": [{
      "distance": {
        "text": "94.6 mi",
        "value": 152193
      },
      "duration": {
        "text": "1 hour 44 mins",
        "value": 6227
      },
      "status": "OK"
    }]
  }],
  "status": "OK"
}
```

Output of `google_maps_distance()`



For each destination, we're getting two data points: the *commute distance*, and *estimated duration*. If we hypothetically wanted to extract those values, typing `response['rows'][0]['elements']['distance']['test']` isn't *too* crazy. I mean, it's somewhat awful and brings on casual thoughts of suicide, but nothing out of the ordinary

Now let's make things interesting by adding a few more stops on our trip:

```python
...

def google_maps_distance():
    """Fetch distance between two points."""
    endpoint = "https://maps.googleapis.com/maps/api/distancematrix/json"
    params = {
       'units': 'imperial',
       'key': API_KEY,
       'origins': 'New York City, NY',
       'destinations': 'Washington,DC|Philadelphia,PA|Santa Barbara,CA|Miami,FL|Austin,TX|Napa County,CA',
       'transit_mode': 'car'
    }
    r = requests.get(endpoint, params=params)
    return r.json()
```

main.py



```json
{
  "destination_addresses": [
    "Washington, DC, USA",
    "Philadelphia, PA, USA",
    "Santa Barbara, CA, USA",
    "Miami, FL, USA",
    "Austin, TX, USA",
    "Napa County, CA, USA"
  ],
  "origin_addresses": [
    "New York, NY, USA"
  ],
  "rows": [{
    "elements": [{
        "distance": {
          "text": "227 mi",
          "value": 365468
        },
        "duration": {
          "text": "3 hours 54 mins",
          "value": 14064
        },
        "status": "OK"
      },
      {
        "distance": {
          "text": "94.6 mi",
          "value": 152193
        },
        "duration": {
          "text": "1 hour 44 mins",
          "value": 6227
        },
        "status": "OK"
      },
      {
        "distance": {
          "text": "2,878 mi",
          "value": 4632197
        },
        "duration": {
          "text": "1 day 18 hours",
          "value": 151772
        },
        "status": "OK"
      },
      {
        "distance": {
          "text": "1,286 mi",
          "value": 2069031
        },
        "duration": {
          "text": "18 hours 43 mins",
          "value": 67405
        },
        "status": "OK"
      },
      {
        "distance": {
          "text": "1,742 mi",
          "value": 2802972
        },
        "duration": {
          "text": "1 day 2 hours",
          "value": 93070
        },
        "status": "OK"
      },
      {
        "distance": {
          "text": "2,871 mi",
          "value": 4620514
        },
        "duration": {
          "text": "1 day 18 hours",
          "value": 152913
        },
        "status": "OK"
      }
    ]
  }],
  "status": "OK"
}
```

Output of `google_maps_distance()`



A lot is happening here. There are objects. There are lists. There are lists of objects which are part of an object. The last thing I'd want to deal with is trying to parse this data only to accidentally get a useless key:value pair like **"status": "OK".**



## Code Snippet To The Rescue

Let's say we only want the human-readable data from this JSON, which is labeled *"text"* for both distance and duration. We've created a function below dubbed `json_extract()` to help us resolve this very issue. The idea is that `json_extract()` is flexible and agnostic, therefore can be imported as a module into any project you might need.

```python
"""Extract nested values from a JSON tree."""


def json_extract(obj, key):
    """Recursively fetch values from nested JSON."""
    arr = []

    def extract(obj, arr, key):
        """Recursively search for values of key in JSON tree."""
        if isinstance(obj, dict):
            for k, v in obj.items():
                if isinstance(v, (dict, list)):
                    extract(v, arr, key)
                elif k == key:
                    arr.append(v)
        elif isinstance(obj, list):
            for item in obj:
                extract(item, arr, key)
        return arr

    values = extract(obj, arr, key)
    return values
```

extract.py



We need to pass this function two values:

- A complex Python dictionary, such as the response we parsed from `r.json()`.
- The name of the dictionary **key** containing values we want to extract.

```python
from extract import json_extract

# Find every instance of `name` in a Python dictionary.
names = json_extract(r.json(), 'name')
print(names)
```

Output of `json_extract()`



Regardless of where the key **"text"** lives in the JSON, this function returns every value for the instance of **"key."** Here's our function in action:

```python
...
from extract import json_extract


def google_maps_distance():
    """Fetch distance between two points."""
    endpoint = "https://maps.googleapis.com/maps/api/distancematrix/json"
    params = {
       'units': 'imperial',
       'key': API_KEY,
       'origins': "New York City,NY",
       'destinations': "Washington,DC|Philadelphia,PA|Santa Barbara,CA|Miami,FL|Austin,TX|Napa Valley,CA",
       'transit_mode': 'car',
    }
   r = requests.get(endpoint, params=params)
   travel_values = json_extract(r.json(), 'text')
   return travel_values
```

main.py

Running this function will result in the following output:

```python
['227 mi',
 '3 hours 54 mins',
 '94.6 mi',
 '1 hour 44 mins',
 '2,878 mi',
 '1 day 18 hours',
 '1,286 mi',
 '18 hours 43 mins',
 '1,742 mi',
 '1 day 2 hours',
 '2,871 mi',
 '1 day 18 hours'
 ]
```

Output of `google_maps_distance()`

Oh *fiddle me timbers*! Because the Google API alternates between **distance** and **trip duration**, every other value alternates between distance and time (can we pause to appreciate this awful design? There are infinitely better ways to structure this response). Never fear, some simple Python can help us split this list into two lists:

```python
my_values = json_extract(r.json(), 'text')

durations = my_values[1::2]  # Get every even-index value from a list
distances = my_values[2::1]  # Get every odd-index value from a list

print('Durations = ', durations)
print('Distances = ', distances)
```

Parse every other value.

This will take our one list and split it in to *two* lists, alternating between even and odd:

```python
Durations = [
    '3 hours 54 mins',
    '1 hour 44 mins',
    '1 day 18 hours',
    '18 hours 43 mins',
    '1 day 2 hours',
    '1 day 18 hours'
]
Distances = [
    '94.6 mi',
    '1 hour 44 mins',
    '2,878 mi',
    '1 day 18 hours',
    '1,286 mi',
    '18 hours 43 mins',
    '1,742 mi',
    '1 day 2 hours',
    '2,871 mi',
    '1 day 18 hours'
]
```

Output



## Getting Creative With Lists

A common theme I run in to while extracting lists of values from JSON objects like these is that the lists of values I extract are very much related.  In the above example, for every *duration* we have an accompanying *distance,* which is a one-to-one basis. Imagine if we wanted to associate these values somehow?

To use a better example, I recently I used our `json_extract()` function to fetch lists of column names and their data types from a database schema. As separate lists, the data looked something like this:

```python
column_names = ['index', 'first_name', 'last_name', 'join_date']
column_datatypes = ['integer', 'string', 'string', 'date']
```

Two related lists

Clearly these two lists are directly related; the latter is describing the former. How can this be useful? By using Python's `zip` method!

```python
schema_dict = dict(zip(column_names, column_datatypes))
print(schema_dict)
```

Zip two lists into a dictionary

I like to think they call it *zip* because it's like zipping up a zipper, where each side of the zipper is a list. This output a dictionary where list 1 serves as the keys, and list 2 serves as values:

```python
{'index': 'integer',
 'first_name': 'string',
 'last_name':'string',
 'join_date': 'date'
 }
```

Zipped lists resulting in a dictionary

And there you have it folks: a free code snippet to copy and secretly pretend you wrote forever. I've thrown the function up on [Github Gists](https://gist.github.com/toddbirchard/b6f86f03f6cf4fc9492ad4349ee7ff8b), if such a thing pleases you.

That's all for today folks! Zip it *up* and zip it *out*. Zippity-do-da, buh bye.

----------------



Hey guys, I modified the function to this and it appears to be working as intended. I also modified the results to return a one-dimensional list instead of possibly nested lists:

`def extract_values(obj, key):
"""Recursively pull values of specified key from nested JSON."""
arr = []

```python
def extract(obj, arr, key):
    """Return all matching values in an object."""
    if isinstance(obj, dict):
        for k, v in obj.items():
            if isinstance(v, (dict, list)):
                if k == key:
                    arr.append(v)
                extract(v, arr, key)
            elif k == key:
                arr.append(v)
    elif isinstance(obj, list):
        for item in obj:
            extract(item, arr, key)
    return arr

results = extract(obj, arr, key)
to_return = []
for result in results:
    if type(result) == list:
        for item in result:
            to_return.append(item)
    else:
        to_return.append(result)
return to_return`
```