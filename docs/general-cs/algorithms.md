
## Algorithms
* Geohash
* Graph algorithms (Dijkstra, ...)
* String searching/matching
  * Rabin-Karp algorithm
  * Boyer-Moore
  * KMP algorithm for pattern searching. [A video about the longest prefix suffix array](https://www.youtube.com/watch?v=tWDUjkMv6Lc&feature=youtu.be).
  * Real use case: [Adblock Plus: investigating filter matching algorithms](https://adblockplus.org/blog/investigating-filter-matching-algorithms)
* Dynamic programming
* [Consistent hashing (Apr 2018)](https://juejin.im/post/5ae1476ef265da0b8d419ef2)
* HyperLogLog. [Theory with code for a coin example (Aug 2016)](https://thoughtbot.com/blog/hyperloglogs-in-redis#the-theory-behind-hyperloglogs). It uses a hash function to uniformly split the items in buckets, then calculate the max number of leading zeros in each bucket, then combine them with a harmonic mean. Besides, [Redis HyperLogLog (Apr 2014)](http://antirez.com/news/75) has standard error 0.81% with only 12k bytes for 16k registers. The command prefix of `PFADD`, `PFCOUNT` is in honor of Philippe Flajolet.
* [RoaringBitmap (Dec 2019)](https://www.jianshu.com/p/818ac4e90daf). Use the most significant 16-bits to assign containers. Numbers with the same most significant 16-bits will be put in the same container. The container type is dependent on the number of elements: an array container can hold up to 4096 items, while a bitmap container can hold a fixed 2^16 items.
* [ ] [Bloom Filter (Apr 2019)](https://www.jianshu.com/p/bef2ec1c361f).
* Others: [Top 10 Algorithms and Data Structures for Competitive Programming](https://www.geeksforgeeks.org/top-algorithms-and-data-structures-for-competitive-programming/)
