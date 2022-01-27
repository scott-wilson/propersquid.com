+++
title = "Another Way to Think About Classes"
date = 2018-05-25
draft = false

[taxonomies]
categories = ["Programming"]
tags = ["programming", "python", "classes"]
+++

One of my friends is learning Python. Which I find exciting, because I am always interested in seeing people learn a skill I really enjoy, and also it is an opportunity for me to help them learn how to program.

There's an experiment that I have always wanted to give to a new Python developer to show them a way of thinking about classes without it being absolutely terrifying. For the sake of a thought experiment, let's imagine that Python didn't have classes, but we wanted to get something that worked in an object oriented way?

This post/lesson is assuming basic Python knowledge (functions, variables, lists, and dictionaries). If you aren't comfortable with the Python basics, then I recommend doing a quick search for beginner courses online. They'll probably teach classes, but may still leave you in a feeling of "What is this black magic?"

First off, what is this class, objects, object oriented thing? First off, object oriented is a way of thinking about data in terms of a thing. For example, you can have data that represents a person, or an animal. We'll continue this example assuming that we want to have code that represents a person. So, the `class` represents a way to construct a person. People have names, hair colours, toes, etc. They also can do certain actions like brush their teeth and watch sitcoms. An `object` represents a very specific person. The object could be someone like me or you. Classes and objects are really two things: data and functions that work with that data. Here's a really basic example:

```python
# This is using the Python 2.7 syntax because I'm use to the VFX world being stuck in 2.7 land.
my_name = "Scott Wilson"  # This is data

def first_name(full_name):  # This is a function that will operate on data.
    return full_name.split()[0]  # Split the name into Scott and Wilson, then get the first part, which is Scott.

print first_name(my_name)  # Prints "Scott"
```

This basic example takes a variable called `my_name` and runs it through the function `first_name` to get the first name from my full name. This is cool, but is very limiting. I want to work with an object that represents me, and not just my name.

```python
me = [
    "Scott",  # My first name
    "Wilson",  # My last name
    "Programmer",  # What I do for a living/fun
    "Human",  # What type of animal I am
]

def first_name(person):
    return person[0]


def last_name(person):
    return person[1]


def full_name(person):
    return "{} {}".format(person[0], person[1])


def career(person):
    return person[4]


def set_career(person, career):
    person[4] = career


def animal_type(person):
    return person[5]
```

Better. I now have an object that represents me, and not just my name. Also, I have added some functions that will work with the `me` "object". I have some functions that are usually referred to as `getters` that take the whole object, and returns a part of it. For example, the `first_name` function will get the first name from the object and return it. There's the `getter` sibling called the `setter`, which does the opposite. It takes data and puts it in the object.

However, it is not great. If I want to support another person, such as Dr Wily, then I have to make sure I create the list correctly. I need to make sure that the first value in the list is the first name, followed by last name, and so on. Here's the same code, but with a simple addition.

```python
def init_person(first_name, last_name, career):  # The simple addition
    return [first_name, last_name, career, "Human"]

def first_name(person):
    return person[0]


def last_name(person):
    return person[1]


def full_name(person):
    return "{} {}".format(person[0], person[1])


def career(person):
    return person[4]


def set_career(person, career):
    person[4] = career


def animal_type(person):
    return person[5]


me = init_person("Scott", "Wilson", "Programmer")  # Returns ["Scott", "Wilson", "Programmer", "Human"]
print full_name(me)  # Prints "Scott Wilson"
print career(me)  # Prints "Programmer"
set_career(me, "Wizard")  # Sets my career from Programmer to Wizard
print career(me)  # Prints "Wizard"
```

This is much better. I've made it really easy for anyone using my code to just create a person object and work with it. The person using my code doesn't have to manage all of the items in the list, because I have code that does that for them. However, it is now very difficult for me to work with. The reason is because I'm using a list, and lists are great when dealing with sequential information (like listing the chapters of a book). But, they're not so good when I want to work with something that can be thought as an object with attributes. This is where dictionaries come in handy! Here's the same code as before, but using a dictionary to hold my object info.

```python
def init_person(first_name, last_name, career):
    return {
        "first_name": first_name,
        "last_name": last_name,
        "career": career,
        "animal_type": "Human"
    }

def first_name(person):
    return person["first_name"]


def last_name(person):
    return person["last_name"]


def full_name(person):
    return "{} {}".format(person["first_name"], person["last_name"])


def career(person):
    return person["career"]


def set_career(person, career):
    person["career"] = career


def animal_type(person):
    return person["animal_type"]


me = init_person("Scott", "Wilson", "Programmer")  # Returns {"first_name": "Scott", "last_name": "Wilson", "career": "Programmer", "animal_type": "Human"}
print full_name(me)  # Prints "Scott Wilson"
print career(me)  # Prints "Programmer"
set_career(me, "Wizard")  # Sets my career from Programmer to Wizard
print career(me)  # Prints "Wizard"
```

