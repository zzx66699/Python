# NaN
Any ordered comparison of a number to a `NaN` (not a number) type is False. A confusing side effect of Python's NaN definition is that NaN never compares equal to NaN.
```py
x = float('NaN')

3 < x
>>>  False

x < 3
>>>  False

# NaN never compares equal to NaN
x == x
>>> False
```