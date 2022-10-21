# generator do not get stored in memory then how come i get those value even after the loop is ended
A loop is created for generating a series of number
```python
import ctypes

g = [ ]

for item1 in range(10):
    print(f'memory address of {item1} = {id(item1)}')
    g.append(id(item1))
```
>here my loops end but the only thing is stored is the location or the memory address not the number

>checking the values at that memory address
```python
for item in g:
    a = ctypes.cast(item, ctypes.py_object).value
    print(f'values at that memory address = {a}')
```
> here <br>
a = ctypes.cast(item, ctypes.py_object).value <br> 
gives the value stored at that memory address <br>

# output
```python
C:\Users\Admin\Desktop\pythonProject\venv\Scripts\python.exe C:/Users/Admin/Desktop/pythonProject/main.py 
memory address of 0 = 140709883771936
memory address of 1 = 140709883771968
memory address of 2 = 140709883772000
memory address of 3 = 140709883772032
memory address of 4 = 140709883772064
memory address of 5 = 140709883772096
memory address of 6 = 140709883772128
memory address of 7 = 140709883772160
memory address of 8 = 140709883772192
memory address of 9 = 140709883772224
```
> my loop has ended before soo they are no longer in the memory according to generator but still those memory address shows the values of those elements of previous loop
```python
values at that memory address = 0
values at that memory address = 1
values at that memory address = 2
values at that memory address = 3
values at that memory address = 4
values at that memory address = 5
values at that memory address = 6
values at that memory address = 7
values at that memory address = 8
values at that memory address = 9

Process finished with exit code 0
```
# Explaination for memory address I found through stackoverflow
> This is just an implementation detail of cpython which has prebuilt singletons for the numbers -4 through 256. Python holds its own reference to these numbers. Your loop adds and removes a reference, but that original reference remains, keeping the values in the same place in memory. <br>If you choose a range outside of this sequence, you get a different result

```python
import ctypes

g = [ ]

for item1 in range(257, 267):
    print(f'memory address of {item1} = {id(item1)}')
    g.append(id(item1))

for item in g:
    a = ctypes.cast(item, ctypes.py_object).value
    print(f'values at that memory address = {a}')
```
# output
```python
memory address of 257 = 139740170741360
memory address of 258 = 139740170743920
memory address of 259 = 139740170743952
memory address of 260 = 139740170743856
memory address of 261 = 139740170743824
memory address of 262 = 139740170741744
memory address of 263 = 139740170743792
memory address of 264 = 139740170744048
memory address of 265 = 139740170744080
memory address of 266 = 139740170744112
values at that memory address = 139740170743920
values at that memory address = 139740170743952
values at that memory address = 139740170743856
values at that memory address = 139740170743824
values at that memory address = 139740170741744
values at that memory address = 139740170743792
values at that memory address = 139740170744048
values at that memory address = 139740170744080
values at that memory address = 139740170744112
values at that memory address = 266
```
# more explicit info which i got from stackover flow about ranges

A range is not a generator It is a class of a series of positive integers ```int``` inherited from the base class ```object``` in Python, and hence it is not a function or a generator. but if you goo through it's class code you can see ```__iter__``` method, from which you can say that, it has the functionality of iteration and ```for``` loop underneat the hud uses ```iter()``` which is iteration. hence it should be able to iterate range because its not a function its a data type in the sense due to keyword ```class```. 
# code
```python
help(range)
```
# output
```python
C:\Users\Admin\PycharmProjects\pythonProject1\venv\Scripts\python.exe C:/Users/Admin/PycharmProjects/pythonProject1/main.py 
Help on class range in module builtins:

class range(object)
 |  range(stop) -> range object
 |  range(start, stop[, step]) -> range object
 |  
 |  Return an object that produces a sequence of integers from start (inclusive)
 |  to stop (exclusive) by step.  range(i, j) produces i, i+1, i+2, ..., j-1.
 |  start defaults to 0, and stop is omitted!  range(4) produces 0, 1, 2, 3.
 |  These are exactly the valid indices for a list of 4 elements.
 |  When step is given, it specifies the increment (or decrement).
 |  
 |  Methods defined here:
 |  
 |  __bool__(self, /)
 |      self != 0
 |  
 |  __contains__(self, key, /)
 |      Return key in self.
 |  
 |  __eq__(self, value, /)
 |      Return self==value.
 |  
 |  __ge__(self, value, /)
 |      Return self>=value.
 |  
 |  __getattribute__(self, name, /)
 |      Return getattr(self, name).
 |  
 |  __getitem__(self, key, /)
 |      Return self[key].
 |  
 |  __gt__(self, value, /)
 |      Return self>value.
 |  
 |  __hash__(self, /)
 |      Return hash(self).
 |   
 |  __iter__(self, /)
 |      Implement iter(self).
 |  
 |  __le__(self, value, /)
 |      Return self<=value.
 |  
 |  __len__(self, /)
 |      Return len(self).
 |  
 |  __lt__(self, value, /)
 |      Return self<value.
 |  
 |  __ne__(self, value, /)
 |      Return self!=value.
 |  
 |  __reduce__(...)
 |      Helper for pickle.
 |  
 |  __repr__(self, /)
 |      Return repr(self).
 |  
 |  __reversed__(...)
 |      Return a reverse iterator.
 |  
 |  count(...)
 |      rangeobject.count(value) -> integer -- return number of occurrences of value
 |  
 |  index(...)
 |      rangeobject.index(value) -> integer -- return index of value.
 |      Raise ValueError if the value is not present.
 |  
 |  ----------------------------------------------------------------------
 |  Static methods defined here:
 |  
 |  __new__(*args, **kwargs) from builtins.type
 |      Create and return a new object.  See help(type) for accurate signature.
 |  
 |  ----------------------------------------------------------------------
 |  Data descriptors defined here:
 |  
 |  start
 |  
 |  step
 |  
 |  stop


Process finished with exit code 0
```
> ifyou have't found it out in upper code, here it is again too take look at the code.
```python

 |  __iter__(self, /)
 |      Implement iter(self).
```
### if having ```__iter__``` make a object generator then why other data type like ```list, tuple, dict```  and ```set``` are'nt generator even if they have same method used ```__iter__(self)```. ###
night bot
 10 hours ago  

### Yes, that's fair. Only Python functions can be generators, and range is not a Python function. Still, it acts like a generator in that successive values are calculated as needed rather than being calculated when the range is created (which is how it worked in Python2). Evidence for that is easy to collect; you can create a range of arbitrary size, instantly and without exhausting memory. Your edited answer is much more accurate. ###
rici
2 hours ago
 
### I think I got my answer by your confirmation thanks for tha
