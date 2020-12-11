# Lab #3: Working with JSON and XML files in Python

<a href="http://creativecommons.org/licenses/by-nc/4.0/" rel="license"><img style="border-width: 0;" src="https://i.creativecommons.org/l/by-nc/4.0/88x31.png" alt="Creative Commons License" /></a>
This tutorial is licensed under a <a href="http://creativecommons.org/licenses/by-nc/4.0/" rel="license">Creative Commons Attribution-NonCommercial 4.0 International License</a>.

## Lab Goals

## Acknowledgements

Information and exercises in this lab are adapted from:
Al Sweigart, "Chapter 16, Working with CSV Files and JSON Data" in [*Automate the Boring Stuff With Python*](https://nostarch.com/automatestuff2) (No Starch Press, 2020): 371-388.

Wes McKinney, "Chapter 6.1, Reading and Writing Data in Text Format" in *[Python for Data Analysis*](https://www.oreilly.com/library/view/python-for-data/9781491957653/) (O'Reilly, 2017): 169-184.

Charles Severance, "Chapter 13, Using Web Services" in *[Python for Everybody*](https://www.py4e.com/book.php) (Charles Severance, 2009): 155-170.

The XML portions of this lab are adapted from the "Project 4: XML and XSLT" project materials developed by [Lindsay K. Mattock](http://lindsaymattock.net/) for the the [SLIS 5020 Computing Foundations course](http://lindsaymattock.net/computingfoundations.html). 

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

We could get all of the keys in the dictionary using the `keys()` method.
```Python
# import json module
import json

# string of JSON data
stringOfJsonData = '{"name": Zophie", "isCat": true, "miceCaught": 0, "felineIQ": null}'

# load JSON data as Python value 
jsonDataAsPythonValue = json.loads(stringOfJsonData)

# print list of keys
print jsonDataAsPython.keys()
```

We could get all of the values in the dictionary using the `values()` method.
```Python
# import json module
import json

# string of JSON data
stringOfJsonData = '{"name": Zophie", "isCat": true, "miceCaught": 0, "felineIQ": null}'

# load JSON data as Python value 
jsonDataAsPythonValue = json.loads(stringOfJsonData)

# print list of values
print jsonDataAsPython.values()
```

We could iterate by keys over the items in the dictionary.
```Python
# import json module
import json

# string of JSON data
stringOfJsonData = '{"name": Zophie", "isCat": true, "miceCaught": 0, "felineIQ": null}'

# load JSON data as Python value 
jsonDataAsPythonValue = json.loads(stringOfJsonData)

# iterate by keys using for loop
for key in jsonDataAsPython.keys():
  print key, jsonDataAsPython[key]
```

We could also iterate over items in dictionary using key-value pairs.
```Python
# import json module
import json

# string of JSON data
stringOfJsonData = '{"name": Zophie", "isCat": true, "miceCaught": 0, "felineIQ": null}'

# load JSON data as Python value 
jsonDataAsPythonValue = json.loads(stringOfJsonData)

# iterate by key value pairs using for loop
for key, value in jsonDataAsPythonValue.items():
  print key, value
```

We can read the value for a particular key using the index operator. The command `jsonDataAsPythonValue['name']` will return `Zophie`.

In situations where JSON data includes nested or hierarchical objects and arrays, we will end up with a list of dictionaries in Python.

For example, let's say we have a different JSON example and want to use more complex expressions in Python.
```Python
# import json module
import json

# new json data
data = '''
[
  { "id" : "001",
    "x" : "2",
    "name" : "Chuck"
  } ,
  { "id" : "009",
    "x" : "7",
    "name" : "Brent"
  }
]'''

#load data as json
info = json.loads(data)

# print number of users
print('User Count:', len(info))

# use for loop to print list of names, IDs, and attributes
for item in info:
  print('Name', item['name'])
  print('Id', item['id'])
  print('Attribute', item['x'])
```

For more on working with dictionaries in Python:
- [Elements of Computing I lab](https://github.com/kwaldenphd/python-lab6/blob/master/README.md#working-with-dictionaries)
- [W3 Schools tutorial](https://www.w3schools.com/python/python_dictionaries.asp)

# Writing to JSON from Python

The `json.dumps()` function will translate a Python dictionary into a string of JSON-formatted data.
```Python
# import json module
import json

# Python dictionary
pythonValue = {'isCat': True, 'miceCaught': 0, 'name': 'Zophie', 'felineIQ': None}

# translate Python value to JSON string
stringOfJsonData = json.dumps(pythonValue)

stringOfJsonData
```

Later in the semester we will talk about how to read JSON data into Python and convert it to a tabular data structure (called a data frame in Python), using a library called `pandas`. Stay tuned!

# JSON Project Prompts

Navigate to an open data portal and download a JSON file. OpenData.gov, city of Chicago, SB, SportsReference, other. Open the data in a spreadsheet program and/or text editor What are you seeing, how can we start to make sense of it What documentation is available, etc.

Read the JSON data into Python and convert to a Python value.

Create your own small dictionary with data and convert to JSON string.

# What is XML and why are we learning about it

Unlike HTML which allowed us to mark up and display information, XML is used for descriptive standards. 

For information professionals that work in places like libraries, XML is commonly associated with metadata--the descriptive information needed to describe information. That is, XML is used to encode metadata. 

Most library catalogues are built on some kind of XML.

Another example of digital work built on XML falls under the umbrella of the [Text Encoding Initiative](https://tei-c.org/), a group that’s been around since the 1970s and the early days of digital humanities. TEI includes a standardized set of tags that are used for marking or encoding various parts of a text.

Sample projects that use TEI:
- [Digital Archives and Pacific Culture](http://pacific.pitt.edu/InjuredIsland.html)
- [The Digital Temple](https://digitaltemple.rotunda.upress.virginia.edu/)
- [The Diary of Mary Martin](https://dh.tcd.ie/martindiary/)
- [Toyota City Imaging Project](http://www.bodley.ox.ac.uk/toyota/)
- [African American Women Writers](http://digital.nypl.org/schomburg/writers_aa19/)
- [The Walt Whitman Archive](https://whitmanarchive.org/)

XML is designed to store and transport data, it does not DO anything - XML is simply information that is wrapped in a set of tags. These tags can be user defined or from a standardized schema (like TEI). So, users of XML are free to develop their own set of tags or content standards in XML to describe whatever kind of information they would like.

28. The root element is the `parent` element to all of the other elements within an XML document. 

29. The elements are arranged hierarchically: `parent` elements have `child` elements, `child` elements have `sibling` or `subchild`’ elements. 

30. The indentation is used to indicate the hierarchical structure of an XML document. 

NOTE: XML tags are case sensitive. This matters when we are working in Python to isolate specific components or elements in XML data.

General XML structure:

```XML
<?xml version="1.0" encoding="UTF-8"?>
<root>
  <child>
    <subchild>xome text</subchild>
  </child>
  <child>
    <subchild>some more text</subchild>
  </child>
</root>
```

<blockquote>XML specification from W3C: http://www.w3.org/TR/REC-xml/</blockquote>

## XML Versus HTML

According to W3C.....

<i>
XML and HTML were designed with different goals:
<ul>
	<li>XML was designed to carry data - with a focus on what data is</li>
	<li>HTML was designed to display data - with focus on how data looks</li>
	<li>XML tags are not predefined like HTML tags are</li>
	</ul>
</i>

Explanation from: http://www.w3schools.com/xml/xml_whatis.asp

## XML Example 1

1. We can create an XML document describing the information in this table.

<table>
  <tr>
    <td>Course Title</td>
    <td>Course Schedule</td>
    <td>On-Campus Meeting Location</td>
    <td>Instructor</td>
    <td>Office Location</td>
    <td>Email</td>
    <td>Office Hours</td>
  </tr>
  <tr>
    <td>Elements of Computing II</td>
    <td>T/R 5:30-6:45 PM</td>
    <td>DeBartolo 102</td>
    <td>Dr. Katherine (Katie) Walden</td>
    <td>1046 Flanner</td>
    <td>kwalden@nd.edu</td>
    <td>T/R, 1-2 PM, 4-5 PM</td>
  </tr>
</table>
  
```XML
<course>
	<department>Computer Science and Engineering</department>
	<courseNumber>10102</courseNumber>
	<courseName>Elements of Computing II</courseName>
	<day>T/R</day>
	<startTime>5:30 PM</startTime>
	<endTime>6:45 PM</endTime>
	<room>102</room>
	<building>DeBartolo</building>
	<instructor>Katherine Walden</instructor>
	<officeRoom>1046</officeRoom>
	<officeBuilding>Flanner</officeBuilding>
	<email>kwalden@nd.edu</email>
	<officeHours>1-2 PM, 3-4 PM</officeHours>
</course>
```
  
2. Each piece of information is enclosed in a set of tags just like HTML, and each tag has an opening and closing tag. These are called elements. 

6. Unlike HTML, XML is only used to describe the data. It doesn’t provide instructions to the browser like HTML does in terms of formatting and display.

7. XML elements can be further defined with attributes. 

8. In the first example, I used the element <instructor> to describe my role in the course. In this second example, I’ve added the type “assistant teaching professor" to further describe the instructor tag.
  
```XML
<course>
	<department>Computer Science and Engineering</department>
	<courseNumber>10102</courseNumber>
	<courseName>Elements of Computing II</courseName>
	<day>T/R</day>
	<startTime>5:30 PM</startTime>
	<endTime>6:45 PM</endTime>
	<room>102</room>
	<building>DeBartolo</building>
	<instructor>Katherine Walden</instructor>
  <instructor type="assistant_teaching_professor">Katherine Walden</instructor>
	<officeRoom>1046</officeRoom>
	<officeBuilding>Flanner</officeBuilding>
	<email>kwalden@nd.edu</email>
	<officeHours>1-2 PM, 3-4 PM</officeHours>
</course>
```

15. So, why would we want to markup all of this information in XML? 

16. Well, imagine that we have a list of all of the different courses taught at Notre Dame. 

17. If we had all of this information marked up in XML, we could run queries against the data. 

18. For example, we could search for all of the courses taught by Jerod Weinman, or all of the courses taught by assistant professors, or find all of the courses taught on Mondays, etc. 

19. This is the power of encoding data in XML.


# XML Example 2

```XML
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE books [
<!ELEMENT books (book+)>
<!ELEMENT book (title,author+,year)>
<!ELEMENT title (#PCDATA)>
<!ATTLIST title type CDATA "lang">
<!ELEMENT author (firstName,lastName)>
<!ELEMENT firstName (#PCDATA)>
<!ELEMENT lastName (#PCDATA)>
<!ELEMENT heading (#PCDATA)>
<!ELEMENT body (#PCDATA)>
<!ELEMENT year (#PCDATA)>
<!ELEMENT signature (#PCDATA)>
<!ATTLIST book category CDATA #REQUIRED>
<!ATTLIST title lang CDATA #REQUIRED>
]>

<books>
  <book category="HTML">
    <title lang="en">HTML and XHTML: The Definitive Guide</title>
    <author>
      <firstName>Bill</firstName>
      <lastName>Kennedy</lastName>
    </author>
    <author>
      <firstName>Chuck</firstName>
      <lastName>Musciano</lastName>
    </author>
    <year>2006</year>
  </book>
  <book category="CSS">
    <title lang="en">CSS: The Definitive Guide</title>
    <author>
      <firstName>Eric</firstName>
      <lastName>Meyer</lastName>
    </author>
    <year>2007</year>
  </book>
  <book category="XML">
    <title lang="en">Learning XML</title>
    <author>
      <firstName>Erik</firstName>
      <lastName>Ray</lastName>
    </author>
    <year>2003</year>
  </book>
</books>
```
32. Let’s look at a more extensive example to illustrate the basic XML syntax. 

33. Here I’ve created a file describing books related to XML, HTML, and CSS.

<blockquote>Q1: Describe the structure of this XML document in your own words.</blockquote>

34. The root element of this document is <books>, followed by a series of <book> child elements that contain information about each of the individual books described in the document. 
  
35. Each `<book>` has an attribute `@category` that describes the subject of the book, a `<title>` (with an attribute `@language`), `<author>` (with child elements `<firstName>` and `<lastName>`), and a publication `<year>`.

<p align="center"><a href="https://github.com/kwaldenphd/XML/blob/master/images/Image_7.jpg?raw=true"><img class="aligncenter" src="https://github.com/kwaldenphd/XML/blob/master/images/Image_7.jpg?raw=true" /></a></p>

36. We can represent the structure generically in a graph, demonstrating the hierarchical structure of the XML document.

# Reading XML into Python

You might already be thinking about how we could interact with XML data in Python. 

XML elements have similarities to JSON's name-value pairs, and Python dictionary key-value pairs.

We could represent the second XML example in Python using distinct dictionaries.

C. One way to accomplish this goal with Python would be to generate two different dictionaries.

```Python
book_0={'title': 'CSS: The Definitive Guide', 'author': 'Eric Meyer', 'date': '2007'}
book_1={'title': 'Learning XML', 'author': 'Erik Ray', 'date': '2003'}
```

D. We could then use a list to generate a list of the metadata for each of the works.

```Python
book_0={'title': 'CSS: The Definitive Guide', 'author': 'Eric Meyer', 'date': '2007'}
book_1={'title': 'Learning XML', 'author': 'Erik Ray', 'date': '2003'}

books = [book_0, book_1]
print(books)
```

E. While the this program outputs all of the metadata, one of the advantages of our XML file is that all of the information was stored in a single place.

F. Another solution would be to embed a list in a dictionary.

```Python
books = {
  'title': ['CSS: The Definitive Guide', 'Learning XML']
  'date': ['2007', '2003']
  'author': ['Eric Meyer', 'Erik Ray']
  }

print ("My books include books by " + books['author'] + ":")
for title in books['title']:
  print("\t" + title)
```

G. This program outputs:

```
My books include books by Eric Meyer and Erik Ray:
  CSS: The Definitive Guide
  Learning XML
```

<blockquote>Note: The <code>\t</code> is a short cut for a TAB so that my list was indented. <code>\n</code> will generate a new line in your output.</blockquote>

H. While this dictionary contains all of the data from my XML, there are some limitations. The dates are not specifically connected to the title that they are associated with. We can solve this problem by nesting a dictionary in a dictionary.

```Python
books = {
  'CSS: The Definitive Guide': {
    'date': '2007', 
    'author': 'Eric Meyer'
    },
  'Learning XML': {
    'date': '2003',
    'author': 'Erik Ray'
    }
}
```

I. This example includes a dictionary called `books` that holds two other dictionaries. It uses the title as the key for each of the dictionaries for the works. The value of each of these keys is a dictionary containing “title”, “date”, and "author.”

J. The following program will output the titles of items in this collection as well as associated dates.

```Python
print("My Books: ")
for book, book_info in books.items():
  full_title = book + " (" + book_info['date'] + ")"
  print("\t" + full_title_title())
```
K. This program prints the title first as a separate line. The `for` loop digs into the data for each of the books. 

L. `book` is the variable assigned to each of the keys in the `books` dictionary - in this example it is the titles of each of the books. 

M. `book_info` is the variable assigned to each of the values. `method .items()` works through each of the dictionaries. 

N. The next line, assigns a variable `full_title` that concatenates each work variable with an opening `(`, the date for each work called with `book_info[‘date’]`, and a closing `)`. 

O. The final line prints a tab `\t` before each concatenated string that we just created with the `full_title` variable.

P. This outputs:
```
My Books:
  CSS: The Definitive Guide (2007)
  Learning XML (2003)
```

<blockquote>Write a similar dictionary for your XML file and generate some output. Copy and paste your code and your result into your notebook. Explain how your program works in your own words.</blockquote>

## Task 12: Parsing XML With Python

Q. You may be thinking, “…but we already have an XML file with this data in it. Can we use Python to with .xml?” The answer to this question is YES! 

R. In this next task, we’ll use the ElementTree API https://docs.python.org/3/library/xml.etree.elementtree.html. This is part of the Python library that has been written specifically to parse XML. 

S. We’ll only work with a few functions and methods from ElementTree as an introduction to the tool.

```XML
<?xml version="1.0" encoding="UTF-8"?>
<books>
  <book>
    <title>CSS: The Definitive Guide</title>
    <author>Eric Meyer</author>
    <year>2007</year>
  </book>
  <book>
    <title>Learning XML</title>
    <author>Erik Ray</author>
    <year>2003</year>
  </book>
</books>
```

T. First, we need to import the ElementTree module into our file so that we can continue to use the methods and functions associated with ElementTree. Type this first line of code into a new file called `XML.py` (or whatever you’d like to call it).

```Python
import xml.etree.cElementTree as ET
```

U. Now we need to import our .xml file. Remember, you’ll need to include the entire file path for your .xml file if it is not in the same folder as your python file.

```Python
import xml.etree.cElementTree as ET
tree = ET.parse('books.xml')
```
V. And, finally, we need to get the root element of our xml file. Remember in our last project we also had to name the root element in our XSL file. This tag that is at the top of the hierarchy. In the example file, the root is `<books>`.

```Python
import xml.etree.cElementTree as ET
tree = ET.parse('books.xml')
root = tree.getroot()
```

# Working with XML in Python

W. Now we are ready to work with our .xml file in Python. Try adding `print` commands.

```Python
import xml.etree.cElementTree as ET
tree = ET.parse('books.xml')
root = tree.getroot()

print(root.tag)
print(root.attrib)
```

X. This program returns the output:
```
books
{}
```

Y. This set of commands returns the tag for the root element `books` and an empty set of braces for the attribute because there is not an attribute associated with the `books` tag. 

Z. You probably didn’t assign attributes to your XML tags, so we’ll keep working with tags. If you have attributes in your file, you can consult the documentation for ElementTree to modify your code.

AA. This code gave us the root element, but what if we want to see the structure of our document? We can use the `iterator` function to pull all of the elements from our XML file in a simple loop that outputs each tag and the text value associated with it.

```Python
import xml.etree.cElementTree as ET
tree = ET.parse('books.xml')
root = tree.getroot()

iter = root.getiterator()

for element in iter:
  print element.tag, element.text
```

BB. This program outputs:
```
books

book

title CSS: The Definitive Guide
date 2007
author Eric Meyer
book

title Learning XML
date 2003
author Erik Ray
```

CC. Now let’s get some data from the file. This next program creates a loop that returns all the titles in the file. 

DD. In the sample XML file, `<book>` tag is the child of `<books>` and `<title>` is a child of `<book>`. 

EE. This loop says for each work, assign text from the `title` element to the variable `title`, and then print the titles.

```Python
import xml.etree.cElementTree as ET
tree = ET.parse('books.xml')
root = tree.getroot()

for book in root.findall('book'):
  title = book.find('title').text
  print(title)
```

FF. This program outputs:
```
CSS: The Definitive Guide
Learning XML
```
GG. But what if we want to pull the title and date? We can modify the code to also pull the information from the `<date>` tag.

```Python
import xml.etree.cElementTree as ET
tree = ET.parse('books.xml')
root = tree.getroot()

for book in root.findall('book'):
  title = book.find('title').text
  date = book.find('date').text
  
  print(title)
```

<blockquote>What do you expect this program out output? Why? Explain how this code works in your own words.</blockquote>

<blockquote>Write a similar program for the .xml file that you created in the last exercise. Pull data from at least two elements. Copy your code and your output in your notebook and explain what your code does (or is attempting to do).</blockquote>

# Writing to XML from Python

When working with JSON, `json.dumps()` made it relatively straightforward to transform a Python dictionary back into a JSON object.

Going from a Python dictionary to XML is less straightforward. In fact, you'd want to use the [`dicttoxml` module](https://pypi.org/project/dicttoxml/) specifically designed to help with that workflow.

For now, we're going to demonstrate how you would create the XML structure manually in Python using Element Tree and then write that to an XML file.

You create the root element using `ET.Element()`. 

Then you can create sub-elements (nested within the root element) using `ET.SubElement()`. 

The `SubElement()` function let's us specify parent elements, tags, and attribute: `SubElement(parent, tag, attrib={}, **extra)`

In this example, `parent` is the parent node for the sub-element. `attrib` is a dictionary with any element attributes. `extra` are any additional keyword arguments being passed to the `SubElement()` function.

Then we would use our standard file `open()` and `write()` operations to write the newly-created structure to an XML file.

To put this all together:
```Python
# import components of Element Tree package
import xml.etree.ElementTree as ET

# create root element
root = ET.Element('root')

# create sub elements
items = ET.SubElement(root, 'items')

# create child elements for items
item1 = ET.SubElement(items, 'item')
item2 = ET.SubElement(items, 'item')
item3 = ET.SubElement(items, 'item')

# set child element item names
item1.set('name', 'item1')
item2.set('name', 'item2')
item3.set('name', 'item3')

# set text for child item elmeents
item1.text = 'item1abc'
item2.text = 'item2abc'

# pass XML document to a string
myData = ET.tostring(root)

# create new XML file
myFile = open('items.xml', 'w')

# write XML string to file
myFile.write(myData)

#close file
myFile.close()
```

<blockquote>QX: Something with your own small XML structure from earlier in the lab and write that to file</blockquote>

# XML Project prompts

Navigate to an open data portal and download an XML file. OpenData.gov, , other. Open the data in a text editor What are you seeing, how can we start to make sense of it What documentation is available, etc.

https://findingaids.loc.gov/source/main
https://data.nhm.ac.uk/?_ga=2.163845238.855747161.1571830706-1425022049.1571830706
https://data.nls.uk/
https://lib.msu.edu/feedingamericadata/
https://www.archives.gov/developer#toc--datasets


Read the XML data into Python and convert to a Python value.

Create your own small XML structure and write to XML file.

# Lab Notebook Questions
