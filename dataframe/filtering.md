## 🧮 1. Boolean Filtering in Pandas

### 🔹 Select rows where a column is **not None / NaN**
```python
df = df[df["current_price"].notna()]
```

---

### 🔹 Multiple Conditions: `|` (OR) and `&` (AND)

```python
# Intersection of two conditions (AND): True only if both are True
is_wife = (data_frame["gender"] == "Female") & (data_frame["married"])

# Result example:
# 0    False
# 1     True
# 2    False
# 3    False
# 4    False
# 5    False
# dtype: bool
```

```python
# Union of two conditions (OR): True if either condition is True
is_skillful = (data_frame["educ"] > 9) | (data_frame["exper"] > 3)

# Result example:
# 0     True
# 1     True
# 2    False
# 3     True
# 4     True
# 5     True
# dtype: bool
```
💡 **Tips**
- Always wrap each condition in parentheses `()` when using `&` or `|`.
- Avoid using Python’s `and` / `or` with pandas Series — they’ll raise an error.
---

### 🔹 “Not Equal To” Condition
```python
not_female = data_frame["gender"] != "Female"
```

or equivalently:
```python
not_female = ~(data_frame["gender"] == "Female")
```

---

