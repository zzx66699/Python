# List
A list is a `mutable` collection of items in `sequence`.

## Slicing notation
```py
# If the start or stop parameters are omitted, the slice will
# start at index zero, and will stop at the end of the list.
colors = ["Red", "Purple", "Green", "Yellow", "Orange", "Pink", "Blue", "Grey"]
primary_colors = colors[::3]
primary_colors
>>> ['Red', 'Yellow', 'Blue']

```

## .reverse() - Reverse
```py
# reversed list
>>> numbers = [1, 2, 3]
>>> numbers.reverse()
>>> numbers
[3, 2, 1]


# or we can just use slicing
list = [1,2,3]
reversed_list = list[::-1]

print(reversed_list)
>>> [3,2,1]
```

## .sort() & sorted() - Reorder
```py
>>> names = ["Tony", "Natasha", "Thor", "Bruce"]
>>> names.sort(reverse=True)

>>> names
["Tony", "Thor", "Natasha", "Bruce"]


# if we don't want to mutate the original list, we can use sort() and it will return a sorted copy.
>>> names = ["Tony", "Natasha", "Thor", "Bruce"]

>>> sorted(names)
['Bruce', 'Natasha', 'Thor', 'Tony']
```

## Convert from set / tuple / str / dic
The list() constructor can be used with an `iterable` as an argument. `Elements` in the iterable are cycled through by the constructor and added to the list in order.
```py
# The tuple is unpacked and each element is added.
multiple_elements_from_tuple = list(("Parrot", "Bird", 334782))

multiple_elements_from_tuple
>>> ['Parrot', 'Bird', 334782]


# The set is unpacked and each element is added.
multiple_elements_from_set = list({2, 3, 5, 7, 11})

multiple_elements_from_set
>>> [2, 3, 5, 7, 11]


# String elements (Unicode code points) are iterated through and added *individually*.
multiple_elements_string = list("Timbuktu")

multiple_elements_string
>>> ['T', 'i', 'm', 'b', 'u', 'k', 't', 'u']


# The iteration default for dictionaries is over the keys, so only key data is inserted into the list.
source_data = {"fish": "gold", "monkey": "brown"}

multiple_elements_dict_1 = list(source_data)
>>> ['fish', 'monkey']


# Numbers are not iterable, and so attempting to create a list with a number passed to the constructor fails.
one_element = list(16)
>>> Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'int' object is not iterable
```

## Combine the list
```py
# Using the plus + operator unpacks each list and creates a new list, but it is not efficient.
>>> new_via_concatenate = ["George", 5] + ["cat", "Tabby"]

>>> new_via_concatenate
['George', 5, 'cat', 'Tabby']

# Likewise, using the multiplication operator * is the equivalent of using + n time.
>>> first_group = ["cat", "dog", "elephant"]
>>> multiplied_group = first_group * 3

>>> multiplied_group
['cat', 'dog', 'elephant', 'cat', 'dog', 'elephant', 'cat', 'dog', 'elephant']
```

## append() & extend() & insert() - Add items
To add an item to the end or "right-hand side" of an existing list, use `<list>`.append(`<item>`):
```py
>>> numbers = [1, 2, 3]
>>> numbers.append(9)

>>> numbers
[1, 2, 3, 9]
```
`<list>`.insert(`<index>`, `<item>`) gives you the ability to add the item to a specific index in the list. It takes 2 parameters:
- the `<index>` at which you want the item to be inserted.
- the `<item>` to be inserted.  

If the given index is 0, the item will be added to the start ("left-hand side") of the list.
```py
>>> numbers = [-2, 1, 2, 3]
>>> numbers.insert(1, 0)

>>> numbers
[-2, 0, 1, 2, 3]
```

`<list>`.extend(`<item>`) can be used to combine an existing list with the `elements` from another iterable (for example, a set, tuple, str, or list). The iterable is unpacked and elements are appended in order (Using `<list>`.append(`<item>`) in this circumstance would add the entire iterable as `a single item`.)
```py
>>> numbers = [1, 2, 3]
>>> other_numbers = [5, 6, 7]

>>> numbers.extend(other_numbers)

>>> numbers
[1, 2, 3, 5, 6, 7]

>>> numbers.extend([8, 9])

>>> numbers
[1, 2, 3, 5, 6, 7, 8, 9]

>>> numbers.append([8,9])

>>> numbers
[1, 2, 3, 5, 6, 7, 8, 9, [8, 9]]
```

## .remove() & .pop() & .clear() - Remove items
To delete an item from a list use `<list>`.remove(`<item>`).   
`<list>`.remove(`<item>`) will throw a ValueError if the item is not present in the list.
```py
>>> numbers = [1, 2, 2, 3]
>>> numbers.remove(2)

>>> numbers   # Only remove the first item
[1, 2, 3]

# Trying to remove a value that is not in the list throws a ValueError
>>> numbers.remove(0)
ValueError: list.remove(x): x not in list
```
Alternatively, using the `<list>`.pop(`<index>`) method will both remove and **return an element for use**.

`<list>`.pop(`<index>`) takes one optional parameter: the index of the item to be removed and returned. If the (optional) index argument is not specified, the `final element` of the list will be removed and returned. 
```py
>>> numbers = [1, 2, 3]

>>> numbers.pop(0)
1

>>> numbers
[2, 3]

>>> numbers.pop()
3

>>> numbers
[2]

>>> numbers.pop(1)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
IndexError: pop index out of range
```
All elements can be removed from a list with list.clear().
```py
>>> numbers = [1, 2, 3]
>>> numbers.clear()

>>> numbers
[]
```

## .count() - Occurrences of an item in a list
It takes the item to be counted as its argument and returns the total number of times that element appears in the list.
```py
>>> items = [1, 4, 7, 8, 2, 9, 2, 1, 1, 0, 4, 3]

>>> items.count(1)
3
```

## <list>.index(`<item>`) - Finding the index of items
It returns the index number of the `first occurrence` of an item passed in. If there are no occurrences, a ValueError is raised. 
```py
>>> items = [7, 4, 1, 0, 2, 5]

>>> items.index(4)
1

>>> items.index(10)
ValueError: 10 is not in list
start and end indices can also be provided to narrow the search to a specific section of the list:

>>> names = ["Tina", "Leo", "Thomas", "Tina", "Emily", "Justin"]

>>> names.index("Tina")
0

>>> names.index("Tina", 2, 5)  #start from index 2 and end at index 5
3
```