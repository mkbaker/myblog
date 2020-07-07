---
title: "Removing a BOM with Python"
date: 2020-06-22T11:07:57-05:00
draft: true
---

## The Error
I was getting the following error when trying to execute a python script: 
```
UnicodeEncodeError: 'charmap' codec can't encode character '\ufeff' in position 109: character maps to <undefined>
```
After some googling, I discovered that this character is a BOM. 
I don't want to get into what a BOM is, but here's a link to the [Wikipedia page](https://en.wikipedia.org/wiki/Byte_order_mark)

I discovered that I had created this problem myself by copying/pasting some database code from another program. The copy used this BOM character instead of a regular space. 
This python script broke when scraping this file at json.loads(). 

This leaves two problems to solve:
1. Remove the BOMs from the original file. 
2. Prevent this from happening again by fixing the python script. 

Let's solve number 2 first, since we can use the offending file as a control.

## Python fix

This took me forever to figure out -- but there is a simple solution! 

The trick is to include `errors="ignore"` when opening the file. This example is shamelessly copied from [this excellent Stack Overflow answer:](https://stackoverflow.com/questions/30922721/remove-all-characters-which-cannot-be-decoded-in-python)

```
with open('filename', 'r', encoding='utf8', errors='ignore') as f:
    ...
```

This tells python to ignore any characters that would usually throw an error. This will ignore the BOM, and as an added bonus, any other character that would mess up your code -- emojis, etc. 

## Programatically remove BOM

Since the code is now ignoring the BOM as well as any other undefined characters, one could argue that it isn't necessary to remove the BOM from your files. But why contribute to tech debt? 

You can remove a BOM from a string with a simple replace() method:
```
myString = myString.replace('\ufeff', '')
```

