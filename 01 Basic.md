# Basic


## Name Assignment (Variables & Constants)
Programmers can bind `names` (also called variables) to any type of object using the assignment = operator: `<name> = <value>`. A `name` can be reassigned (or re-bound) to different values over its lifetime.

`Constants` are names meant to be assigned only once in a program. They should be defined at a module (file) level, and are typically visible to all functions and classes in the program. Using `SCREAMING_SNAKE_CASE` signals that the name should not be re-assigned, or its value mutated. All caps signal that this is intended as a constant.


## Functions
Units of functionality are encapsulated in `functions`., which are themselves objects.

Functions explicitly return a value or object via the  `return` keyword.  
Functions that do not have an explicit `return` expression will implicitly return the `None` object. 

Dot (.) notation is used for calling functions defined inside a class or module.
```py
# Calling methods or functions in classes and modules.
>>> start_text = "my silly sentence for examples."
>>> str.upper(start_text)  # Calling the upper() method for the built-in str class.
"MY SILLY SENTENCE FOR EXAMPLES."

# Importing the math module
import math

>>> math.pow(2,4)  # Calling the pow() function from the math module
>>> 16.0
```


## Docstrings
The first statement of a function body can optionally be a `docstring`, which concisely summarizes the function or object's purpose. Docstrings are declared using triple double quotes `(""")` indented at the same level as the code block:
```py
# An example from PEP257 of a multi-line docstring.
def complex(real=0.0, imag=0.0):
    """Form a complex number.

    Keyword arguments:
    real -- the real part (default 0.0)
    imag -- the imaginary part (default 0.0)
    """

    if imag == 0.0 and real == 0.0:
        return complex_zero
```

`Docstrings` are read by automated documentation tools and are returned by calling the special attribute `.__doc__` on the function, method, or class name.
```py
# Printing the __doc__ attribute for the built-in type: str.
>>> print(str.__doc__)
str(object='') -> str
str(bytes_or_buffer[, encoding[, errors]]) -> str

Create a new string object from the given object. If encoding or
errors is specified, then the object must expose a data buffer
that will be decoded using the given encoding and error handler.
Otherwise, returns the result of object.__str__() (if defined)
or repr(object).
encoding defaults to sys.getdefaultencoding().
errors defaults to 'strict'.
```


## ValueError 
```py
if number <= 0:
    raise ValueError("Only positive integers are allowed")
```
