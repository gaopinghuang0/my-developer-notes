## word2vec
* [深入浅出Word2Vec原理解析 - 知乎](https://zhuanlan.zhihu.com/p/114538417)
  * one-hot representation 的优缺点
    * 当词典数较小时，可以很简洁地存储和计算。
    * 随着词典的增大，词向量会越来越大，容易受维度灾难的困扰。
    * 两个相似单词在one-hot representation中没有体现出来，因为只在一个位置取1，其余为0，取1的位置相互独立，看不出相似性。
    * 强稀疏性，0特别多，有用信息很少。
  * distributed representation
    * one-hot representation的词向量中只有一个非零向量，非常集中；而distributed representation的词向量中有大量非零向量，相对分散（有点风险平摊的感觉），把词的信息分布到各个分量中去了。
    * distributed representation一般包括 LSA (Latent Semantic Analysis)、LDA（Latent Dirichlet Allocation）和神经网络。word2vec就属于神经网络中的一种。
    * 一般是一个输入词的one-hot representation乘以一个隐藏权重矩阵得到隐藏层。而这个隐藏的权重矩阵就是词向量。
    * word2vec包含 CBOW（Continuous Bag of Words）和 Skip-gram Model。前者是给定上下文，输出一个单词的概率；后者是给定一个单词，预测上下文单词的概率。
    * 在计算中，多个词的softmax会求和，计算量很大。word2vec提了两个方法进行优化。
      * Hierarchical softmax。将词典的词按照词频进行Huffman 编码，这样高频词距离根节点更近，低频词距离根节点远。从根节点开始一层层的预测（二分类，采用sigmoid函数）。这样，越低频的词得到的训练就越多，越高频的词得到的训练少，大大减低了训练量，同时也让低频词的训练效果更好。
      * [ ] Negative Sampling。还没仔细看。