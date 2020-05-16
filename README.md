# Documentation
Documentation Portfolio

# READ ME

# Title: Working with XML in Python

This mini-tutorial shows you how to:
  a. Import XML data
  b. Use a pattern to access the data within the XML tree structure
  c. Create a program that will allow the user to interact with the data
  
Core Concepts:
  a. Loading the data into objects
  b. Creating the main application that will read and return data
  c. Access data in XML

Example data:


## Overview:
After learning the core concepts of this tutorial, you will be to apply these concepts to any project using XML data. You'll be able to access that data within an XML tree structure, manipulate it, and also be able to create a program that interacts with the user/data by returning information within the XML data.

Pre-requisites:
  a. Python libraries:
    1. ElementTree
    2. collections
    3. os

## Part I: Importing the necessary libraries

### Step 1. Import the necessary libraries by declaring them at the top of your code.

Code snippet:
from xml.etree import ElementTree
import collections
import os

## Part II: Reading the data

We'll need to follow a common file manipulation pattern whereby we will create a folder object, a file object and then use the with open pattern to read the file. And since we're dealing with XML, we'll need an extra step to convert the xml to a string object.

### Step 1. Create a folder object.

Follow this pattern to assign a folder object:
folder = os.path.dirname(__file__)

This uses the OS library we imported to call the path method and dirname of the file we need.

### Step 2. Create the file ojbect.

The file object uses the OS library we imported to call the path and join method by passing the arguments for the folder path, the name of the subfolder, and then the name of the file.

file = os.path.join(folder, 'subfolder_data', 'filename.xml')

The join method takes the programmatic way to create a filepath in an OS-agnostic environment, so you don't have to worry about the differences between Mac and Windows file path conventions.

### Step 3. Read the file and create a new file object.

This is a common pattern to read a file and create a new file object at the same time.

    with open(file) as fin:
        xml_text = fin.read()

The "with open" method takes the file argument (which we've already created using the os path library) and we're going to reference it with a shorthand name called 'fin' instead of 'file'. We're going to create the 'xml_text' object by taking the file we've just renamed as 'fin' and call the "read()" method on it.

### Step 4. Create a new object to structure the data into a way we can query the XML structure and it's elements.

Basically, XML uses a nested structure that can be accessed by calling on the root level element and drilling down to it's child elements. But in order to do this, we need the ElementTree library, and specifically the "fromstring" method.

dom = ElementTree.fromstring(xml_text)

We'll create a new object called 'dom', and we'll call the ElementTree library and the fromstring method, and pass our data named 'xml_text'.

### Part II Summary 
- we created a folder, a file and a new file with the xml data loaded into. That's our 'xml_text' file. 
- now we take our 'xml_text' file and use the ElementTree library to make it query-able. We now have a new object called 'dom' and this is the one where we'll point to to extract the info we need.

## Part III: Manipulating the XML structure

What we want to do is create a scaffold to enable the user to query different elements within the XML tree. 

    course_nodes = dom.findall('course')

    courses = [
        Course(
            n.find('title').text,
            n.find('place/room').text,
            n.find('place/building').text
        )
        for n in course_nodes
    ]

### Step 1. Create a new 'root level object' that we're going to drill down into.

The root level object is something we're going to call the 'findall' method on. So to do that, we need to take our file, call findall and pass the 'course' variable as the parameter to the findall method. Since we haven't defined what a course is yet, we'll need to do that next.

    course_nodes = dom.findall('course')

### Step 2. Create a 'course' object which contains the information you need.

Since we have a nested structure where we need to drill down to two levels, we will need to call the 'find' method and 'text' to extract the info we need.

We're going to create an object that houses all of our data called 'courses.' 
'Courses' is a list that contains the structure of the data we need.

    courses = [
        Course(
            n.find('title').text,
            n.find('place/room').text,
            n.find('place/building').text
        )

Since we want to do this for every course in the file, we need to express this as:

        for n in course_nodes

Putting it all together, we get:

    course_nodes = dom.findall('course')

    courses = [
        Course(
            n.find('title').text,
            n.find('place/room').text,
            n.find('place/building').text
        )
        for n in course_nodes
    ]

## III. Input/Output to User Queries on XML Data

Now that we have the internal plubming in place, we can now create the I/O that will allow the user to query and get results.

### Step 1. Create two input objects.

One input is for 'building' and the other input is for 'room'. The program asks the user to enter the name of the building and the room number and will provide the course associated with the building/room combination.

    building = input("What building are you in? ")
    room = input("What room are you next to? ")

### Step 2. Create a new object to associate building and room together.

In order to associate these objects, we'll create a single object with a loop and some 'if' logic.

Room_courses is the new object that contains a list. For every object in 'courses' (a list we previously created), if the building name is the same as the one input by the user, and the room number is the same as the one input by the user.

We're going to loop through this for every object in 'room_courses', print out the title of the course.

    room_courses = [
        c.title
        for c in courses
        if c.building == building and c.room == room
        ]

    for c in room_courses:
        print("* " + c)

### Final Code

When we put everything together, the final code looks like this:

from xml.etree import ElementTree
import collections
import os

Course = collections.namedtuple('Course', 'title room building')

def main():
    folder = os.path.dirname(__file__)
    file = os.path.join(folder, 'xml_data', 'reed.xml')

    with open(file) as fin:
        xml_text = fin.read()

    dom = ElementTree.fromstring(xml_text)

    course_nodes = dom.findall('course')

    courses = [
        Course(
            n.find('title').text,
            n.find('place/room').text,
            n.find('place/building').text
        )
        for n in course_nodes
    ]

    building = input("What building are you in? ")
    room = input("What room are you next to? ")

    room_courses = [
        c.title
        for c in courses
        if c.building == building and c.room == room
        ]

    for c in room_courses:
        print("* " + c)

if __name__ == '__main__':
    main()








