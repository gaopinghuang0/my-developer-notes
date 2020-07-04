

## MySQL

## SQLite

## MongoDB

## Redis
  * [Why is Redis so fast? (2019)](https://zhuanlan.zhihu.com/p/57089960)
    * In memory
    * Single thread
    * Efficient data structures
      * String. C string is replaced by Simple Dynamic String (SDS). it has `(int len, int free, char buf[])` to store used bytes, free bytes, and actual string data. 
      * Hashes. It has two hash tables (ht[0] and ht[1]) to support rehash (e.g., increase size of ht[1] and copy from ht[0] to ht[1]). If the number of items is big, use [progressive rehash](https://programmersought.com/article/83551965973/;jsessionid=FE4CEA8AAA9A20393F116B935719D579). For future CRUD, if is rehashing, use 1 millisecond of CPU time to rehash part of the table.
      * Lists and Hashes can be further optimized by ZipList below if they have relatively small number of elements (default 512).
      * Zset. Uses skiplist or ziplist.
  * [Redis HyperLogLog (Apr 2014)](http://antirez.com/news/75) has standard error 0.81% with only 12k bytes for 16k registers. The commands of `PFADD`, `PFCOUNT`, and `PFMERGE` have prefix 'PF', which is in honor of Philippe Flajolet.  [More details of HyperLogLog in Redis (Oct 2019)](https://www.jianshu.com/p/1327d03c8124) with sparse and dense representation.
  * [ZipList in Redis](https://redislabs.com/ebook/part-2-core-concepts/01chapter-9-reducing-memory-use/9-1-short-structures/9-1-1-the-ziplist-representation/). It uses `(len, len, string)` struture to avoid 3 extra pointers (prev, next, and buf) and two integers of `len` and `free`. Here, the `len` in ZipList could even be 1-byte, rather than 4-byte integer.
  * Skip List
  * Command `debug_object(key)` can discover info about the corresponding value, such as the encoding ('ziplist' or 'linkedlist'), and serialized length.

## ElasticSearch
* [ElasticSearch如何实现分布式?](https://github.com/doocs/advanced-java/blob/master/docs/high-concurrency/es-architecture.md)
  * Data representation: `index -> type -> mapping -> document -> field`. Roughly, `index` is similar to a MySQL table, while `type` can also be treated as a table (then `index` is a collection of similar tables). `Mapping` is like the schema of a table. `Document` is like a row, while `field` is a column of a row.
  * To support high concurrency, each `index` can have multiple "shards", while each shard stores part of the data.
  * To support high availability, use primary shard and replica shard. This is similar to Kafka.
* [ElasticSearch工作原理?](https://github.com/doocs/advanced-java/blob/master/docs/high-concurrency/es-write-query-search.md)
  * In terms of Read, `hash` the `doc id` and find the proper `shard`. Then use `round-robin` to randomly find a shard between the primary and replica shards. This is load balancing.
  * In terms of Write,
    * First write data to in-memory `buffer` as well as `translog`. For now, the data is not searchable.
    * Every 1s, `refresh` the `buffer` into `os cache`, clear the `buffer`, and create a new `segment file`. Meanwhile, `inverted index` is created. Now, the data is searchable. Due to this 1s lag, ES is called near real time (NRT). `refresh` can also be called manually. 
    * Since each `refresh` will create a `segment file`, there will be many `segment files`. They will be merged into one `segment file` periodically. Note that the `segment file` is in `os cache`, not in disk yet.
    * Every 5s, the `translog` is written to disk. This prevents data loss in `buffer` or `os cache`. In other words, there will be at most 5s' data loss when the data in `translog` has not been saved to disk yet. Such "async" save mode is changed in ES 5.x, in which save-to-disk is done per request by default.
    * Every 30 mins or if the `translog` is too big, the `segment file` in `os cache` will be `flush`/`fsync` into disk, then ES will clear the old `translog` and create a new one. The whole process is called `commit`.


## LevelDB
* [ ] Read source code of [leveldb by Jeff Dean](https://github.com/google/leveldb). PS: [A chat about why this is worth reading](https://yq.aliyun.com/articles/241361).
