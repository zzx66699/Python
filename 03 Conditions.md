# Conditions
Control flow statements determine **how a program executes**.  
They include conditional statements, loops, and loop control statements.

---

## Conditional Statements

###  if / elif / else

```python
x = 10

if x > 0:
    print("Positive number")
elif x == 0:
    print("Zero")
else:
    print("Negative number")
```

**Explanation:**  
- `if` checks a condition.  
- `elif` stands for “else if”, used for multiple conditions.  
- `else` executes only if none of the above are true.

---

## Loops

Python has two main types of loops: `for` and `while`.

### `for` loop

Used to iterate over a sequence (list, tuple, string, etc.)

```python
for i in range(5):
    print("i =", i)
```

### `while` ... `else` loop
Runs as long as the condition is true.
Once while end, run else.

```python
# Pig Latin
# Rule 1: If a word begins with a vowel, or starts with "xr" or "yt", add an "ay" sound to the end of the word.
# Rule 2: If a word begins with one or more consonants, first move those consonants to the end of the word and then add an "ay" sound to the end of the word.
# Rule 3: If a word starts with zero or more consonants followed by "qu", first move those consonants (if any) and the "qu" part to the end of the word, and then add an "ay" sound to the end of the word.
# Rule 4: If a word starts with one or more consonants followed by "y", first move the consonants preceding the "y"to the end of the word, and then add an "ay" sound to the end of the word.

def translate(text):
    words = text.split()
    vowels = "aeiou"
    result = []

    for word in words:
        w = word.lower()

        if w[0] in vowels or w.startswith("xr") or w.startswith("yt"):
            result.append(w + "ay")
            continue
        
        i = 0
        n = len(w)
        while i < n and w[i] not in vowels:
            if w[i:i+2] == "qu":
                result.append(w[i+2:] + w[:i+2] + "ay")
                break
            if w[i] == "y" and i != 0:
                result.append(w[i:] + w[:i] + "ay")
                break
            i += 1
        else:
            result.append(w[i:] + w[:i] + "ay") 
    
    return " ".join(result)
    

```

---

## 3️⃣ Loop Control Statements

Loop control statements modify the normal sequence of execution inside a loop.

| Statement | Description |
|------------|--------------|
| `break` | Immediately terminate the entire loop |
| `continue` | Skip the rest of the current iteration and continue to the next |
| `pass` | Do nothing — acts as a placeholder |

---

### 🧩 Example — `break`

```python
for i in range(5):
    if i == 3:
        break
    print(i)
```

**Output:**
```
0
1
2
```

➡️ When `i == 3`, the `break` statement stops the entire loop.

---

### 🧩 Example — `continue`

```python
for i in range(5):
    if i == 2:
        continue
    print(i)
```

**Output:**
```
0
1
3
4
```

➡️ When `i == 2`, the `continue` statement skips `print(i)` and jumps to the next iteration.

---

### 🧩 Example — `pass`

```python
for i in range(5):
    if i == 2:
        pass  # placeholder, does nothing
    print(i)
```

**Output:**
```
0
1
2
3
4
```

➡️ `pass` simply allows the program to run without doing anything — useful when you haven’t implemented logic yet.

---

## 4️⃣ Nested Loops + Control Statements

```python
for i in range(3):
    for j in range(3):
        if j == 1:
            continue  # skip j == 1
        print(f"i={i}, j={j}")
```

**Output:**
```
i=0, j=0
i=0, j=2
i=1, j=0
i=1, j=2
i=2, j=0
i=2, j=2
```

---

## 5️⃣ `else` with Loops

Python allows `else` clauses with `for` and `while` loops.  
They execute **only if the loop was not terminated by `break`.**

### 📘 Example — `for/else`

```python
for i in range(5):
    print(i)
else:
    print("Loop finished normally.")
```

**Output:**
```
0
1
2
3
4
Loop finished normally.
```

### 📘 Example — `for/else` with `break`

```python
for i in range(5):
    if i == 3:
        break
    print(i)
else:
    print("Loop finished normally.")
```

**Output:**
```
0
1
2
```

➡️ The `else` block did **not** run because the loop ended via `break`.

---

## 6️⃣ Summary Table

| Statement | Purpose | Typical Use Case |
|------------|----------|------------------|
| `break` | Exit the loop completely | Stop looping when a condition is met |
| `continue` | Skip the rest of this iteration | Ignore specific cases |
| `pass` | Do nothing | Placeholder in incomplete code |
| `for/else` | Execute if loop finishes normally | Search loop with no matches |
| `while/else` | Execute if loop finishes normally | Repeated condition with completion check |

---

## ✅ Practice Exercises

Try the following examples on your own:

```python
# 1. Print numbers 1–10 except those divisible by 3
for i in range(1, 11):
    if i % 3 == 0:
        continue
    print(i)

# 2. Stop the loop when i reaches 7
for i in range(1, 11):
    if i == 7:
        break
    print(i)

# 3. Use pass as a placeholder
for i in range(5):
    if i == 2:
        pass
    print(i)
```

---

End of notes ✨
