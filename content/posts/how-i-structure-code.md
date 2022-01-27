+++
title = "How I Structure Code"
date = 2018-07-14
draft = false

[taxonomies]
categories = ["Programming"]
tags = ["programming", "python"]
+++

This is an example of how I structure code in Python, but the rules I follow applies to any other language. This page may update as my styles change, or there's more information I can add.

# Module layout

```python
# I use the rules set in the default isort util.
# https://github.com/timothycrosley/isort
import standardlibrary

import thirdpartylibrary

from . import packagelibrary


def publicFunction():
    pass


class Class(object):
    def __magicmethod__(self):
        pass

    def publicMethod(self):
        pass

    def _privateMethod(self):
        pass

    def __protectedMethod(self):
        pass

    @classmethod
    def classMethod(cls):
        pass

    @staticmethod
    def staticMethod():
        pass


def _privateFunction():
    pass
```

The typical rule I follow is to put the public functions/classes/methods first because when someone looks at the code, they generally start with what's visible to the public. Also, I tend to put the class and static methods after the object methods because it helps keep things organized.

# Project layout

```
{projectName}/
  docs/
  python/
    {projectName}
  resources/
  src/
  tests/
  {projectMetadata}
```

I believe that the implementation of a project shouldn't live in the root directory of a project. Instead, project metadata such as the `README` or `setup.py` should live there. But, at the same time, if a language such as Go prefers to have everything in the root directory, then that's where it will live. Then, all Python code lives under the `python/{projectName}`. But, if this is a C++ or Rust project, then then code will live under `src/`. `docs/` contains the source for the document generator, while `tests/` is for the testing. Lastly, `resources/` is for anything that isn't code such as fonts or images.

Generally, the less items there are in the root directory, the better.

# Let your code breathe

Whitespaces aren't evil. However, too much whitespace isn't very helpful either. I tend to use whitespace around if statements and loops to help make the blocks easier to see. Also, using whitespace around code can help group logical pieces together is also very useful.

```python
def uniqueNamespace(name):
    # Set up some needed variables.
    number = 1
    namespacePattern = "{}_{{}}".format(name)

    # Transform that data into a namespace.
    namespace = namespacePattern.format(number)

    # Keep on transforming that namespace if it exists.
    while namespaceExists(namespace):
        number += 1
        namespace = namespacePattern.format(number)

    # Return it.
    return namespace
```

This function will be as readable if you take away the comments.

```python
def uniqueNamespace(name):
    number = 1
    namespacePattern = "{}_{{}}".format(name)

    namespace = namespacePattern.format(number)

    while namespaceExists(namespace):
        number += 1
        namespace = namespacePattern.format(number)

    return namespace
```

# The code should explain itself

This is a bit of a given, but here's an example of what I consider to be really bad code.

```python
from maya import cmds

print cmds.listAttr("myNode", l=1)
```

This is a valid function in Maya, and it's also something I see again and again in code. We can parse that this lists attributes for some node, but what does `l=1` mean? Is 1 representing an integer, enum, float, or bool? Is l listing?

Here's the same code, but it is now more self-explaining.

```python
from maya import cmds

print cmds.listAttr("myNode", locked=True)
```

I'd prefer isLocked, but anyways. Parsing this lets us guess that we're listing all the locked attributes for "myNode". The `locked=True` is a flag, and not anything else.

If someone can look at a function/class/method/arguments/etc and be mostly right about what the thing is, then you're doing a good job.

# Code should only do one thing

This is part of the don't repeat yourself principle, but it's also about preventing confusion. Once again, I'm going to pick on Maya.

Maya has a MEL command (wrapped by Python) called `file`. It handles nearly any and all operations with files. So, opening, saving, creating new files, referencing, importing, etc. It can also query data, remove references, and plenty of other things. The arguments you pass in can change the type of data you receive. This is a bad design. But, to make it more clear, here's an example of why it is bad.

```python
def nodeData(node, name=None, attribute=None, attributeValue=None):
    # Implementation of function
    ...
```

Here's a function that does everything with node data. Looking at the fields, we don't easily know if we're querying or setting, and if we're querying, what data we will get back, and what type. While Python does a very good job of not caring to a certain point, we can find ourselves with weird and expected bugs. For example, getting a list of strings when we expect a string. A better way to handle this would be to have class methods (or if you can't), then have a function for each task.

```python
def nodeName(node):
    return "nodeName"

def setNodeName(node, name):
    ...

def nodeAttribute(node, attribute):
    return ...

def setNodeAttribute(node, attribute, value):
    ...
```

While attribute is still a bit of a mystery in what it returns, at least we don't have to try to play a guessing game for most other functions.

# (Mostly) listen to your linters

While linters aren't perfect tools, they do catch quite a bit of style or actual code problems. While there may be some issues it brings up that are safe to ignore (such as line length), having a mostly clean code base sometimes does reduce errors. But, at the end of the day, use your brain. Sometimes what a linter suggests is a net negative for the readability of the code, and sometimes letting lines go on for too long will hurt you. As they say, write code assuming that the guy maintaining it is a serial murderer with a short temper that knows where you live.

# Keep like things together

It is usually a good idea to keep functions that are related together. For example, if there are functions that are triggered by signals or callbacks, keep those together, and keep UI updating methods together.

```python
class MyWidget(QtWidgets.QWidget):
    def __updateCanSubmit(self):
        pass

    def __updateButtonColor(self):
        pass

    def __onButtonClicked(self):
        pass

    def __onSignalEmitted(self):
        pass
```

# Make as little as public as possible

Most of the functions made are going to be used for the implementation. When creating code, start off with everything protected or private, and then promote things to public as needed. This way, there's less worries about API compatibility later.
