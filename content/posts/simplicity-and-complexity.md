+++
title = "Simplicity and Complexity"
date = 2018-09-04
draft = false
+++

As far as I can tell, there's two ways to do software development. Simplicity through complexity and complexity through simplicity. But, before I go into which one I prefer, let's look at some examples which will hopefully explain my reasoning.

Let's pretend that we have the following object:

```python
class Context(object):
    def get(self):
        return self.__entity, self.__task

    def set(self, entity, task):
        pass
```

So, this doesn't tell us much. It tells us that we have a context object that can give us a tuple containing an entity and a task, and it can take an entity and probably store it internally. This is a good thing, because we don't need to know how it does it, but we only care about what it does. As far as we can tell, this is very simple.

Now, let's pretend that we need to initialize the context object. There's a few ways we can do this.

1. We can have an initializing function feed the context with the current context.
2. We can guess what the current context should be.

Both are valid(ish), but one is simpler. If we go with option 1, then the context object will remain super simple. You know that whenever you call get, you will always get a context you have previously set. However, if we go with option 2, then the context object will try to be smart. For example, we could have it so whenever the current file has changed in your application, the context object will automatically try to guess what the current context should be. Here's an example.

### Option 1:

```python
context = Context()
context.set(Entity(123), Task(456))

app.load_file("/path/to/file.ext")

print context.get()  # Returns (Entity(123), Task(456))
```

### Option 2:

```python
context = Context()
context.set(Entity(123), Task(456))

app.load_file("/path/to/file.ext")

print context.get()  # Returns (???, ???)
```

The problem with option 2 is we can't guarantee that the context we set is going to be the same context that we get. This, in my opinion is simplicity through complexity. In other words, it is a system that tries to provide a simple API that hides complex functionality that may or may not be problematic in the end. However, with complexity through simplicity, we compose simple code to create complex actions. This way, when something breaks, it should be relatively straight forward to debug.

Here's an example of complexity through simplicity in action.

```python
# appinit.py
# Let's pretend that a session is a singleton
session = init_session(app="app", user=User(987), entity=Entity(123), task=Task(456))
```

```python
# scenelib.py
class Scene(object):
    def __init__(self, session):
        self.__session = session

    def load(self, context, path):
        app.load_file(path)
        session.set_context(context)

    def save_as(self, context, path):
        app.save_file_as(path)
        session.set_context(context)
```

```python
# sceneui.py
class SceneDialog(QtWidgets.QDialog):
    def __init__(self, session, parent=None):
        super(SceneDialog, self).__init__(parent=parent)
        self.__session = session
        self.__scene = Scene(self.__session)

    def onLoad(self):
        context = self.__contextWidget.context()
        path = self.__pathWidget.path()

        self.__scene.load(context, path)

    def onSaveAs(self):
        context = self.__contextWidget.context()
        path = self.__pathWidget.path()

        self.__scene.load(context, path)
```

In this case, the scene UI is doing some complex stuff. It's saving or loading a scene based on the context and path provided by widgets in the UI. Also, the init_session is also fairly complex because it has to gather the proper information for a valid session. But after that, it provides simple objects that don't do anything magical in them. It allows the developer working with your library to be able to trust that as long as you provide a valid environment, you'll get expected results. Breaking that trust can really ruin a developer's day/week/month/year.

Anyways, that's the end of my ramblings for today. Hopefully it makes sense, and I'm not spouting nonsense. If I am, feel free to [open a ticket here and let me know](https://github.com/scott-wilson/propersquid.com/issues).
