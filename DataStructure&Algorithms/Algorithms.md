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

# Algorithms

![img](https://www.lavivienpost.com/wp-content/uploads/2018/04/algorithms-cheat-sheet3-768x460.jpg)

