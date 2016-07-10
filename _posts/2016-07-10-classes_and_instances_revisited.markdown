---
layout: post
title:  "Classes and Instances Revisited"
date:   2016-07-10 03:25:06 +0000
---

These blogs have become a way for me to review material in order to help me retain everything I've learned. As I am just finishing up the last few lessons in Ruby, these next couple blogs will be dedicated to going over a few of the key concepts I've covered.

> All you can do in Ruby is call methods on an object.

Classes are like object factories. They instaniate new object instances. We tell objects to do what we want by using dot notation. In dot notation, the object is followed by a period, and then a method which we call on that object.

Example:

`Person.new`

In the above code, the Person is the object/receiver, and new is the method/message.


**Class and Instance Methods:**

Instance methods are specific to instances of that class. They cannot be called without an instance.

Example:

```
def instance_method_name
  #some code
end
```

An initialize method is an instance method that allows us to create new instances. It is used to automatically assign certain attributes to those instances. The initialize method is called whenever you call .new on an object.

Example:

```
def initialize(name)
  @name = name
end
```

A class method is a method that is called on the class itself, not on individual instances of the class. It deals with behaviours that belong to the whole class.

Example:

```
def self.class_method_name
	#some code
end
```


**Class and Instance Variables:**

Instance variables can be called in any instance method in a particular class. An instance variable has an @ on the front of it which distinguishes it from a local variable, which cannot be called within another method.

Example:

`@instance_variable_name`


Instance variables are defined by making setter and getter methods.

A setter method takes in an argument and sets it equal to a variable.

A getter method will spit out the name when called.

Example:

```
class Person
	def name= (persons_name)	
		@persons_name = persons_name
	end

	def name	
		@persons_name
	end
end
```

In the above code, the first instance method is the setter method, and the second instance method is the getter method.


You can use macro to make your setter and getter methods for you.

```
attr_reader :name	
attr_writer :name
attr_accessor :name
```

The first line of code will make a getter/reader method for you.
The second line of code will make a setter/writer method for you.
The third line of code will make both!


Whereas instance variables can only be used within the instance methods of a class, class variables are accessible to the entire class. They store info that is relevant to the whole class, and not a specific instance. They have @@ in front of them to distinguish them from other types of variables.

Example:

`@@class_variable_name`


**Class Constants:**

Class constants are available to all instances in a class, whereas instance variables are specific to an individual instance of that class.

Class constants are name-spaced like so:

`Class::CONSTANT`

This tells us that the constant on the right side of the colon is defined within the class on the left.

All you can do in Ruby is call methods on an object. So if you think of our methods as behaviours and our variables as data, this helps us to write programs that reflect real-world objects and their interactions!

