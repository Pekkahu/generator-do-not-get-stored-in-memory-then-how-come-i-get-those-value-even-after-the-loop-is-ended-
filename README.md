# generator do not get stored in memory then how come i get those value even after the loop is ended
A loop is created for generating a series of number
```
import ctypes

g = [ ]

for item1 in range(10):
    print(f'memory address of {item1} = {id(item1)}')
    g.append(id(item1))
```
>here my loops end but the only thing is stored is the location or the memory address not the number

>checking the values at that memory address
```
for item in g:
    a = ctypes.cast(item, ctypes.py_object).value
    print(f'values at that memory address = {a}')
```
> here <br>
a = ctypes.cast(item, ctypes.py_object).value <br> 
gives the value stored at that memory address <br>

# output
```
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
```
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
