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

r.encoding='utf-8-sig'

## Programatically remove BOM



