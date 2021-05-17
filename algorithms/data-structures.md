
## Data Structures
* [Skip List](https://en.wikipedia.org/wiki/Skip_list). 

* [Binary Indexed Tree (BIT)](https://medium.com/@adityakumar_98609/fenwick-tree-binary-index-tree-aca7824d9c2a). This blog also includes BIT for 2D array. Compared to Segment Tree, BIT requires less space and is easier to implement.  BIT(idx) uses 1-based index (`idx`). Then `idx & (-idx)` will get the least significant set bit (LSB). For example, 6 in binary is 110, whose LSB is at the second to the rightmost bit. Similarly, 5 in binary is 101 whose LSB is the rightmost bit. When getting the range sum of idx, keep adding `BIT(idx)` and removing the LSB from idx until idx is 0. When updating the value at idx, keep adding the LSB until greater than size of array.

* 线段树 [Segment Tree](https://www.geeksforgeeks.org/segment-tree-set-1-sum-of-given-range/) 和 [这篇](https://zhuanlan.zhihu.com/p/261880675).  
  * It is more flexible than BIT. It is able to retrieve the sum, max, min, gcd, or other stuff of the elements between `(l, r)`.
  * 递归实现

* [李超线段树](https://zhuanlan.zhihu.com/p/64946571)，可以用来处理这个问题：给定一个平面直角坐标系，支持动态插入一条线段，询问从某一个位置（x，正无穷）向下看能看到的最高的一条线段（也就是给一条竖线，问这条竖线与所有线段的最高的交点）

* Trie and Radix Tree (or Radix Trie) and Suffix Tree.