# Python JSON

[ref: https://pynative.com/python-json/]



[TOC]



## What is JSON

JSON (an acronym for JavaScript Object Notation) is a data-interchange format and is most commonly used for client-server communication. Refer [Official JSON documentation](https://www.json.org/json-en.html)

**Example**:

```json
{"name": "jane doe", "salary": 9000, "email": "JaneDoe@pynative.com"}
```

A JSON is an unordered collection of key and value pairs, resembling Python’s native dictionary.

- Keys are unique Strings that cannot be null.
- Values can be anything from a String, Boolean, Number, list, or even null.
- A JSON can be represented by a String enclosed within curly braces with keys and values separated by a colon, and pairs separated by a comma

Whenever the client needs information, it calls the server using a URI, and the server returns data to the client in the form of JSON. Later we can use this data in our application as per our requirement. Also, when the client application wants to store the data on the server. It can POST that data in the form of JSON.

JSON is most commonly used for client-server communication because:

- It is human readable.
- It’s both easy to read/write and
- JSON is language-independent.

### Python built-in module json

Python comes with a built-in module called json for working with JSON data. You only need to add `import json` at the start of your file and you are ready to use it.

![Python JSON Tutorial](https://pynative.com/wp-content/uploads/2020/01/python_json-1.jpg)Python JSON Tutorial



# Python JSON Encoding using json.dump and dumps(): Write JSON into file and String

- The `json.dump()` method (without “**s**” in “dump”) used to **write Python serialized object as JSON formatted data into a file**. The file type can be anything including text, JSON or even binary file.
- The `json.dumps()` method **encodes any Python object into JSON formatted String**.



## Mapping between JSON and Python entities while Encoding

To encode Python objects into JSON equivalent json module uses the following conversion table. The `json.dump()` and `json.dumps()` the method performs the translations when encoding.

| PYTHON                                | JSON   |
| ------------------------------------- | ------ |
| dict                                  | object |
| list, tuple                           | array  |
| str                                   | string |
| int, float, int & float-derived Enums | number |
| True                                  | true   |
| False                                 | false  |
| None                                  | null   |

## Understand json.dump() and json.dumps() method in detail

You can do many things using `json.dump()` and `json.dumps()` method. Let’s understand the different parameters of `json.dump()` to achieve different results.

**Syntax of json.dump()**

```python
json.dump(obj, fp, *, skipkeys=False, ensure_ascii=True, check_circular=True, allow_nan=True, cls=None, indent=None, separators=None, default=None, sort_keys=False, **kw)
```

*It is used to write a Python object into a file as a JSON formatted data.*

**Syntax of json.dumps()**

```python
json.dumps(obj, *, skipkeys=False, ensure_ascii=True, check_circular=True, allow_nan=True, cls=None, indent=None, separators=None, default=None, sort_keys=False, **kw)
```

*It is used to write a Python object into a JSON String.*

All other parameters are the same in both methods.

- **`obj`** is nothing but a Python serializable object which you want to convert into as a JSON format.
- A **`fp`** is a file pointer used to write JSON formatted data into a file. Python json module always produces string objects, not bytes objects, therefore, **`fp.write()`** must support string input.
- If **`skipkeys`** is true (default: False), then dict keys that are not of a basic type, (str, int, float, bool, None) will be skipped instead of raising a `TypeError`. For example, if one of your dictionary keys is a custom Python object, that key will be omitted while converting the dictionary into JSON.
- If **`ensure_ascii`** is true (the default), the output is guaranteed to have all incoming non-ASCII characters escaped. If `ensure_ascii` is false, these characters will be output as-is.
- **`allow_nan`** is True by default so their JavaScript equivalents (NaN, Infinity, -Infinity) will be used. If False it will be a ValueError to serialize out of range float values (nan, inf, -inf).
- An **`indent`** argument is used to pretty-print JSON to make it more readable. The default is `(', ', ': ')` . To get the most compact JSON representation, you should use `(',', ':')` to eliminate whitespace.
- If **`sort_keys`** is true (default: False), then the output of dictionaries will be sorted by key

## Convert Python Dict to JSON formatted String using json.dumps()

There are multiple scenarios where you need to use serialized JSON data in your program. If you need this serialized JSON data in your application of further processing then you can convert it to a native Python `str` object instead of writing it in a file.

For example, you receive an HTTP request to send developer detail. you fetched developer data from the database table and store it in a Python dictionary or any Python object, Now you need to send that data back to the requested application so you need to **convert Python dictionary object into a JSON** formatted string to send as a response in JSON string. To do this you need to use `json.dumps()`.

The **`json.dumps()` returns the JSON string representation of the Python dict.** Let see the example now.

Convert Python dictionary into a JSON formatted String

```python
import json

def SendJsonResponse(resultDict):
    print("Convert Python dictionary into JSON formatted String")
    developer_str = json.dumps(resultDict)
    print(developer_str)

# sample developer dict
developer_Dict = {
    "name": "Jane Doe",
    "salary": 9000,
    "skills": ["Python", "Machine Learning", "Web Development"],
    "email": "jane.doe@pynative.com"
}
SendJsonResponse(developer_Dict)
```

 Copy

**Output**:

```
Writing JSON data into a Python String
{"name": "Jane Doe", "salary": 9000, "skills": ["Python", "Machine Learning", "Web Development"], "email": "jane.doe@pynative.com"}
```

### Convert Python primitive types into JSON equivalent

In this section, we will see how to convert any Python primitive types such as a dictionary, list, set, tuple, string, numbers into a JSON formatted data. just to get to know the JSON equivalent of Python type, please refer to the following example.

Let see the example now.

```python
import json

sampleDict = {
    "colorList": ["Red", "Green", "Blue"],
    "carTuple": ("BMW", "Audi", "range rover"),
    "sampleString": "pynative.com",
    "sampleInteger": 457,
    "sampleFloat": 225.48,
    "booleantrue": True,
    "booleanfalse": False,
    "nonevalue": None
}
print("Converting Python primitive types into JSON")
resultJSON = json.dumps(sampleDict)
print("Done converting Python primitive types into JSON")
print(resultJSON)
```

 Copy

**Output**:

```
Converting Python primitive types into JSON
Done converting Python primitive types into JSON
{"colorList": ["Red", "Green", "Blue"], "carTuple": ["BMW", "Audi", "range rover"], "sampleString": "pynative.com", "sampleInteger": 457, "sampleFloat": 225.48, "booleantrue": true, "booleanfalse": false, "nonevalue": null}
```

## Encode and Write JSON data to a file using json.dump()

We can use it in the following cases.

- To write the JSON response in a file: Most of the time, when you execute a GET request, you receive a response in JSON format, and you can store JSON response in a file for future use or for an underlying system to use.
- For example, you have data in a list or dictionary or any Python object, and you want to encode and store it in a file in the form of JSON.

In this example, we are going to convert the Python dictionary into a JSON format and write it into a file.

```python
import json

# assume you have the following dictionary
developer = {
    "name": "jane doe",
    "salary": 9000,
    "email": "JaneDoe@pynative.com"
}
print("Started writing JSON data into a file")
with open("developer.json", "w") as write_file:
    json.dump(developer, write_file) # encode dict into JSON
print("Done writing JSON data into .json file")
```

 Copy

**Output**:

```
Started writing JSON data into a file
Done writing JSON data into developerDetail.json file
```

![File after writing JSON encoded data using Python](https://pynative.com/wp-content/uploads/2020/01/file_after_writing_json_encoded_data_using_python.jpg)File after writing JSON encoded data using Python

### Write Indented and pretty printed JSON data into a file using the dump method

If the user wants to read JSON file so it must be readable and well organized, so whosever consumes this will have a better understanding of a structure of a data. The dump() method provides the following arguments to pretty-print JSON data.

- The **indent** parameter specifies the spaces that are used at the beginning of a line.
- The **separator** argument of a json.dump method you can specify any separator between key and value.
- The **`sort_keys`** to sort JSON data by keys.

Let’s see how to write pretty-printed JSON data into a file.

```python
import json

developer = {
    "name": "jane doe",
    "salary": 9000,
    "skills": ["Raspberry pi", "Machine Learning", "Web Development"],
    "email": "JaneDoe@pynative.com"
}

with open("developerPrettyPrint.json", "w") as write_file:
    json.dump(developer, write_file, indent=4, separators=(", ", ": "), sort_keys=True)
print("Done writing pretty printed JSON data into a file")
```

 Copy

Output:

```
Done writing pretty printed JSON data into a file
```

![File after writing prettyprinted JSON data](https://pynative.com/wp-content/uploads/2020/01/file_after_writing_prettyprinted_json_data_using_python.jpg)File after writing prettyprinted JSON data