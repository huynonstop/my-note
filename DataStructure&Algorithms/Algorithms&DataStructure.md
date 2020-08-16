# TIme Complexity

![image-20200808195416003](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200808195416003.png)

# Big-O notation

Mô tả cận trên hoặc trường hợp xấu nhất độ phức tạp của một thuật toán tỉ lệ như thế nào khi tăng số lượng phần tử 

O(2N) = O(N) = O(669N)

![image-20200301153930483](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200301153930483.png)

![image-20200301153945953](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200301153945953.png)

```javascript
for (int a : arrA) {
    print(a );
 }
for (int b : arrB) {
   print(b) ;
}
//O(N)
for (int a : arrA) {
    for (int b : arrB) {
    print( a + " , " + b);
   }
}
//O(N^2)
```

<img src="https://www.lavivienpost.com/wp-content/uploads/2018/05/ds-overview.jpg" alt="img" style="zoom:200%;" />

![image-20200301153741625](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200301153741625.png)

# Algorithms

![img](https://www.lavivienpost.com/wp-content/uploads/2018/04/algorithms-cheat-sheet3-768x460.jpg)

# Data structure

## Build-in

![image-20200807220953518](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200807220953518.png)

![image-20200807221116770](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200807221116770.png)

## Custom Data Structure

### Linked List

![image-20200808175311665](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200808175311665.png)

![image-20200808174914030](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200808174914030.png)

![image-20200808195148050](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200808195148050.png)

```js
class Node {
    constructor(value,next = null) {
        this.value = value
        this.next = null
    }
}
class LinkedList {
    constructor() {
        this.head = null
        this.tail = null
    }
    append(value) {
        const newNode = new Node(value)
        if (this.tail) {
            this.tail.next = newNode
        }
        this.tail = newNode
        if (!this.head) {
            this.head = newNode
        }
    }
    prepend(value) {
        const newNode = new Node(value,this.head)
        this.head = newNode
        if (!this.tail) {
            this.tail = newNode
        } 
    }
    delete(value) {
        if(!this.head) {
            return;
        }
        while(this.head && this.head.value === value) {
            this.head = this.head.next
        }
        let node = this.head
        while(node.next) {
            if(node.next.value === value) {
                node.next = node.next.next
            } else {
                node = node.next
            }
        }
        if(this.tail.value === value) {
            this.tail = node
        }
    }
    find(value) {
        if(!this.head) {
            return;
        }
        let node = this.head
        while(node.next) {
            if(node.next.value === value) {
                return node
            } else {
                node = node.next
            }
        }  
        return null
    }
    insertAfter(value,afterValue) {
        const node = this.find(afterValue)
        if(node) {
            const newNode = new Node(value, node.next)
            node.next = newNode
            if(node === this.tail) {
                this.tail = newNode 
            }
        }
    }
    toArray() {
        const array = []
        let node = this.head
        while(node) {
            array.push(node)
            node = node.next
        }
        return array
    }
}
```

#### VS array

![image-20200808195510033](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200808195510033.png)

### Queue

#### Vs linked list

### Stack

#### Vs linked list

### Hash Tables

### Tree

### BST tree

### AVL tree

#### Vs BST

### Tries

#### Vs hash table

### Priority Queues

### Heaps

### Graphs

#### Adj matrix

#### Adj Lists

# More

- More on Algorithms & Data Structures: https://adrianmejia.com/how-you-can-change-the-world-learning-data-structures-algorithms-free-online-course-tutorial/
- Understanding Time Complexity & Big O (incl. Examples): https://adrianmejia.com/most-popular-algorithms-time-complexity-every-programmer-should-know-free-online-tutorial-course/
- More on Data Structures: https://adrianmejia.com/data-structures-time-complexity-for-beginners-arrays-hashmaps-linked-lists-stacks-queues-tutorial/
- A Comprehensive Collection of Examples: https://github.com/trekhleb/javascript-algorithms
- https://pro.academind.com/p/javascript-datastructures-the-fundamentals