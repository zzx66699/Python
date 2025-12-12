# string 
## <str>[<start>:stop:<step>] - Slicing
``` python
a = 'asdfghjklasdfghj'      
print(a[1:10:3])          
>>> 'sgk'                
```
Add an element at a given index
```python
a = "iewk"
a = a[:3] + "b" + a[3:]
print (a)
>>> "iewbk"
```
Remove an element at a given index
```python
# Remove the element at the index 3
a = "afhuwe"
a = a[:3] + a[4:]
print(a)

>>> "afhwe"
```
------------------
## enumerate()
```py
exercise = 'asdfg'
for code_point in exercise:
    print(code_point)
>>> "a"
    "s"
    "d"
    "f"
    "g"


# Using enumerate will give both the value and index position of each element.
for index, code_point in enumerate(exercise):
    print(index, ": ", code_point)
>>> 0: "a"
    1: "s"
    2: "d"
    3: "f"
    4: "g"

```
---------------------------
## strip() - Removes characters from both the beginning and the end of the original string
The code points specified in <chars> are not a prefix or suffix - `all combinations` of the code points will be removed starting from both ends of the string. If nothing is specified for <chars>, all combinations of whitespace code points will be removed. Whitespace characters include:
- space " "
- tab "\t"
- newline "\n"
- carriage return "\r"

```python
# "Fine. Be that way!" This is how he responds to silence. The convention used for silence is nothing, or various combinations of whitespace characters.

def response(hey_bob):
    if hey_bob.strip() == "":
        return "Fine. Be that way!"
```

```py
# This will remove "https://", because it can be formed from "/stph:". 
'https://unicode.org/emoji/'.strip('/stph:')
>>>'unicode.org/emoji'

# Prefix and suffix in one step.
'unaddressed'.strip('dnue')
>>> 'address'

'  unaddressed  '.strip('dnue ')
>>> 'address'
```
------------------
## .split() - Break into smaller strings
The default is split by whitespace characters and get a list.
```py
text = "quick fast run"
words = text.split()
print(words)

>>> ["quick", "fast", "run"]
```
Example
```py
#Implement the adjective_to_verb(<sentence>, <index>) function that takes two parameters. A sentence using the vocabulary word, and the index of the word, once that sentence is split apart. The function should return the extracted adjective as a verb.


def adjective_to_verb(sentence, index):
    """Change the adjective within the sentence to a verb.

    :param sentence: str - that uses the word in sentence.
    :param index: int - index of the word to remove and transform.
    :return: str - word that changes the extracted adjective to a verb.

    For example, ("It got dark as the sun set.", 2) becomes "darken".
    """
    word = sentence.strip(".").split()  # remove the . for the last word first, then split
    return word[index] + "en"
```
---------------------------------
## str.join() - Makes a new string from the iterables elements.
```py
chickens = ["hen", "egg", "rooster"] # Lists are iterable.

' '.join(chickens)
>>> 'hen egg rooster'

# Any string can be used as the joining element.
>>> ' :: '.join(chickens)
'hen :: egg :: rooster'

' 🌿 '.join(chickens)
>>> 'hen 🌿 egg 🌿 rooster'
```
Any iterable can be used as input.
```py
>>> flowers = ("rose", "daisy", "carnation")  # Tuples are iterable.
>>> '*-*'.join(flowers)
'rose*-*daisy*-*carnation'

>>> flowers = {"rose", "daisy", "carnation"}  # Sets are iterable, but output order is not guaranteed.
>>> '*-*'.join(flowers)
'rose*-*carnation*-*daisy'

>>> phrase = "This is my string"  # Strings are iterable, but be careful!
>>> '..'.join(phrase)
'T..h..i..s.. ..i..s.. ..m..y.. ..s..t..r..i..n..g'
```
------------------
# str() - Change an interger to string
```py
print('The sum is ' + str(z)) 
```
---------
## .lower() & .upper()
```py
w = word.lower()
```

------------------
## startswith() & endswith() 
```py
if w.startswith("xr")

'Do you want to 💃?'.endswith('💃')  # the string includes the punctuation
>>> False
```

------------------
## replace() - Remove a part from the string / Replace a part 
``` python
a = 'asasdf'
b = a.replace('a','')  
print(b)
>>>'ssdf'  

c = a.replace('a','e')       
print(c) 
>>> 'esesdf'

print(a) 
>>> 'asasdf'    # the raw string doesn't change                            
```

-----------------------
## Repeat
```py
a = "wufew"
b = a * 3
print(b)
>>> "wufewwufewwufew"
```
----------------------
## .isalpha() & .isdigit()
```py
i.isalpha()
```
----------------------
## <str>.title() - Capitalizes the first "character" of each word
```py
man_in_hat_en = 'the man in the hat.'
man_in_hat_en.title()
>>>'The Man In The Hat.'
```


a = '1asad_sa_'
print(a.translate({ord("_"): None}))                       # 建translation table 
--- 1asadsa
print(a.translate({ord(c): None for c in "1a_"}))          # 多个值一起删掉
--- sds

------------------
