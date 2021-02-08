# Python Cheatsheet

## Getting help

*Documentation*
`help(<function_name>)` - Will print the function docstring.

*Built in methods, functions and classes*
`dir(<object>)` - Will show all the built in methods that can be called on the object.

## Print Formatting

*f strings* - https://medium.com/@NirantK/best-of-python3-6-f-strings-41f9154983e

*Printing decimal places*
`print("%.6f" % num)` - Will print the variable `num` to 6 decimal places.

*Right Padding*
`str.rjust(width, fillchar)` - Will make the string `str` have a length of `width`. The extra characters are filled by `fillchar` and ther text is right aligned (new characters are added on the left)


## Array Sorting

*Sorting Numerical Arrays*
`arr.sort()` - Will sort `arr` in ascending order. Will modify the existing list.
    * `arr.sort(reverse=True|False, key=func)`


`sorted(arr)` - Will sort `arr` in ascending order. Will return a new list.
    * `sorted(iterable, reverse=True|False, key=func)`
    
    
## Global Variables

The `global` keyword allows you to modify the variable outside of the current scope.

```
counter = 0

def update_counter():
    global counter
    counter += 1
```