This is much easier on the eyes. If I have a program that is thousands of lines long, it is now much easier for me to guess what data I'm working with in the functions. The developer using my code gets the same result either way, but the person that has to maintain my code will not be wanting me dead.

Also, if we convert this to a regular Python class, then we see a very similar pattern.

```python
class Person(object):
    def __init__(self, first_name, last_name, career):
        self.__first_name = first_name
        self.__last_name = last_name
        self.__career = career
        self.__animal_type = "Human"

        def first_name(self):
            return self.__first_name

        def last_name(self):
            return self.__last_name

        def full_name(self):
            return "{} {}".format(self.__first_name, self.__last_name)

        def career(self):
            return self.__career

        def set_career(self, career):
            self.__career = career

        def animal_type(self):
            return self.__animal_type


me = Person("Scott", "Wilson", "Programmer")  # Returns the person object that represents me.
print me.full_name()  # Prints "Scott Wilson"
print me.career()  # Prints "Programmer"
me.set_career(, "Wizard")  # Sets my career from Programmer to Wizard
print me.career()  # Prints "Wizard"
```

So, asides from the class syntax, what is different? Instead of using a dict, I'm storing the data in variables (called attributes) that belong to the object. Note that I used `__` before all of my variable names. This makes the attribute `protected`, so no `subclasses` of the person class can use the attribute directly. Basically, it is a way to make sure that if someone creates a programmer class that inherits from the person class, and happens to use the same attributes for something else, it won't clash and cause weird bugs. Also, instead of every function's first argument being `person`, it is `self`. `self` refers to the current object you're working with. And it is always the first argument in all functions (called `methods` for classes). You could call them something else like `tomato`, but any developer looking at your code will probably look at you odd.

Going back to my simple class, what if I want to subclass this? What if I think we really need a programmer class that inherits from a person class?

```python
# Person functions.
def init_person(first_name, last_name, career):
    return {
        "first_name": first_name,
        "last_name": last_name,
        "career": career,
        "animal_type": "Human"
    }

def first_name(person):
    return person["first_name"]


def last_name(person):
    return person["last_name"]


def full_name(person):
    return "{} {}".format(person["first_name"], person["last_name"])


def career(person):
    return person["career"]


def set_career(person, career):
    person["career"] = career


def animal_type(person):
    return person["animal_type"]


# Programmer functions
def init_programmer(first_name, last_name, languages):
    # Use the init_person so we can modify the object it returns to subclass it.
    person = init_person(first_name, last_name, "Programmer")
    person["languages"] = languages

    return person


def languages(programmer):
    return programmer["languages"]


me = init_programmer("Scott", "Wilson", ["Python", "Go", "C++", "Rust", "TypeScript"])  # Returns {"first_name": "Scott", "last_name": "Wilson", "career": "Programmer", "animal_type": "Human", "languages": ["Python", "Go", "C++", "Rust", "TypeScript"]}
print full_name(me)  # Prints "Scott Wilson"
print career(me)  # Prints "Programmer"
set_career(me, "Wizard")  # Sets my career from Programmer to Wizard. Note that this will cause some of the internal assumptions to be wrong.
print career(me)  # Prints "Wizard"
print languages(me)  # Prints ["Python", "Go", "C++", "Rust", "TypeScript"]
```

So this shows that I can use code that was written previously for a class that my code depends on. Also, if I want to make sure I have access to the data that the person class sets up, then I need to call that `init_person` function inside of my `init_programmer`. Otherwise, if I try to access the programmer's name, then it will error out because we don't know what it is. We could have reimplemented the `init_person` functionality, but life is too short for that. This is the same as using the `super` function inside of a class, but the difference is `super` will manage the order of methods to run. For example, if your class inheritance looks something like `Animal` -> `Mammal` -> `Person` -> `Programmer`, then the super will run the functions in that order. Here's an example:

```python
class Person(object):
    def __init__(self, first_name, last_name, career):
        self.__first_name = first_name
        self.__last_name = last_name
        self.__career = career
        self.__animal_type = "Human"


# Note who this inherits from
class Programmer(Person):
    def __init__(self, first_name, last_name, languages):
        super(Programmer, self).__init__(first_name, last_name, "Programmer")
        self.__languages = languages
```

Hopefully this makes sense. If you notice anything wrong, [feel free to open up an issue on my GitHub](https://github.com/scott-wilson/propersquid.com/issues), and I'll take a look. Until then, happy coding!
