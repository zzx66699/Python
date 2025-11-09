# 🧩 BeautifulSoup Common Functions — Cheat Sheet

## 1️⃣ Creating the soup

```python
from bs4 import BeautifulSoup

html = "<html><body><h1>Hello</h1></body></html>"
soup = BeautifulSoup(html, "html.parser")
```

---

## 2️⃣ Finding elements

| Function | Description | Example |
|-----------|--------------|----------|
| `soup.find(tag, attrs)` | Finds the **first** matching element | `soup.find("h1")` |
| `soup.find_all(tag, attrs)` | Finds **all** matching elements (returns a list) | `soup.find_all("p", class_="price")` |
| `soup.select(css_selector)` | Finds all elements using **CSS selector** | `soup.select(".product-name span")` |
| `soup.select_one(css_selector)` | Finds the **first** element matching a CSS selector | `soup.select_one("div#main > h1")` |
| `soup.get_text(strip=True)` | Extracts all text inside the tag | `soup.find("h1").get_text(strip=True)` |

---

## 3️⃣ Navigating inside the DOM tree

| Operation | Description | Example |
|------------|--------------|----------|
| `.parent` | Get the parent element | `tag.parent` |
| `.children` | Get all direct children (iterator) | `list(tag.children)` |
| `.descendants` | All nested children (deep) | `list(tag.descendants)` |
| `.next_sibling` / `.previous_sibling` | Move between adjacent elements | `tag.next_sibling` |
| `.find_next(tag)` / `.find_previous(tag)` | Find next/previous matching element | `tag.find_next("p")` |

---

## 4️⃣ Accessing attributes

| Code | Description |
|------|--------------|
| `tag["href"]` | Get attribute value (e.g., link URL) |
| `tag.get("src")` | Safely get attribute, returns `None` if missing |
| `tag.attrs` | Dictionary of all attributes |

**Example:**
```python
a = soup.find("a")
print(a["href"])
print(a.get("title"))
```

---

## 5️⃣ Searching with filters

You can filter by tag name, class, id, text, or even regex:

```python
soup.find_all("div", class_="price")
soup.find_all(id="product-info")
soup.find_all(text=re.compile("€\d+"))
```

---

## 6️⃣ Modifying (optional)

If you ever need to clean or alter HTML:

```python
tag.decompose()       # remove the tag completely
tag.clear()           # remove tag contents but keep tag
tag["class"] = "new"  # change attribute
```

---

## 7️⃣ Quick summary

| Category | Common methods |
|-----------|----------------|
| **Search** | `find()`, `find_all()`, `select()`, `select_one()` |
| **Navigate** | `.parent`, `.children`, `.next_sibling`, `.find_next()` |
| **Text** | `.get_text(strip=True)` |
| **Attributes** | `.get()`, `["attr"]`, `.attrs` |
| **Modify / Clean** | `.decompose()`, `.clear()` |
