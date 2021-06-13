https://www.youtube.com/watch?v=kM4oZTJaO8k

https://academind.com/learn/python/

https://scrimba.com/learn/python

# Conditional statement

```python
if x < y:
    print("x is less than y")
elif x > y:
    print("x is greater than y")
else:
    print("x is equal to y")
```



# Loops

```python
while True:
    print("hi")
    
#--------------------------
i = 0
while i < 3:
    print("hi",i)
    i += 1
    
#--------------------------
for i in [0,1,2]:
    print("hi",i)
    
for i in range(3):
    #range(3) return [0,1,2]
    print("hi",i)
```

## range

```python
>>> list(range(10))
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
>>> list(range(1, 11))
[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
>>> list(range(0, 30, 5))
[0, 5, 10, 15, 20, 25]
>>> list(range(0, 10, 3))
[0, 3, 6, 9]
>>> list(range(0, -10, -1))
[0, -1, -2, -3, -4, -5, -6, -7, -8, -9]
>>> list(range(0))
[]
>>> list(range(1, 0))
[]
```

- `start`

  The value of the *start* parameter (or `0` if the parameter was not supplied)

- `stop`

  The value of the *stop* parameter

- `step`

  The value of the *step* parameter (or `1` if the parameter was not supplied)

# Datatype

integer, float, boolean, string, bytes

```python
str()
chr() <-> ord()
repr()
int()
float()
round(number,decimal)
bool()
bytes()

list()
dict()
set()
```

# Sequence type

range

list

tuple

dict

set

# Interpreter

translate python => machine code on the fly

# Scope

a variable will exist until the end of the function

```js
def get_pos_int():
	while True:
    	n = int(input("Positive interger: "))
		if n>0:
        	break;
	return n
```

# argv

![image-20201016215141605](assets/python/image-20201016215141605.png)

# swap

```python
x = 1
y = 2

x,y = y,x
```

# csv

```python
import csv
file = open("phonebook.csv","a")
name = input("Name: ")
number = input("Number: ")
writer = csv.writer(file)

writer.writerow([name,number])
file.close()

#-----------------------------
with open("phonebook.csv","a") as file:
    name = input("Name: ")
    number = input("Number: ")
    writer = csv.writer(file)
    writer.writerow([name,number])
    
#-----------------------------
reader = csv.reader(file)
next(reader)
for row in reader:
    print(row)
```

