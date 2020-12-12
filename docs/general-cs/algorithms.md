
## Algorithms
* Geohash
* Graph algorithms (Dijkstra, ...)
  * Tarjan's algorithm to find [Strongly Connected Components](https://www.geeksforgeeks.org/tarjan-algorithm-find-strongly-connected-components/), [articulation point](https://www.geeksforgeeks.org/articulation-points-or-cut-vertices-in-a-graph/), [bridge](https://www.geeksforgeeks.org/bridge-in-a-graph/), biconnected component. Note that in the implementation, suppose we are currently at node u, if a node v is not visited, then `low[u] = min(low[u], low[v])`; however, if node v is visited, then `low[u] = min(low[u], disc[v])`. Note, in the latter part, we must use `disc[v]`, not `low[v]`. Here is an example to explain why using `low[v]` is wrong. Suppose nodes [1,2,3,4] form a cycle and node 1 is visited first. If `low[v]` is used, then all nodes have the same `low` value (i.e., low[4] = low[3] = low[2] = low[1]). Suppose nodes [4,5,6] form another cycle. Then all nodes [4,5,6] will also have low[5] = low[6] = low[4] = low[1]. In such case, **there will be no articulation point**. However, if using `disc[v]`, then low[5] = low[6] = desc[4], which is different from low[1].
  * [Strongly connected component (Kosaraju's algorithm)](https://www.geeksforgeeks.org/strongly-connected-components).
    * Create an empty Stack `'S'` and do DFS on the graph. Push a vertex to stack after visiting all its neighbors. The vertices in the `'S'` are sorted by their finish time.  Essentially, this is a topological sort.
    * Create a reverse graph.
    * Pop vertex from `'S'` one by one and do DFS on the reversed graph. Save into `who[u]` array. The vertices in one SCC will have the same `who[u]`. The key is that DFS on the top of stack `'S'` is always starting from the sink of the reversed graph. After visiting all nodes of one SCC, the top of stack is the new sink.
* String searching/matching
  * Rabin-Karp algorithm, which uses hashing to find an exact match of a pattern string in a text. The key is to use a rolling hash, which can compute new hash value based on previous one. A good rolling hash function is Rabin fingerprint. [Wikipedia](https://en.wikipedia.org/wiki/Rabin%E2%80%93Karp_algorithm) uses a polynomial approach, which is simpler and equally good. A practical application of the algorithm is detecting plagiarism. Another example is [Leetcode 1044. Longest Duplicate Substring](https://leetcode.com/problems/longest-duplicate-substring/).
  * Boyer-Moore
  * KMP algorithm for pattern searching. [A video about the longest prefix suffix array](https://www.youtube.com/watch?v=tWDUjkMv6Lc&feature=youtu.be).
  * Real use case: [Adblock Plus: investigating filter matching algorithms](https://adblockplus.org/blog/investigating-filter-matching-algorithms)
* Dynamic programming
* [Consistent hashing (Apr 2018)](https://juejin.im/post/5ae1476ef265da0b8d419ef2)
* HyperLogLog. [Theory with code for a coin example (Aug 2016)](https://thoughtbot.com/blog/hyperloglogs-in-redis#the-theory-behind-hyperloglogs). It uses a hash function to uniformly split the items in buckets, then calculate the max number of leading zeros in each bucket, then combine them with a harmonic mean. Besides, [Redis HyperLogLog (Apr 2014)](http://antirez.com/news/75) has standard error 0.81% with only 12k bytes for 16k registers. The command prefix of `PFADD`, `PFCOUNT` is in honor of Philippe Flajolet.
* [RoaringBitmap (Dec 2019)](https://www.jianshu.com/p/818ac4e90daf). Use the most significant 16-bits to assign containers. Numbers with the same most significant 16-bits will be put in the same container. The container type is dependent on the number of elements: an array container can hold up to 4096 items, while a bitmap container can hold a fixed 2^16 items.
* [Bloom Filter (Apr 2019)](https://www.jianshu.com/p/bef2ec1c361f). Core idea: A bit array with m bits + k hash functions. An element will first compute k hash values respectively, and set the corresponding positions of the bit array as 1. To check if an element is existing, check all k positions. If there is at least one 0, then the element does not exist; otherwise it may exist. There will be false positive, but no false negative. The blog also walks through the implementation of Bloom Filter in Guava.
* [CP 匹配 (Gale-Shapley算法)](https://zhuanlan.zhihu.com/p/171718378)。在N男N女相亲活动中，每个男生和女生都对异性有个心里评价，甚至优先级排名。设计一个算法，将这N男N女组成稳定的CP。
  * 解法：第一轮我们让所有的男生都去追求自己最心仪的女生，那么就会出现多个男生同时追求同一名女生的情况，这里我们做一个非常简单的假设，假设女生始终会选择在自己列表上排名高的那一个男生作为自己的CP。经过一系列竞争，必然会有一些男生成功的组成了CP。第二轮，我们让单身的男生再去追求自己第二喜欢的女生，经过一轮竞争，又有一些人脱单了（但也有些之前组成CP的男生重新单身！）。我们如此循环往复，直到所有的人都配对。代码实现见文章。
  * 用反证法可证这种匹配是稳定的，而且不会有人没有配对成功的情况。假设女生只有一个人追求的时候必定组成CP。
  * 也可以用二分图匹配来解决。
  * 注意：这个算法对男生是有优势的。因为女生只能被动等待男生发起追求，而没有能力追求自己最喜欢的男生。
* Others: [Top 10 Algorithms and Data Structures for Competitive Programming](https://www.geeksforgeeks.org/top-algorithms-and-data-structures-for-competitive-programming/)
