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

More on Algorithms & Data Structures: https://adrianmejia.com/how-you-can-change-the-world-learning-data-structures-algorithms-free-online-course-tutorial/

Understanding Time Complexity & Big O (incl. Examples): https://adrianmejia.com/most-popular-algorithms-time-complexity-every-programmer-should-know-free-online-tutorial-course/

https://pro.academind.com/p/javascript-datastructures-the-fundamentals

More on Data Structures: https://adrianmejia.com/data-structures-time-complexity-for-beginners-arrays-hashmaps-linked-lists-stacks-queues-tutorial/

A Comprehensive Collection of Examples: https://github.com/trekhleb/javascript-algorithms

Introduction:

https://www.geeksforgeeks.org/data-structures/https://www.tutorialspoint.com/data_structures_algorithms/data_structures_basics.htm

Measuring Efficiency with BigO Notation

https://www.bigocheatsheet.com/https://medium.com/@binyamin/data-structures-and-big-o-notation-ec7ac060f186

https://www.101computing.net/big-o-notation/

https://hackernoon.com/what-does-the-time-complexity-o-log-n-actually-mean-45f94bb5bfbf

The Array:

https://www.geeksforgeeks.org/introduction-to-arrays/

https://www.w3schools.com/python/python_arrays.asp

https://docs.oracle.com/javase/tutorial/java/nutsandbolts/arrays.html

https://www.bigocheatsheet.com/

https://processing.org/tutorials/2darray/

https://www.faceprep.in/data-structures/advantages-and-disadvantages-of-arrays/
The ArrayList:

https://www.w3schools.com/java/java_arraylist.asp

https://www.w3schools.com/python/python_ref_list.asp

https://courses.cs.washington.edu/courses/cse341/98au/java/jdk1.2beta4/docs/api/java/util/ArrayList.html

https://www.programiz.com/python-programming/methods/list

https://www.tutorialspoint.com/java/java_arraylist_class.htm

https://www.w3resource.com/java-tutorial/arraylist/arraylist_object-toarray.php

https://stackoverflow.com/questions/23245386/how-does-memory-allocation-of-an-arraylist-work/23245487

https://www.geeksforgeeks.org/array-vs-arraylist-in-java/

The Stack:

https://www.callicoder.com/java-stack/

https://www.geeksforgeeks.org/stack-in-python/

https://docs.microsoft.com/en-us/dotnet/api/system.collections.stack?view=netcore-3.1

https://www.geeksforgeeks.org/stack-push-and-pop-in-c-stl/

https://www.tutorialsteacher.com/csharp/csharp-stack#contains

https://www.bigocheatsheet.com/https://www.quora.com/What-are-the-real-life-applications-of-stack-data-structure

The Queue:

https://www.geeksforgeeks.org/how-to-create-a-queue-in-c-sharp

https://www.javatpoint.com/java-priorityqueue

https://docs.oracle.com/javase/7/docs/api/java/util/Queue.html

https://www.geeksforgeeks.org/queue-interface-java/

https://www.quora.com/What-are-some-real-world-applications-of-a-queue-data-structure-1

https://www.studytonight.com/data-structures/queue-data-structure

The LinkedList:

https://stackoverflow.com/questions/644167/what-is-a-practical-real-world-example-of-the-linked-list

https://medium.com/future-vision/data-structures-algorithms-linked-lists-fc0b8a82d609 

https://www.geeksforgeeks.org/linked-list-set-3-deleting-node/

