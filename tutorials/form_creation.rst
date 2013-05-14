=========================================
Form creation tutorial
=========================================

In this tutorial we will create a simple registration form for the Knights of
the Round Table. We will learn about field types and about validation. It will
not cover the actual rendering of a form in a browser. For this tutorial to
work you need to install the package Anthrax only.

Simplest version with no custom validation.
----------------------------------------------------

First, let us define some fields.


.. testcode::

    from anthrax.container import Form
    from anthrax.field import TextField, IntegerField

    class RegistrationForm(Form):
        """The registration form for knights."""

        name = TextField()
        nickname = TextField()
        age = IntegerField()
        password = TextField()

The code above creates a :py:class:`~anthrax.container.Form` that contains four
fields. Three of them are text fields - the accept any text. The other is
integer field - it will accept only values that can be converted to ``int`` and
will return ``int`` as its python value.

.. testsetup::

    from pprint import pprint
    print = pprint
    from collections import OrderedDict

.. testcode::

    form = RegistrationForm()
    form.__raw__ = {
        'name': 'Sir Galahad',
        'nickname': 'The Pure',
        'age': '21',
        'password': 'guinevre123',
    }

    print(OrderedDict(form))

The above code will produce a dict (pretty-printed here)

.. testoutput::

    {'name': 'Sir Galahad',
     'nickname': 'The Pure',
     'age': 21,
     'password': 'guinevre123'}

Nothing mind-blowing happened here. Three of the four fields are
:py:class:`anthrax.field.text.TextField`,
so their raw value and python value are the same. The field ``age`` got
converted to integer. We can check the form has already some basic validation:

.. testcode::

    form = RegistrationForm()
    form.__raw__ = {
        'name': 'Sir Galahad',
        'nickname': 'The Pure',
        'age': 'twenty-one',
        'password': 'guinevre123',
    }

    print(form.__errors__)

Which will print

.. testoutput::

    {'age': <Valid integer required>}

This is the validation that is required to see if the value can be correctly
converted to a python object. However we also want to check a little more -
namely if:

* The names of the knights aren't too long for our database
* Only adults are allowed to enter the elite club of The Round Table
* The user didn't mistype his password
* Some dirty guy isn't trying to impersonate Sir Galahad

Using max_len
------------------------------------------

The first two tasks require validation limited to one field only. For the other
two however we need to compare several values from the form. Let's start with
the easier cases first.

All the fields in anthrax that accept strings as their *raw value* support
``max_len`` configuration option. Let's amend our form:

.. testcode:: 

    class RegistrationForm(Form):
        """The registration form for knights."""

        name = TextField(max_len=32)
        nickname = TextField()
        age = IntegerField()
        password = TextField()

    form = RegistrationForm()

    form.__raw__ = {
        'name': 'Sir Knightwhosenamerequirestypingalot',
        'nickname': 'The Pure',
        'age': '21',
        'password': 'guinevre123',
    }

    print(form.__errors__)

.. testoutput::

    {'name': <Value can't be longer than 32>}

This does our first task.

A little about field hierarchy
--------------------------------------------

The class hierarchy of the :py:class:`anthrax.field.base.Field` reflects the
essential properties of the types they can contain. It sports some abstract
classes that add some configuration types, but don't represent a usable field
by itself.

.. doctest::

    >>> from anthrax.field.ordered import OrderedField
    >>> issubclass(IntegerField, OrderedField)
    True
    >>> issubclass(TextField, OrderedField)
    False


The field :py:class:`anthrax.field.ordered.OrderedField` is a superclass for 
all the fields for which it makes sense to define ``min`` and ``max`` value.
Therefore we can now modify our form.

.. testcode:: 

    class RegistrationForm(Form):
        """The registration form for knights."""

        name = TextField(max_len=32)
        nickname = TextField()
        age = IntegerField(min=18)
        password = TextField()

    form = RegistrationForm()

    form.__raw__ = {
        'name': 'Sir Galahad',
        'nickname': 'The Pure',
        'age': '7',
        'password': 'guinevre123',
    }

    print(form.__errors__)

.. testoutput::

    {'age': <Value can't be lower than 18>}

If we want this message to be more informative we can specify our own.

.. testcode:: 

    class RegistrationForm(Form):
        """The registration form for knights."""

        name = TextField(max_len=32)
        nickname = TextField()
        age = IntegerField(min=18, min_message='You must be at least 18')
        password = TextField()

    form = RegistrationForm()

    form.__raw__ = {
        'name': 'Sir Galahad',
        'nickname': 'The Pure',
        'age': '7',
        'password': 'guinevre123',
    }

    print(form.__errors__)

.. testoutput::

    {'age': <You must be at least 18>}
