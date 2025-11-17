## enumerate
Normally, when you loop like this:
``` python
for url in urls:
    ...
```
You only get the element (url), but no position / index.  

With `enumerate`
``` python
for idx, url in enumerate(urls, start=1)
```
You get two things at the same time:  
idx → the index (1, 2, 3, 4, …)  
url → the value from the list (urls[idx-1])  

| idx | url |
| --- | --- |
| 1   | "a" |
| 2   | "b" |
| 3   | "c" |

