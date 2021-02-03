# Working with JSON and XML files in Python

<a href="http://creativecommons.org/licenses/by-nc/4.0/" rel="license"><img style="border-width: 0;" src="https://i.creativecommons.org/l/by-nc/4.0/88x31.png" alt="Creative Commons License" /></a>
This tutorial is licensed under a <a href="http://creativecommons.org/licenses/by-nc/4.0/" rel="license">Creative Commons Attribution-NonCommercial 4.0 International License</a>.

## Lab Goals

This lab covers the basic components and structure for JSON and XML data files. This lab covers various methods for reading JSON and XML data into Python using `json` and `ElementTree`. The lab also overs writing data from Python to JSON or XML.

By the end of this lab, students will be able to:
- Describe the structure and components of JSON and XML  files
- Read a JSON file file into Python using the `json` module
- Understand how Python dictionaries work as a type of structured data
- Understand how to generate an XML structure from within Python
- Write data from Python to JSON and XML files

[Click here](https://raw.githubusercontent.com/kwaldenphd/json-xml-python/main/json-xml-python.ipynb) and select the "Save as" option to download this lab as a Jupyter Notebook.

## Acknowledgements

Information and exercises in this lab are adapted from:
- Al Sweigart, "Chapter 16, Working with CSV Files and JSON Data" in [*Automate the Boring Stuff With Python*](https://nostarch.com/automatestuff2) (No Starch Press, 2020): 371-388.
- Wes McKinney, "Chapter 6.1, Reading and Writing Data in Text Format" in [*Python for Data Analysis*](https://www.oreilly.com/library/view/python-for-data/9781491957653/) (O'Reilly, 2017): 169-184.
- Charles Severance, "Chapter 13, Using Web Services" in [*Python for Everybody*](https://www.py4e.com/book.php) (Charles Severance, 2009): 155-170.

The XML portions of this lab are adapted from the "Project 4: XML and XSLT" project materials developed by [Lindsay K. Mattock](http://lindsaymattock.net/) for the the [SLIS 5020 Computing Foundations course](http://lindsaymattock.net/computingfoundations.html). 

# Table of Contents

- [Data](#data)
- [JSON](#json)
  * [What is JSON and why are we learning about it](#what-is-json-and-why-are-we-learning-about-it)
  * [Reading JSON into Python](#reading-json-into-python)
  * [Working with JSON in Python](#working-with-json-in-python)
  * [Writing to JSON from Python](#writing-to-json-from-python)
  * [JSON Project Prompt](#json-project-prompt)
- [XML](#xml)
  * [What is XML and why are we learning about it](#what-is-xml-and-why-are-we-learning-about-it)
    * [XML Versus HTML](#xml-versus-html)
    * [XML Example 1](#xml-example-1)
    * [XML Example 2](#xml-example-2)
  * [Reading XML Into Python](#reading-xml-into-Python)
    * [Parsing XML in Python](#parsing-xml-in-python)
  * [Working With XML in Python](#working-with-xml-in-python)
  * [Writing to XML from Python](#writing-to-xml-from-python)
  * [XML Project Prompt](#xml-project-prompt)
- [Lab Notebook Questions](#lab-notebook-questions)

# Data

The only data needed for this lab is the `books.xml` file, which can be dowloaded from this GitHub repo.

[Link to Google Drive access (ND users only)](https://drive.google.com/drive/folders/1fa78Av2rELSuk2Nhj1DR8yErJeZQRNbN?usp=sharing)

# JSON

## What is JSON and why are we learning about it

1. JavaScript Object Notation (JSON) is as popular way to format data as a single (purportedly human-readable) string. 

2. JavaScript programs use JSON data structures, but we can frequently encounter JSON data outside of a JavaScript environment.

3. Websites that make machine-readable data available via an application programming interface (API- more on these in an upcoming lab) often provide that data in a JSON format. Examples include Twitter, Wikipedia, Data.gov, etc. Most live data connections available via an API are provided in a JSON format.

4. JSON structure can vary WIDELY depending on the specific data provider, but this lab will cover some basic elements of working with JSON in Python.

5. The easiest way to think of JSON data as a plain-text data format made up of something like key-value pairs, like we've encountered previously in working with dictionaries.

6. Example JSON string: `stringOfJsonData = '{"name": "Zophie", "isCat": true, "miceCaught": 0, "felineIQ": null}'`

7. From looking at the example string, we can see field names or keys (`name`, `isCat`, `miceCaught`, `felineIQ`) and values for those fields.

8. To use more precise terminology, JSON data has the following attributes:
- uses name/value pairs
- separates data using commas
- holds objects using curly braces `{}`
- holds arrays using square brackets `[]`

9. In our example `stringOfJsonData`, we have an object contained in curly braces. 

10. An object can include multiple name/value pairs. Multiple objects together can form an array.

11. Values stored in JSON format must be one of the following data types:
- string
- number
- object (JSON object)
- array
- boolean
- null

12. How is data stored in a JSON format different than a CSV? 

13. A `.csv` file uses characters as delimiters and has more of a tabular (table-like) structure.

14. JSON data uses characters as part of the syntax, but not in the same way as delimited data files. 

15. Additionally, the data stored in a JSON format has values that are attached to names (or keys).

16. JSON can also have a hierarchical or nested structure, in that objects can be stored or nested inside other objects as part of the same array.

17. For example, take a look at sapmle JSON data from Twitter's API:
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

<blockquote>Q1: Decipher what we're seeing in the JSON here. What are the name/value pairs, and how are they organized in this object?</blockquote>

## Reading JSON into Python

18. We can read JSON into Python using the `json` module.

<blockquote><a href="https://docs.python.org/3/library/json.html">Click here</a> to learn more about the <code>json</code> module.</blockquote>

19. The `json.loads()` and `json.dumps()` functions translate JSON data and Python values.

20. Translation table:
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

21. To translate a string of JSON data into a Python value, we pass it to the `json.loads()` function.
```Python
# import json module
import json

# string of JSON data
stringOfJsonData = '{"name": "Zophie", "isCat": true, "miceCaught": 0, "felineIQ": null}'

# load JSON data as Python value 
jsonDataAsPythonValue = json.loads(stringOfJsonData)

# output JSON string as Python value
jsonDataAsPythonValue
```

22. This block of code imports the `json` module, calls the `loads()` function and passes a string of JSON data to the `loads()` function.

23. NOTE: JSON strings always use double quotes, which is rendered in Python as a dictionary. Because Python dictionaries are not ordered, the order of the Python dictionary may not match the original JSON string order.

## Working with JSON in Python

24. Now that the JSON data is stored as a dictionary in Python, we can interact with it via the functionality avaialble via Python dictionaries.

25. We could get all of the keys in the dictionary using the `keys()` method.
```Python
# import json module
import json

# string of JSON data
stringOfJsonData = '{"name": "Zophie", "isCat": true, "miceCaught": 0, "felineIQ": null}'

# load JSON data as Python value 
jsonDataAsPythonValue = json.loads(stringOfJsonData)

# print list of keys
print(jsonDataAsPythonValue.keys())
```

26. We could get all of the values in the dictionary using the `values()` method.
```Python
# import json module
import json

# string of JSON data
stringOfJsonData = '{"name": "Zophie", "isCat": true, "miceCaught": 0, "felineIQ": null}'

# load JSON data as Python value 
jsonDataAsPythonValue = json.loads(stringOfJsonData)

# print list of values
print(jsonDataAsPythonValue.values())
```

27. We could iterate by keys over the items in the dictionary.
```Python
# import json module
import json

# string of JSON data
stringOfJsonData = '{"name": "Zophie", "isCat": true, "miceCaught": 0, "felineIQ": null}'

# load JSON data as Python value 
jsonDataAsPythonValue = json.loads(stringOfJsonData)

# iterate by keys using for loop
for key in jsonDataAsPythonValue.keys():
  print(key, jsonDataAsPythonValue[key])
```

28. We could also iterate over items in dictionary using key-value pairs.
```Python
# import json module
import json

# string of JSON data
stringOfJsonData = '{"name": "Zophie", "isCat": true, "miceCaught": 0, "felineIQ": null}'

# load JSON data as Python value 
jsonDataAsPythonValue = json.loads(stringOfJsonData)

# iterate by key value pairs using for loop
for key, value in jsonDataAsPythonValue.items():
  print(key, value)
```

29. We can read the value for a particular key using the index operator. The command `jsonDataAsPythonValue['name']` will return `Zophie`.

30. In situations where JSON data includes nested or hierarchical objects and arrays, we will end up with a list of dictionaries in Python.

31. For example, let's say we have a different JSON example and want to use more complex expressions in Python.
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

32. For more on working with dictionaries in Python:
- [Elements of Computing I lab](https://github.com/kwaldenphd/python-lab6/blob/master/README.md#working-with-dictionaries)
- [W3 Schools tutorial](https://www.w3schools.com/python/python_dictionaries.asp)

## Writing to JSON from Python

33. The `json.dumps()` function will translate a Python dictionary into a string of JSON-formatted data.
```Python
# import json module
import json

# Python dictionary
pythonValue = {'isCat': True, 'miceCaught': 0, 'name': 'Zophie', 'felineIQ': None}

# translate Python value to JSON string
stringOfJsonData = json.dumps(pythonValue)

stringOfJsonData
```

34. We can also write data in a Python dictionary to a JSON file also using `json.dump()`.
```Python
# import json module
import json

# Python dictionary
pythonValue = {'isCat': True, 'miceCaught': 0, 'name': 'Zophie', 'felineIQ': None}

# create new JSON file and write dictionary to file
with open('output.json', 'w') as json_file:
	json.dump(pythonValue, json_file)
```

35. Later in the semester we will talk about how to read JSON data into Python and convert it to a tabular data structure (called a data frame in Python), using a library called `pandas`. Stay tuned!

## JSON Project Prompt

36. Navigate to an open data portal and download a JSON file. 

37. Some options that can get you started:
- [Data.gov](https://www.data.gov/)
- [City of Chicago Data Portal](https://data.cityofchicago.org/)
- [City of South Bend Open Data](https://data-southbend.opendata.arcgis.com/)

38. Open the data in a spreadsheet program and/or text editor 

39. Describe what are you seeing. How can we start to make senes of this data? What documentation is available?

40. Read the JSON data into Python and convert to a Python value.

41. Create your own small dictionary with data and convert to JSON string.

# XML

## What is XML and why are we learning about it

42. Unlike HTML which allowed us to mark up and display information, XML is used for descriptive standards. 

43. For information professionals that work in places like libraries, XML is commonly associated with metadata--the descriptive information needed to describe information. That is, XML is used to encode metadata. 

44. Another example of digital work built on XML falls under the umbrella of the [Text Encoding Initiative](https://tei-c.org/), a group that’s been around since the 1970s and the early days of digital humanities. TEI includes a standardized set of tags that are used for marking or encoding various parts of a text.

45. Sample projects that use TEI:
- [Digital Archives and Pacific Culture](http://pacific.pitt.edu/InjuredIsland.html)
- [The Digital Temple](https://digitaltemple.rotunda.upress.virginia.edu/)
- [The Diary of Mary Martin](https://dh.tcd.ie/martindiary/)
- [Toyota City Imaging Project](http://www.bodley.ox.ac.uk/toyota/)
- [African American Women Writers](http://digital.nypl.org/schomburg/writers_aa19/)
- [The Walt Whitman Archive](https://whitmanarchive.org/)

46. XML is designed to store and transport data, it does not DO anything - XML is simply information that is wrapped in a set of tags. 

47. These tags can be user defined or from a standardized schema (like TEI). 

48. So, users of XML are free to develop their own set of tags or content standards in XML to describe whatever kind of information they would like.

49. The root element is the `parent` element to all of the other elements within an XML document. 

50. The elements are arranged hierarchically: `parent` elements have `child` elements, `child` elements have `sibling` or `subchild`’ elements. 

51. The indentation is used to indicate the hierarchical structure of an XML document. 

52. NOTE: XML tags are case sensitive. This matters when we are working in Python to isolate specific components or elements in XML data.

53. General XML structure:

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

### XML Versus HTML

54. According to W3C.....

<i>
XML and HTML were designed with different goals:
<ul>
	<li>XML was designed to carry data - with a focus on what data is</li>
	<li>HTML was designed to display data - with focus on how data looks</li>
	<li>XML tags are not predefined like HTML tags are</li>
	</ul>
</i>

Explanation from: http://www.w3schools.com/xml/xml_whatis.asp

### XML Example 1

55. We can create an XML document describing the information in this table.

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
  
56. Each piece of information is enclosed in a set of tags just like HTML, and each tag has an opening and closing tag. These are called elements. 

57 Unlike HTML, XML is only used to describe the data. It doesn’t provide instructions to the browser like HTML does in terms of formatting and display.

58. XML elements can be further defined with attributes. 

59. In the first example, I used the element <instructor> to describe my role in the course. In this second example, I’ve added the type “assistant teaching professor" to further describe the instructor tag.
  
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

60. So, why would we want to markup all of this information in XML? 

61. Well, imagine that we have a list of all of the different courses taught at Notre Dame. 

62. If we had all of this information marked up in XML, we could run queries against the data. 

63. For example, we could search for all of the courses taught by Jerod Weinman, or all of the courses taught by assistant professors, or find all of the courses taught on Mondays, etc. 

64. This is the power of encoding data in XML.

### XML Example 2

65. Let’s look at a more extensive example to illustrate the basic XML syntax. 

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

66. Here I’ve created a file describing books related to XML, HTML, and CSS.

<blockquote>Q2: Describe the structure of this XML document in your own words.</blockquote>

67. The root element of this document is `<books>`, followed by a series of `<book>` child elements that contain information about each of the individual books described in the document. 
  
68. Each `<book>` has an attribute `@category` that describes the subject of the book, a `<title>` (with an attribute `@language`), `<author>` (with child elements `<firstName>` and `<lastName>`), and a publication `<year>`.

<p align="center"><a href="https://github.com/kwaldenphd/json-xml-python/blob/main/Figure_1.jpg?raw=true"><img class="aligncenter" src="https://github.com/kwaldenphd/json-xml-python/blob/main/Figure_1.jpg?raw=true" /></a></p>

69. We can represent the structure generically in a graph, demonstrating the hierarchical structure of the XML document.

## Reading XML into Python

70. You might already be thinking about how we could interact with XML data in Python. 

71. XML elements have similarities to JSON's name-value pairs, and Python dictionary key-value pairs.

72. We could represent the second XML example in Python using distinct dictionaries.

74. One way to accomplish this goal with Python would be to generate two different dictionaries.

```Python
book_0={'title': 'CSS: The Definitive Guide', 'author': 'Eric Meyer', 'date': '2007'}
book_1={'title': 'Learning XML', 'author': 'Erik Ray', 'date': '2003'}
```

75. We could then use a list to generate a list of the metadata for each of the works.

```Python
book_0={'title': 'CSS: The Definitive Guide', 'author': 'Eric Meyer', 'date': '2007'}
book_1={'title': 'Learning XML', 'author': 'Erik Ray', 'date': '2003'}

books = [book_0, book_1]
print(books)
```

76. While the this program outputs all of the metadata, one of the advantages of our XML file is that all of the information was stored in a single place.

77. Another solution would be to embed a list in a dictionary.

```Python
books = {
  'title': ['CSS: The Definitive Guide', 'Learning XML'],
  'date': ['2007', '2003'],
  'author': ['Eric Meyer', 'Erik Ray']
  }

print ("My books include books by ", books['author'], ":")
for title in books['title']:
  print("\t" + title)
```

78. This program outputs:

```
My books include books by Eric Meyer and Erik Ray:
  CSS: The Definitive Guide
  Learning XML
```

<blockquote>Note: The <code>\t</code> is a short cut for a TAB so that my list was indented. <code>\n</code> will generate a new line in your output.</blockquote>

79. While this dictionary contains all of the data from my XML, there are some limitations. The dates are not specifically connected to the title that they are associated with. We can solve this problem by nesting a dictionary in a dictionary.

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

80. This example includes a dictionary called `books` that holds two other dictionaries. It uses the title as the key for each of the dictionaries for the works. The value of each of these keys is a dictionary containing “title”, “date”, and "author.”

81. The following program will output the titles of items in this collection as well as associated dates.

```Python
print("My Books: ")

for book, book_info in books.items():
  full_title = str(book) + " (" + str(book_info['date']) + ")"
  print("\t" + full_title)
```
82. This program prints the title first as a separate line. The `for` loop digs into the data for each of the books. 

83. `book` is the variable assigned to each of the keys in the `books` dictionary - in this example it is the titles of each of the books. 

84. `book_info` is the variable assigned to each of the values. `method .items()` works through each of the dictionaries. 

85. The next line, assigns a variable `full_title` that concatenates each work variable with an opening `(`, the date for each work called with `book_info[‘date’]`, and a closing `)`. 

86. The final line prints a tab `\t` before each concatenated string that we just created with the `full_title` variable.

87. This outputs:
```
My Books:
  CSS: The Definitive Guide (2007)
  Learning XML (2003)
```

<blockquote>Q3: Write a similar dictionary for the <code>book.xml</code> file contained in this repo and generate some output. Include code + comments. Explain how your program works in your own words.</blockquote>

### Parsing XML in Python

88. You may be thinking, “…but we already have an XML file with this data in it. Can we use Python to with .xml?” The answer to this question is YES! 

89. In this next task, we’ll use the [ElementTree API](https://docs.python.org/3/library/xml.etree.elementtree.html). This is part of the Python library that has been written specifically to parse XML. 

90. We’ll only work with a few functions and methods from ElementTree as an introduction to the tool.

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

91. First, we need to import the ElementTree module into our file so that we can continue to use the methods and functions associated with ElementTree. Type this first line of code into a new file called `XML.py` (or whatever you’d like to call it).

```Python
import xml.etree.cElementTree as ET
```

92. Now we need to import our .xml file. Remember, you’ll need to include the entire file path for your .xml file if it is not in the same folder as your python file.

```Python
import xml.etree.cElementTree as ET
tree = ET.parse('books.xml')
```
93. And, finally, we need to get the root element of our xml file. Remember in our last project we also had to name the root element in our XSL file. This tag that is at the top of the hierarchy. In the example file, the root is `<books>`.

```Python
import xml.etree.cElementTree as ET
tree = ET.parse('books.xml')
root = tree.getroot()
```

## Working with XML in Python

94. Now we are ready to work with our `.xml` file in Python. Try adding `print` commands.

```Python
import xml.etree.cElementTree as ET
tree = ET.parse('books.xml')
root = tree.getroot()

print(root.tag)
print(root.attrib)
```

95. This program returns the output:
```
books
{}
```

96. This set of commands returns the tag for the root element `books` and an empty set of braces for the attribute because there is not an attribute associated with the `books` tag. 

97. You probably didn’t assign attributes to your XML tags, so we’ll keep working with tags. If you have attributes in your file, you can consult the documentation for ElementTree to modify your code.

98. This code gave us the root element, but what if we want to see the structure of our document? We can use the `iter` function to pull all of the elements from our XML file in a simple loop that outputs each tag and the text value associated with it.

```Python
import xml.etree.cElementTree as ET
tree = ET.parse('books.xml')
root = tree.getroot()

getIterator = root.iter()

for element in getIterator:
  print(element.tag, element.text)
```

99. This program outputs:
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

100. Now let’s get some data from the file. This next program creates a loop that returns all the titles in the file. 

101. In the sample XML file, `<book>` tag is the child of `<books>` and `<title>` is a child of `<book>`. 

102. This loop says for each work, assign text from the `title` element to the variable `title`, and then print the titles.

```Python
import xml.etree.cElementTree as ET
tree = ET.parse('books.xml')
root = tree.getroot()

for book in root.findall('book'):
  title = book.find('title').text
  print(title)
```

103. This program outputs:
```
CSS: The Definitive Guide
Learning XML
```
104. But what if we want to pull the title and date? We can modify the code to also pull the information from the `<year>` tag.

```Python
import xml.etree.cElementTree as ET
tree = ET.parse('books.xml')
root = tree.getroot()

for book in root.findall('book'):
  title = book.find('title').text
  date = book.find('year').text
  
  print(title)
```

<blockquote>Q4: What do you expect this program to output? Why? Explain how this code works in your own words.</blockquote>

<blockquote>Q5: Write a similar program for the .xml file that you created in the last exercise. Pull data from at least two elements. Copy your code and your output in your notebook and explain what your code does (or is attempting to do).</blockquote>

## Writing to XML from Python

105. When working with JSON, `json.dumps()` made it relatively straightforward to transform a Python dictionary back into a JSON object.

106. Going from a Python dictionary to XML is less straightforward. In fact, you'd want to use the [`dicttoxml` module](https://pypi.org/project/dicttoxml/) specifically designed to help with that workflow.

107. For now, we're going to demonstrate how you would create the XML structure manually in Python using Element Tree and then write that to an XML file.

108. You create the root element using `ET.Element()`. 

109. Then you can create sub-elements (nested within the root element) using `ET.SubElement()`. 

110. The `SubElement()` function lets us specify parent elements, tags, and attribute: `SubElement(parent, tag, attrib={}, **extra)`

111. In this example, `parent` is the parent node for the sub-element. `attrib` is a dictionary with any element attributes. `extra` are any additional keyword arguments being passed to the `SubElement()` function.

112. Then we would use our standard file `open()` and `write()` operations to write the newly-created structure to an XML file.

113. To put this all together:
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
myFile = open('items.xml', 'wb')

# write XML string to file
myFile.write(myData)

#close file
myFile.close()
```

<blockquote>Q6: Create a small XML structure and write it to an XML file.</blockquote>

## XML Project Prompt

114. Navigate to an open data portal and download an XML file. 

115. A few places to start:
- [Data.gov](https://www.data.gov/)
- [City of Chicago Data Portal](https://data.cityofchicago.org/)
- [City of South Bend Open Data](https://data-southbend.opendata.arcgis.com/)
- [Library of Congress XML finding aids](https://findingaids.loc.gov/source/main)
- [National Library of Scotland](https://data.nls.uk/)
- [National Archives and Records Administration (NARA)](https://www.archives.gov/developer#toc--datasets)
- [Natural History Museum Data Portal](https://data.nhm.ac.uk/)
- Various TEI projects
  * [Digital Archives and Pacific Culture](http://pacific.pitt.edu/InjuredIsland.html)
  * [The Digital Temple](https://digitaltemple.rotunda.upress.virginia.edu/)
  * [The Diary of Mary Martin](https://dh.tcd.ie/martindiary/)  
  * [Toyota City Imaging Project](http://www.bodley.ox.ac.uk/toyota/)
  * [African American Women Writers](http://digital.nypl.org/schomburg/writers_aa19/)
  * [The Walt Whitman Archive](https://whitmanarchive.org/)
  * [Michigan State University, Feeding America: The Historic American Cookbook Dataset](https://lib.msu.edu/feedingamericadata/)

116. Open the data in a text editor. Describe what are you seeing. How can we start to make sense of this data? What documentation is available?

117. Read the XML data into Python and convert to a Python value.

# Lab Notebook Questions

Q1: Decipher what we're seeing in the JSON here. What are the name/value pairs, and how are they organized in this object?

Q2: Describe the structure of this XML document in your own words.

Q3: Write a similar dictionary for the <code>book.xml</code> file contained in this repo and generate some output. Include code + comments. Explain how your program works in your own words.

Q4: What do you expect this program out output? Why? Explain how this code works in your own words.

Q5: Write a similar program for the .xml file that you created in the last exercise. Pull data from at least two elements. Copy your code and your output in your notebook and explain what your code does (or is attempting to do).

Q6: Create a small XML structure and write it to an XML file.
