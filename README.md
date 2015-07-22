---
tags: datastore, database, python, appengine, crud
level: 3
languages: python, appengine
---

# App Engine Datastore Intro

## Objectives

+ Understand what Datastore does and why it is useful
+ Store, query, and delete data in Datastore from an AppEngine app
+ Use the Datastore viewer to see the contents of your Datastore
+ Create new data models to solve a problem

## Motivation / Why Should You Care?

The apps we've created so far have been kind of dumb. They can't remember anything between one request and a second request, unless we keep track with a query string or in the post request directly before. A *database* solves that problem by giving you a way to save information for later retrieval.

*Datastore* is a database provided as part of App Engine. We can use it in our App Engine apps to store data in one request and then retrieve it in another request.

## Lesson Plan

### Importing App Engine database

We’ll interact with App Engine using the ndb module (stands for new database). First, import the module.

```python
from google.appengine.ext import ndb
```

### Creating a Model

Before we actually store data, we need to figure out how to structure it. The structure of our data is called a model. This is like a giant spreadsheet. Each column of the spreadsheet is a property and each row in a new entity.

Now, let’s create a Student model together. A student will have a name, a university, and a birthday. Students don’t have to provide their birthday though. These parts of a student (name, university, birthday) are called properties. Notice that properties have a type. In this case, we are using strings (for name and university) and an integer (for the age). Properties can also be required or optional.

```python
class Student(ndb.Model):
  name = ndb.StringProperty(required=True)
  university = ndb.StringProperty(required=True)
  age = ndb.IntegerProperty(required=False)
```

### Creating an *Entity* from the model

```python
student = Student(name="Adina Wallis", university="U. Mich.")
student.put()
```

We create the Student *entity* on the first line and then we store it in the database with the method call to `put()`.

### Running the code with App Engine

```
dev_appserver.py --datastore_consistency_policy=consistent students
```

### Datastore viewer

App Engine conveniently provides a way to look at the actual data in the datastore.  This is like looking at the entire 'spreadsheet' of the database. All you need to do is navigate to `http://localhost:8000` in your browser.

### Datastore Keys

When we store something in the datastore using the `put()` method it returns a *key* which allows us to retrieve the stored entity later.

```python
key = student.put()
```

### Getting an entity with a key

To retrieve an entity with a key we use the `get()` method. (put...get... get it!)

```python
self.response.write(key.get().name)
```

### Using the key

You can build a Key object from a type and id using the Key constructor:

```python
key = ndb.Key('Student', id)
```

Now we can get that student from the database. We could also do this using the get_by_id(ID) function like so:

```python
student = Student.get_by_id(id)
```

If we need to use a key in a url, we can use the urlsafe() function:

```python
# Get urlsafe key and navigate to that url
self.redirect('/view_student?key=%s' % key.urlsafe())

# Get an entity from an ID
key = ndb.Key(urlsafe=self.request.get('key'))
student = key.get()
```

### Querying from the Datastore




## Conclusion / So What?


## Hints and Hurdles

+ Make sure you call `put()` when writing to the database or your *entity* won't get saved.
