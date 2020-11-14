

## Fundamentals of Database
* [Stanford CS245: Principles of Data-Intensive Systems](http://web.stanford.edu/class/cs245/)
  * [Column-Oriented Database Systems (SIGMOD '06)](http://web.stanford.edu/class/cs245/readings/c-store-compression.pdf)
  * [Query Execution](http://web.stanford.edu/class/cs245/slides/06-Query-Execution.pdf) with four steps:
    * Query representation (e.g., SQL). One of our key principles in data intensive systems was declarative APIs: Specify what you want to compute, not how.
    * Logical query plan (e.g., relational algebra).  Namely, parse SQL into tree and convert into logical query plan.
      * Relational algebra (RA)
        * Codd's original RA: tables are sets of tuples (unordered and tuples cannot repeat).
        * SQL's RA: tables are bags (multisets) of tuples; unordered but each tuple may repeat.
      * Relational algebra operators
        * Basic set operators: Intersection (R ∩ S), Union (R ∪ S), Difference (R – S), Cartesian Product (R ⨯ S).
          * Cartesian Product of two sets is the set of all possible *ordered pairs* (R ⨯ S = {(r,s): r ∈ R and s ∈ S}). Note that r must be before s.
        * Special query operators:
          * Selection: σ_condition(R) => { r ∈ R | condition(r) is true }
          * Projection: PIE_expressions(R) => { expressions(r) | r ∈ R }
          * Natural Join: R ⨝ S => { (r, s) ∈ R ⨯ S) | r.key = s.key }
          * Aggregation: keys_G_agg(attr)(R) => SELECT agg(attr) FROM R GROUP BY keys
    * Optimized logical plan.
      * Rule-based: apply some rules to replace expressions into other expressions.
      * Cost-based: propose several execution plans and pick best based on a cost model.
      * Adaptive: update execution plan at runtime.
    * Physical plan (i.e., code/operators to run).
      * Interpretation: per-record exec. Walk through query plan operators for each record.
        * Simple, but too slow. Lots of virtual function calls and branches for each record.
        * Transactional database (e.g., MySQL) mostly uses this.
      * Vectorization: walk through in batches.
        * +Faster than per-record exec if the query processes many records.
        * +Relatively simple to implement
        * -Lots of nulls in batches if query is selective
        * -Data travels between CPU & cache a lot
        * Analytical systems (e.g., Vertica, Spark SQL) mostly uses this, sometimes compilation.
      * Compilation: generate code (like System R).
        * Potential to get fastest possible execution
        * Leverage existing work in compilers
        * Complex to implement
        * Compilation takes time
        * Generated code may not match hand-written
        * ML libs (Tensorflow) mostly vectorization (the records are vectors!), sometimes compilation.
  * [Query Execution 2 and Query Optimization](http://web.stanford.edu/class/cs245/slides/07-Query-Optimization-p1.pdf)
    * Common Rule-Based Optimizations
      * Simplifying relational operator graphs (select, project, join, etc) has the most impact.
      * Common rules:
        * push Select as far down as possible so that we can reduce # of records early to minimize work in later ops. For example, if condition p only refers to R, σ_p(R ⨝ S) = σ_p(R) ⨝ S, we can filter R based on p early.
        * push Projects as far down as possible so that we don't process fields that are not necessary
        * However, Project rules may backfire. Therefore, need more info to make good decisions, including data statistics and cost models.
      * Data statistics 
        * Info about the tuples in a relation that can be used to estimate cost & size. Example：
          * T(R) = # of tuples in R
          * S(R) = average size of R's tuples in bytes
          * B(R) = # of blocks to hold all of R's tuples
          * V(R, A) = # distinct values of attribute A in R
          * % of null values for each attribute
        * W = R1⨯R2 => S(W) = S(R1) + S(R2), T(W) = T(R1) x T(R2)
        * W = σ_{A=a}(R) => S(W) = S(R), T(W) = T(R) / V(R, A) or T(W) = T(R) / DOM(R, A)
        * W = σ_{z>=val}(R) => f = fraction of distinct values ≥ val, T(W) = f x T(R)
        * W = σ_{user_defined_func(z)>=val}(R)  => In postgres db, just T(W) = 1/3 x T(R)
      * Example: Spark SQL’s Catalyst optimizer with >500 contributors worldwide, >1000 types of expressions, and hundreds of rules


## MySQL
* [深入理解 MySQL 索引底层原理](https://zhuanlan.zhihu.com/p/113917726)
  * Hash. 支持单个查找，但是不支持范围查找。
  * Binary Search Tree. 支持单个查找和范围查找。但极端情况会退化为链表。
  * AVL 树和红黑树。优点是自平衡，避免了极端情况成为链表。但是数据库查询的瓶颈在于磁盘 IO，如果使用 AVL 树，每个树节点只存储了一个数据，那么比较时就需要一次IO操作。当树很高时，会有很多次 IO 操作，很浪费时间。所以希望树的节点尽可能多地存储数据，然后能在一次磁盘IO里多加载些数据。这就是B树和B+树的优势了。
  * B树。每个节点现在最多存储 k 个 key，一个节点如果超过 k 个 key 就会自动分裂。
  * B+树。跟B树的区别在于，B 树一个节点里存的是数据，而 B+ 树存储的是索引（地址），所以 B+ 树的节点里可以放更多索引，进一步减少 IO。而 B+ 树的叶子节点存所有的数据。另外， B+ 树的叶子节点用了一个链表串联起来，便于范围查找。
  * Innodb 引擎和 MyISAM 引擎。
    * MyISAM 不支持事务处理。Innodb 最大的特色就是支持了 ACID 兼容的事务功能，而且他支持行级锁。
    * MyISAM 引擎把数据和索引分开了，一人一个文件，这叫做非聚集索引方式；Innodb 引擎把数据和索引放在同一个文件里了，这叫做聚集索引方式。
    * MyISAM 的主键和其他字段的索引都指向数据文件里的地址。而Innodb的主键的索引树的叶子节点存储了数据，而其他字段的B+树的叶子节点存的是主键 KEY。这样避免重复保存数据，但也导致了需要查询两次，所以性能比 MyISAM 差点。

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
* [ElasticSearch工作原理?](https://github.com/doocs/advanced-java/blob/master/docs/high-concurrency/es-write-query-search.md). Also this [blog (Apr 2018)](https://ezlippi.com/blog/2018/04/elasticsearch-translog.html) has good diagrams.
  * In terms of Read, `hash` the `doc id` and find the proper `shard`. Then use `round-robin` to randomly find a shard among the primary and replica shards. This is for load balancing.
  * In terms of Write,
    * First write data to in-memory `buffer` as well as `translog`. For now, the data is not searchable.
    * Every 1s, `refresh` the `buffer` into `os cache`, clear the `buffer`, and create a new `segment file`. Meanwhile, `inverted index` is created. Now, the data is searchable. Due to this 1s lag, ES is called near real time (NRT). `refresh` can also be called manually. 
    * Since each `refresh` will create a `segment file`, there will be many `segment files`. They will be merged into one `segment file` periodically. Note that the `segment file` is in `os cache`, not in disk yet.
    * Every 5s, the `translog` is written to disk. This prevents data loss in `buffer` or `os cache`. In other words, there will be at most 5s' data loss when the data in `translog` has not been saved to disk yet. Such "async" save mode is changed in ES 5.x, in which save-to-disk is done per request by default.
    * Every 30 mins or if the `translog` is too big, the `segment file` in `os cache` will be `flush`/`fsync` into disk, then ES will clear the old `translog` and create a new one. The whole process is called `commit`.
* Optimization. See the list in [System Design -> ElasticSearch](../general-cs/system-design-architecture.md)


## LevelDB
* [ ] Read source code of [leveldb by Jeff Dean](https://github.com/google/leveldb). PS: [A chat about why this is worth reading](https://yq.aliyun.com/articles/241361).
