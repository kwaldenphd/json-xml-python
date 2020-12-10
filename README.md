# Lab #3: Working with JSON and XML files in Python

<a href="http://creativecommons.org/licenses/by-nc/4.0/" rel="license"><img style="border-width: 0;" src="https://i.creativecommons.org/l/by-nc/4.0/88x31.png" alt="Creative Commons License" /></a>
This tutorial is licensed under a <a href="http://creativecommons.org/licenses/by-nc/4.0/" rel="license">Creative Commons Attribution-NonCommercial 4.0 International License</a>.

## Lab Goals

## Acknowledgements

Information and exercises in this lab are adapted from:
Al Sweigart, "Chapter 16, Working with CSV Files and JSON Data" in [*Automate the Boring Stuff With Python*](https://nostarch.com/automatestuff2) (No Starch Press, 2020): 371-388.

Wes McKinney, "Chapter 6.1, Reading and Writing Data in Text Format" in *[Python for Data Analysis*](https://www.oreilly.com/library/view/python-for-data/9781491957653/) (O'Reilly, 2017): 169-184.

Charles Severance, "Chapter 13, Using Web Services" in *[Python for Everybody*](https://www.py4e.com/book.php) (Charles Severance, 2009): 155-170.

# Table of Contents

# What is JSON and why are we learning about it

JavaScript Object Notation (JSON) is as popular way to format data as a single (purportedly human-readable) string. 

JavaScript programs use JSON data structures, but we can frequently encounter JSON data outside of a JavaScript environment.

Websites that make machine-readable data available via an application programming interface (API- more on these in an upcoming lab) often provide that data in a JSON format. Examples include Twitter, Wikipedia, Data.gov, etc. Most live data connections available via an API are provided in a JSON format.

JSON structure can vary WIDELY depending on the specific data provider, but this lab will cover some basic elements of working with JSON in Python.

The easiest way to think of JSON data as a plain-text data format made up of something like key-value pairs, like we've encountered previously in working with dictionaries.

Example JSON string: `stringOfJsonData = '{"name": Zophie", "isCat": true, "miceCaught": 0, "felineIQ": null}'`

From looking at the example string, we can see field names or keys (`name`, `isCat`, `miceCaught`, `felineIQ`) and values for those fields.

To use more precise terminology, JSON data has the following attributes:
- uses name/value pairs
- separates data using commas
- holds objects using curly braces `{}`
- holds arrays using square brackets `[]`

In our example `stringOfJsonData`, we have an object contained in curly braces. 

An object can include multiple name/value pairs. Multiple objects together can form an array.

Values stored in JSON format must be one of the following data types:
- string
- number
- object (JSON object)
- array
- boolean
- null

How is data stored in a JSON format different than a CSV? A `.csv` file uses characters as delimiters and has more of a tabular (table-like) structure.

JSON data uses characters as part of the syntax, but not in the same way as delimited data files. Additionally, the data stored in a JSON format has values that are attached to names (or keys).

JSON can also have a hierarchical or nested structure, in that objects can be stored or nested inside other objects as part of the same array.

For example, take a look at sapmle JSON data from Twitter's API:
```JSON
{
  "created_at": "Thu Apr 06 15:24:15 +0000 2017",
  "id_str": "850006245121695744",
  "text": "1\/ Today we\u2019re sharing our vision for the future of the Twitter API platform!\nhttps:\/\/t.co\/XweGngmxlP",
  "user": {
    "id": 2244994945,
    "name": "Twitter Dev",
    "screen_name": "TwitterDev",
    "location": "Internet",
    "url": "https:\/\/dev.twitter.com\/",
    "description": "Your official source for Twitter Platform news, updates & events. Need technical help? Visit https:\/\/twittercommunity.com\/ \u2328\ufe0f #TapIntoTwitter"
  },
  "place": {   
  },
  "entities": {
    "hashtags": [      
    ],
    "urls": [
      {
        "url": "https:\/\/t.co\/XweGngmxlP",
        "unwound": {
          "url": "https:\/\/cards.twitter.com\/cards\/18ce53wgo4h\/3xo1c",
          "title": "Building the Future of the Twitter API Platform"
        }
      }
    ],
    "user_mentions": [     
    ]
  }
}
```

<blockquote>Q1: Decipher what we're seeing in the JSON here. What are the name/value pairs, and how are they organized in this object?

# Reading JSON into Python

We can read JSON into Python using the `json` module.

<blockquote><a href="https://docs.python.org/3/library/json.html">Click here</a> to learn more about the <code>json</code> module.</blockquote>

The `json.loads()` and `json.dumps()` functions translate JSON data and Python values.

Translation table:
JSON | Python
--- | ---
object | dict
array | list
string | str
number (int) | int
number (real) | float
true | True
false | False
null | None

To translate a string of JSON data into a Python value, we pass it to the `json.loads()` function.
```Python
# import json module
import json

# string of JSON data
stringOfJsonData = '{"name": Zophie", "isCat": true, "miceCaught": 0, "felineIQ": null}'

# load JSON data as Python value 
jsonDataAsPythonValue = json.loads(stringOfJsonData)

# output JSON string as Python value
jsonDataAsPythonValue
```

This block of code imports the `json` module, calls the `loads()` function and passes a string of JSON data to the `loads()` function.

NOTE: JSON strings always use double quotes, which is rendered in Python as a dictionary. Because Python dictionaries are not ordered, the order of the Python dictionary may not match the original JSON string order.

# Working with JSON in Python

Now that the JSON data is stored as a dictionary in Python, we can interact with it via the functionality avaialble via Python dictionaries.

For more on working with dictionaries in Python:
- [Elements of Computing I lab](https://github.com/kwaldenphd/python-lab6/blob/master/README.md#working-with-dictionaries)
- [W3 Schools tutorial](https://www.w3schools.com/python/python_dictionaries.asp)

# Writing to JSON from Python



When would you want to write to JSON?

# What is XML and why are we learning about it

How is XML different CSV (and JSON)

# Reading XML into Python

# Working with XML in Python

# Writing to XML from Python

When would you want to write to XML

# Project prompts

# Lab Notebook Questions
