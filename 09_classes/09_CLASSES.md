Classes provide a means of bunding data and functionality together. 
Creating a new class creats a new type of object, allowing new instances of that type to be made. Each class instance can have attributes attached to it for maintaining its state. Class instance can also have methods (defined by its class) for modifying its state. 🙏

Compared with other programming languages, Python's class mechanism adds classes with a minimum of new syntax and semantics. It is a mixture of the class mechanisms found in C++ and Modula-3. Python classes provide all the standard featrues of Object Orientaed Programming: The class inheritance mechanism allows multiple base classes, a derived class can override any methods of its base class or classes. and a method can call the method of a base class with the same name. Objects can contain arbitrary amounts and kinds of data. As is true for modules, classes partake of the dynamic nature of Python; they are created ar runtime, and can be modified further after creation.

In C++ terminology, normally class members(including the data members) are public (except see below Private Variables), and all member functions are virtual. As in Modula-3, there are no shorthands for referencing the object's members from its members: the method function is declared with an explicit first argument representing the object,
which is provided implicitly by the call. As in Smaltalk, 🙏classes themselves are objects.This provides semantics for importing and renaming. Unlike C++ and Modula-3, bulit-in types can be used as base classes for extension by the user. 🙏Also, Like in C++, most built-in operators with sepcial syntax (arithmetic operator, subscripting etc.) can be redefined for class instances.

(Lacking universally accpeted terminology to talk about classes, I will occasinal use of Smaltalk and C++ terms. I would use Modula-3 terms, since its object-oriented semantics are closer to those of Python than C++, but I expect that few readers have heard of it.)
9.1 A Word About Names and Objects

Objects have individuality, and multiple names (in mulitple scopes) can be bound to the same object. This is knows as aliasing in other languages. This is usually not appreciated on a fisrt glance at Python.This is usually not appreciated on a first glance at Python, and can be safely ignored when dealing with immutable basic types (numbes, strings, tuples). However, aliasing has a possibly surprising dffect on the semantics of Python code involving mutable objects such as lists, dictionaries, and most other types. This is usually used to the benefit of the program, since aliases behave like pointers in some respects. For example, passing an object is cheap since only a pointer is passed by the implementation; and if a function modifies an object passed as an argument, the caller will see the change -- this eliminates the need for two diffrent argument passing mechanisms as in Pascal. 

and if a function modifies an object passed as an argument, the caller will see the change -- this eliminates the need for two different argument passing mechanisms as in Pascal.

```
class Person:
    def __init__(self, name):
        self.name = name

original = Person("Alice")
alias = original

alias.name = "Bob"
print(original.name)
```



9.2. Python Scopes and Namespaces
Before introducing classes, I first have to tell you something about Python’s scope rules. Class definitions play some neat tricks with namespaces, and you need to know how scopes and namespaces work to fully understand what’s going on. Incidentally, knowledge about this subject is useful for any advanced Python programmer.

Let’s begin with some definitions.

A namespace is a mapping from names to objects. Most namespaces are currently implemented as Python dictionaries, but that’s normally not noticeable in any way (except for performance), and it may change in the future. Examples of namespaces are: the set of built-in names (containing functions such as abs(), and built-in exception names); the global names in a module; and the local names in a function invocation. In a sense the set of attributes of an object also form a namespace. The important thing to know about namespaces is that there is absolutely no relation between names in different namespaces; for instance, two different modules may both define a function maximize without confusion — users of the modules must prefix it with the module name.

By the way, I use the word attribute for any name following a dot — for example, in the expression z.real, real is an attribute of the object z. Strictly speaking, references to names in modules are attribute references: in the expression modname.funcname, modname is a module object and funcname is an attribute of it. In this case there happens to be a straightforward mapping between the module’s attributes and the global names defined in the module: they share the same namespace!

```
from typing import List
from functools import reduce
class CalcBoring:
    
    @classmethod
    def sum(cls, *args: List[int]):
        results: int = 0
        for i in args:
            results += i
        return results

class CalcFancy:

    @classmethod
    def sum(cls, *args: List[int]):
        # lambda 
         return reduce(lambda x, y: x + y, args, 0)


assert CalcBoring.sum(1, 2, 3, 4, 5) == CalcFancy.sum(1, 2, 3, 4, 5)
```