[https://www.interviewbit.com/courses/programming/topics/linked-lists](https://www.interviewbit.com/courses/programming/topics/linked-lists/#:~:text=A linked list is a,has a reference to null.)

https://docs.oracle.com/javase/7/docs/api/java/util/LinkedList.html

[https://techdifferences.com/difference-between-linear-and-non-linear-data-structure.html](https://techdifferences.com/difference-between-linear-and-non-linear-data-structure.html#:~:text=In the linear data structure%2C the data is organized in,linked one after the other.&text=Examples of the linear data,the non-linear data structure.)

https://realpython.com/linked-lists-python/https://www.tutorialspoint.com/python_data_structure/python_linked_lists.htm

https://www.geeksforgeeks.org/linked-list-set-1-introduction/

https://www.geeksforgeeks.org/data-structures/linked-list/

The Doubly-LinkedList:

[https://codeforwin.org/2015/10/doubly-linked-list-data-structure-in-c.html](https://codeforwin.org/2015/10/doubly-linked-list-data-structure-in-c.html#:~:text=Doubly linked list can be,implement Undo and Redo functionality.)

https://www.geeksforgeeks.org/doubly-linked-list/

Dictionaries + Hash Tables:

https://computersciencewiki.org/index.php/Dictionaries

[https://en.wikibooks.org/wiki/A-level_Computing/AQA/Paper_1/Fundamentals_of_data_structures/Dictionaries](https://en.wikibooks.org/wiki/A-level_Computing/AQA/Paper_1/Fundamentals_of_data_structures/Dictionaries#:~:text=A dictionary is a general,has a single associated value.&text=Different languages enforce different type,often implemented as hash tables.)

https://realpython.com/python-dicts/

https://www.geeksforgeeks.org/python-dictionary/

[https://brilliant.org/wiki/associative-arrays/#:~:text=Associative%20arrays%2C%20also%20called%20maps,a%20list%20of%20phone%20numbers.](https://brilliant.org/wiki/associative-arrays/#:~:text=Associative arrays%2C also called maps,a list of phone numbers.)

https://towardsdatascience.com/hash-tables-explained-5dc457db50da 

[https://www.hackerearth.com/practice/data-structures/hash-tables/basics-of-hash-tables/tutorial/](https://www.hackerearth.com/practice/data-structures/hash-tables/basics-of-hash-tables/tutorial/#:~:text=A hash table is a,function%2C hashing can work well.)

https://en.wikipedia.org/wiki/Associative_arrayhttps://en.wikipedia.org/wiki/Hash_functionhttps://en.wikipedia.org/wiki/Hash_table

Trees:

https://practice.geeksforgeeks.org/problems/advantages-and-disadvantages-of-bsthttps://www.codesdope.com/blog/article/trees-in-computer-science/https://www.quora.com/What-are-the-types-of-trees-in-data-structureshttps://www.quora.com/Who-invented-data-structures-like-stack-queues-and-linked-listshttp://pages.cs.wisc.edu/~vernon/cs367/notes/8.TREES.htmlhttps://computersciencewiki.org/index.php/Treehttps://www.freecodecamp.org/news/all-you-need-to-know-about-tree-data-structures-bceacb85490c/https://medium.com/brandons-computer-science-notes/trees-the-data-structure-e3cb5aabfee9

Tries:

https://www.interviewcake.com/concept/java/trie 

https://medium.com/basecs/trying-to-understand-tries-3ec6bede0014 ‘

Heaps:

[https://brilliant.org/wiki/heaps/](https://brilliant.org/wiki/heaps/#:~:text=Heaps are used in many,or minimum element very quickly.)https://brilliant.org/wiki/priority-queues/

https://www.geeksforgeeks.org/heap-sort/

https://medium.com/@randerson112358/max-heap-deletion-step-by-step-f402861523da

https://medium.com/@randerson112358/lets-build-a-max-heap-161d676394e

Graphs:

https://medium.com/@BennettGarner/what-the-graph-a-beginners-simple-intro-to-graphs-in-computer-science-3808d542a0e5

http://web.cecs.pdx.edu/~sheard/course/Cs163/Doc/Graphs.html

https://www.geeksforgeeks.org/graph-data-structure-and-algorithms/

https://www.khanacademy.org/computing/computer-science/algorithms/graph-representation/a/describing-graphs

https://softwareengineering.stackexchange.com/questions/168058/what-are-graphs-in-laymens-terms

http://think-like-a-git.net/sections/graph-theory/directed-versus-undirected-graphs.html

https://www.baeldung.com/cs/graphs-directed-vs-undirected-graph

https://www.quora.com/Whats-the-difference-between-a-cyclic-and-an-acyclic-graph

https://en.wikipedia.org/wiki/Directed_acyclic_graph

http://courses.cs.vt.edu/~cs3114/Fall10/Notes/T22.WeightedGraphs.pdf