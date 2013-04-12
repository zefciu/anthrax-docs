Basic concepts of anthrax
=========================================

When working with Anthrax and especially when expanding it one should be
familiar with some concepts and understand some terms the way they are
understood by the library.

**Raw value**:
    A value that is passed to the form. It is most often a string, but can be
    also a file or some other object. Raw value is less useful for our
    application and can be possibly invalid.

**Python value**:
    A python object that is useful for our application and validated. This
    value is a result of raw value conversion.

**Field**:
    An object that handles some basic value. The field has no idea how it is
    represented to user. All it knows is how to convert raw value to python
    value and the other way around, possibly raising validation problems.

**Frontend**:
    A system for rendering and accepting forms. It can be something like 'raw
    html' or 'some javascript framework' or 'pdf forms'.

**Widget**:
    An abstract, frontend-independent way of representing a field. E.g. 'a
    spinner', 'a dropdown'. Fields define a list of widgets that can be used to
    render them.

**View**:
    A representation of a widget in a frontend. Frontends *implement* a a set
    of widgets by assigning views to them.

**View negotiation**:
    A process of finding an optimal way of viewing a form. It is performed
    between a form and a frontend. The form presents its fields with preferred
    widgets. The frontend uses the view for the best widget it implements.

**Reflection**:
    Creating a form from some other object. A simple example of reflection
    source might be a class in some ORM or some other data structure
    definition.
