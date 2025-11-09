# 🧩 Python Regular Expressions (Regex) — Complete Guide with Examples

## 0. Basics: The `re` module and raw strings

Import the module:

```python
import re
```

Always use **raw strings** (`r"..."`) when writing regex patterns:

```python
pattern = r"\d+\.\d+"
```

Raw strings prevent Python from interpreting `\n`, `\t`, etc. as escape sequences.

---

## 1. Common functions in the `re` module

### 1.1 `re.search(pattern, string, flags=0)`

Searches for **the first match** anywhere in the string.

```python
import re

text = "The price is 1.69 Euro"
m = re.search(r"\d+\.\d+", text)

if m:
    print(m.group())   # '1.69'
    print(m.span())    # (13, 17)
```

---

### 1.2 `re.findall(pattern, string, flags=0)`

Finds **all matches** and returns a list.

```python
text = "Prices: 1.69 Euro, 2.39 Euro, 0.99 Euro"
prices = re.findall(r"\d+\.\d+", text)
print(prices)  # ['1.69', '2.39', '0.99']
```

---

### 1.3 `re.sub(pattern, repl, string, flags=0)`

Search and replace.

```python
text = "Dirk is cheap. Dirk has offers."
new_text = re.sub(r"Dirk", "AH", text)
print(new_text)  # "AH is cheap. AH has offers."
```

---

### 1.4 `re.split(pattern, string, flags=0)`

Split a string by a regex pattern.

```python
text = "apple, banana; orange  pear"
parts = re.split(r"[,;\s]+", text)
print(parts)  # ['apple', 'banana', 'orange', 'pear']
```

---

## 2. Regex building blocks (metacharacters)

### 2.1 Literal characters

- `a` → matches the letter “a”
- `1` → matches the character “1”
- `.` → special: means “any character” (except newline)

```python
re.search(r"Dirk", "Dirk is cheap")  # matches
```

---

### 2.2 Character types

| Symbol | Meaning | Example matches |
|---------|----------|----------------|
| `.` | Any single character (except newline) | `a`, `1`, `@` |
| `\d` | Digit (0–9) | `1`, `8`, `0` |
| `\w` | Word character (letter, digit, underscore) | `a`, `Z`, `9`, `_` |
| `\s` | Whitespace (space, tab, newline) | `' '`, `\n`, `\t` |

Examples:

```python
re.findall(r"\d", "a1b2c3")      # ['1', '2', '3']
re.findall(r"\w", "a1_ ?")       # ['a', '1', '_']
re.findall(r"\s", "a b\tc\n")    # [' ', '\t', '\n']
re.findall(r"a.c", "abc aXc a c") # ['abc', 'aXc']
```

---

### 2.3 Quantifiers: `*`, `+`, `?`, `{m,n}`

| Quantifier | Meaning | Example |
|-------------|----------|----------|
| `*` | 0 or more times | `a*` matches `""`, `"a"`, `"aa"` |
| `+` | 1 or more times | `a+` matches `"a"`, `"aa"` |
| `?` | 0 or 1 time | `a?` matches `""` or `"a"` |
| `{m}` | Exactly m times | `\d{4}` matches 4 digits |
| `{m,}` | At least m times | `\d{2,}` matches 2+ digits |
| `{m,n}` | Between m and n times | `\d{1,2}` matches 1 or 2 digits |

Examples:

```python
re.findall(r"a+", "aaab")             # ['aaa']
re.findall(r"\d{1,2}", "a1 b22 c333") # ['1', '22', '33']
re.findall(r"\d{4}", "2025-11-05")    # ['2025']
```

---

### 2.4 Character sets: `[...]`

Match *one character* from a list or range.

| Example | Meaning |
|----------|----------|
| `[abc]` | `a` or `b` or `c` |
| `[0-9]` | any digit |
| `[A-Za-z]` | any letter |
| `[^0-9]` | not a digit |

Examples:

```python
re.findall(r"[abc]", "axbycz")  # ['a', 'b', 'c']
re.findall(r"[0-9]", "a1b2c3")  # ['1', '2', '3']
re.findall(r"[^0-9]", "a1b2")   # ['a', 'b']
```

---

### 2.5 Groups and capturing: `( ... )`

Parentheses group expressions and **capture** parts of a match.

