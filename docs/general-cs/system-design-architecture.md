
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
  * [腾讯万亿级 Elasticsearch 技术解密 (Dec 2019)](https://mp.weixin.qq.com/s/CNf75yT0A0QPki-Qhw3_8w). Mentioned a lot of real challenges, but the solutions are very high-level.
  * [基于 Kafka 和 ElasticSearch，LinkedIn是如何构建实时日志分析系统的？(Jan 2017)](https://www.sohu.com/a/123749588_355140)
  * [ES 在数据量很大的情况下（数十亿级别）如何提高查询效率？](https://github.com/doocs/advanced-java/blob/master/docs/high-concurrency/es-optimizing-query-performance.md)
    * Cache. Use `filesystem cache` to store as many `idx segment file` as possible. Then the search will be done in memory, which is much faster than from disk.
    * Data Partition. Due to the memory size limit, it is impossible to load all data into cache. One way is to only save some key fields (e.g., `id,name,age`) in ElasticSearch while other extra fields in MySQL/HBase. In such case, first use ES to find the desired `doc id`s based on the key fields, then use `doc id`s to find other data from MySQL/HBase.
    * Data Warmup. Even if we do the above partition, it is possible that the data is more than twice of the cache size. Then we can do some pre-warmup by periodically search some popular data, which will be stored in the cache for later use.
    * Hot-warm-cold architecture. Use one index to store hot data, while another index to store cold data. Then ES can keep most hot data in cache after warmup and the hot data will not be replaced by cold data.
    * `document` design. Unlike MySQL, do not use complex search in ES, such as `join`, `nested`.... Instead, carefully design the `document` (`mapping`), try to do join operation in Java, and save the result directly into ES. Then we can search without doing join.