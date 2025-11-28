## sep
```python
a = int(input())
b = int(input())
print(a+b, a-b, a*b, sep="\n")
```

## end
```python
for i in range(1,n+1):
    print(i, end="")

# input = 3
# output = 123
```

## f-string
put variable or expression in the {}, put string outside {}
```python
name = "Alice"
print(f"Hello, {name}!")  
print(f"2 + 3 = {2 + 3}")
print(f"Uppercase: {name.upper()}")
```
### Number Formatting
show 2 decimals
```python
x = sum(list) / len(list)
print(f"{x:.2f}")
```
show 2 decimals and in % format
```python
x = 0.123
print(f"{x:.2%}")

# 12.35%
```
total width is 8, and keep 3 decimals
```python
print(f"3.13435346546:8.3f")
# ____3.142 # 4 spaces before 3.142
```
