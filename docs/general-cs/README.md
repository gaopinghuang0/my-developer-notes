# General Computer Science Knowledge

## Data Structures
* [ ] Skip List
* [ ] Zip List
* [ ] Binary Indexed Tree

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

## Design Patterns

## System Design and Architecture
* Google Docs. 
  * Video: [Issues and Experiences in Designing Real-time Collaborative Editing Systems (Nov 2008)](https://www.youtube.com/watch?v=84zqbXUQIHc). Talk about the early version of Google Docs.
  * A three-part series of google drive blogs (Sep 2010): [Working together](https://drive.googleblog.com/2010/09/whats-different-about-new-google-docs_21.html), [conflict resolution](https://drive.googleblog.com/2010/09/whats-different-about-new-google-docs_22.html), and [fast collaboration](https://drive.googleblog.com/2010/09/whats-different-about-new-google-docs.html).
  * [Operational transformation](http://www.codecommit.com/blog/java/understanding-and-applying-operational-transformation).
* Google Maps. 
  * Video: [Google I/O 2013 - Behind the scenes of Google Maps](https://www.youtube.com/watch?time_continue=2230&v=ybXVRYWqN6s). Talking about some optimization of UX.
  * [ ] [A summary of several links about Google Map Architecture (Oct 2015)](https://massivetechinterview.blogspot.com/2015/10/google-map-architecture.html).
  * [Prototyping a Smoother Map (Aug 2018)](https://medium.com/google-design/google-maps-cb0326d165f5).
* Elasticsearch
  * [腾讯万亿级 Elasticsearch 技术解密](https://mp.weixin.qq.com/s/CNf75yT0A0QPki-Qhw3_8w)

## Operating Systems (OS)

## Distributed Systems
* eBook: [Distributed systems for fun and profit (2013)](http://book.mixu.net/distsys/)

## Compilers

## Computer Graphics

## Networks
* My collection of [network topics](./networks.md).