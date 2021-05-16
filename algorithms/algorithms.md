
## Algorithms
* Geohash
 
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
