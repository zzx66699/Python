# Numbers
Python has 3 different types of built-in numbers: `integers` (int), `floating-point` (float), and `complex` (complex). 

`Fractions` (fractions.Fraction) and `Decimals` (decimal.Decimal) are also available via import from the standard library.

Whole numbers including hexadecimal (hex()), octal (oct()) and binary (bin()) numbers without decimal places are also identified as ints.

Python fully supports arithmetic between these different number types, and will convert **narrower numbers** to match their **less narrow counterparts** when used with the binary arithmetic operators (+, -, *, /, //, and %).
```py
a = 5        # int
b = 2.0      # float
result = a + b  # int is narrower than float, so a will be changed to float in the arithmetic
print(result)  # output: 7.0
```
## complex()
```py
z = complex(1, 2)   # 1 + 2j
```

## Arithmetic

### % - Modulo
a mod b 
``` python
print (3 % 2)  

# 1
```

### / - Division
Division always returns a `float`, even if the result is a whole number
```py
6/2

# 3.0
```

### // - Integer division / Floor division
``` python
# If an int result is needed, you can use // to truncate the result.
print (3//5, 3/5, sep="\n")

# 0
# 0.6
```

### ** - Exponentiation
```py
result = 2 ** 3
print(result)  
>>> 8


result = 9 ** 0.5    # compute the square root
print(result)  
>>> 3
```

## round() - keep xx decicals
this is for calculation. it is not for formatting!
```python
x = 0.1 + 0.2
print(x)      # 0.30000000000000004

x = round(x, 2)
print(x)      # 0.3

print(f"{x:.2}")   # 0.30
```

## Convert from int to float
```py
>>> int(3.45)
3

>>> float(3)
3.0
```