```python
m = re.search(r"(\d+)\.(\d+)", "Price 1.69 Euro")
print(m.group(0))  # '1.69'  entire match
print(m.group(1))  # '1'     first group
print(m.group(2))  # '69'    second group
print(m.groups())  # ('1', '69')
```

You used this in your `_parse_valid_range()` function to extract day/month/year.

---

### 2.6 Alternation: `|`

Means “OR”.

```python
re.findall(r"Dirk|AH", "Dirk vs AH vs Jumbo")  # ['Dirk', 'AH']
```

Combined with groups:

```python
re.findall(r"(voor|for)\s+([\d.,]+)\s*Euro",
           "voor 1,69 Euro and for 2.39 Euro")
```

Group 2 will hold the numeric price.

---

### 2.7 Anchors: `^` and `$`

| Anchor | Meaning |
|---------|----------|
| `^` | start of string |
| `$` | end of string |

Examples:

```python
re.match(r"^\d{4}$", "2025")   # match (4 digits only)
re.match(r"^\d{4}$", "2025-")  # no match
```

Used for strict format validation.

---

### 2.8 Escaping special characters: `\`

Use `\` to match regex symbols literally.

| You want to match | Write this |
|-------------------|-------------|
| a dot `.` | `\.` |
| a backslash `\` | `\\` |

Example:

```python
re.findall(r"\d+\.\d+", "1.69 2.39")  # ['1.69', '2.39']
```

---

## 3. Real examples related to your project

### 3.1 Extract price: `"voor 1.69 Euro"`

```python
text = "Deze week in de aanbieding bij Dirk voor 1.69 Euro"
m = re.search(r"voor\s+([\d.,]+)\s*Euro", text)

if m:
    price_str = m.group(1)              # '1.69'
    price = float(price_str.replace(",", "."))
    print(price)                        # 1.69
```

Explanation:

| Part | Meaning |
|------|----------|
| `voor` | literal word “voor” |
| `\s+` | one or more spaces |
| `([\d.,]+)` | digits, commas, dots (captured) |
| `\s*Euro` | optional spaces before “Euro” |

---

### 3.2 Extract date range: `"Geldig van ... t/m ..."`

```python
text = "Geldig van woensdag 5 november t/m dinsdag 11 november 2025"

m = re.search(
    r"Geldig van\s+\w+\s+(\d{1,2})\s+(\w+)\s+t/m\s+\w+\s+(\d{1,2})\s+(\w+)\s+(\d{4})",
    text,
    flags=re.IGNORECASE
)
if m:
    print(m.groups())
    # ('5', 'november', '11', 'november', '2025')
```

Explanation:
- `(\d{1,2})` = 1–2 digit day
- `(\w+)` = month name
- `(\d{4})` = year (4 digits)
- You can then map month names via a dictionary like `MONTHS_NL`.

---

## 4. Flags (`re.IGNORECASE`, etc.)

You can pass flags to change how matching works:

| Flag | Meaning |
|------|----------|
| `re.IGNORECASE` or `re.I` | case-insensitive |
| `re.MULTILINE` or `re.M` | `^` and `$` match start/end of each line |
| `re.DOTALL` or `re.S` | `.` matches newline too |

Example:

```python
re.search(r"geldig van", "Geldig van ...", flags=re.IGNORECASE)
```

---

## 5. Practice: try in a small script

```python
import re

cases = [
    "Geldig van woensdag 5 november t/m dinsdag 11 november 2025",
    "Deze week in de aanbieding bij Dirk voor 1.69 Euro",
]

for text in cases:
    m = re.search(r"\d+\.\d+", text)
    print(text, "=>", m.group() if m else None)
```

Keep modifying `pattern` and see what happens — that’s the fastest way to learn.

---

## ✅ Summary: what to remember

| Symbol | Meaning |
|--------|----------|
| `\d` | digit |
| `\w` | word char (letter/number/underscore) |
| `\s` | whitespace |
| `.` | any char (except newline) |
| `*` | 0+ times |
| `+` | 1+ times |
| `?` | 0 or 1 |
| `{m,n}` | between m and n times |
| `( )` | group and capture |
| `|` | or |
| `^`, `$` | start/end anchors |
| `[...]` | character set |
| `\.` | literal dot |
| `\d+\.\d+` | a decimal number like 1.69 |

---

> 🧠 Tip: In practice, always test your regex at [regex101.com](https://regex101.com/) — it shows explanations for every part and supports Python flavor.
