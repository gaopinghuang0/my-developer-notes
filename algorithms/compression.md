
## Video compression (MPEG, H.264)
* [H.264视频压缩算法](https://www.cnblogs.com/pjl1119/p/9914861.html)和[Everything You Should Know about H.264/AVC](https://www.videoproc.com/resource/h264-codec.htm).基本原理如下：
  * 划分宏块：H264默认是使用 16X16 大小的区域作为一个宏块。
  * 划分好宏块后，计算宏块的象素值。
  * 划分子块：为了更高的压缩率，还可以在 16X16 的宏块上更划分出更小的子块。
  * 帧分组：主要利用时间上的数据冗余和空间上的数据冗余
    * I-frame contains the complete information of the image, it is coded independently of other non-I-frame pictures.
    * P-frame contains differences relative to preceding frames.
    * B-frame contains differences relative to both preceding and following frames.
    * GOP (Group of Pictures) is made of I, P, B frames starting with I frame. A typical structure could be IBBPBBPBBI or IBBPBBPBBPBBI. The more frames between I-frames, the longer the GOP.
    * 帧间预测：运动估计与补偿，算出运动矢量。
    * 帧内预测：H264的帧内压缩与JPEG很相似。一幅图像被划分好宏块后，对每个宏块可以进行 9 种模式的预测。找出与原图最接近的一种预测模式。
  * [DCT (Discrete Cosine Transform) 变换](https://blog.51cto.com/u_7335580/2066650)：从空域变换到频域的一个矩阵。左上角的低频系数中（图像变化不多的区域，人眼对低频部分更敏感）部分集中了大量能量，右下角的高频系数（图像变化剧烈的部分或边缘，人眼对高频部分不敏感）集中了很少的能量，处理为这样的结果实际上为后续的量化和Zig-Zag扫描做了很好的铺垫。有损压缩。
    * 离散傅里叶变换需要进行复数运算，尽管有FFT可以提高运算速度，但在图像编码、特别是在实时处理中非常不便。离散傅里叶变换在实际的图像通信系统中很少使用，但它具有理论的指导意义。根据离散傅里叶变换的性质，实偶函数的傅里叶变换只含实的余弦项，因此构造了一种实数域的变换——离散余弦变换(DCT)。这是由于离散余弦变换具有很强的"能量集中"特性:大多数的自然信号(包括声音和图像)的能量都集中在离散余弦变换后的低频部分。
  * [量化(Quantization)](https://blog.51cto.com/u_7335580/2067118)。把前一步的频域矩阵除以一个量化步长，然后取整。这样，很多小的值都取0了，实现进一步压缩。这个量化步长越大，得到的0越多，画质就越低，反之，步长越小画质越高。有损压缩。
  * Deblocking filter: Since H.264/AVC is block-oriented, the compressed video is composed of square blocks that might result in severe artifacts. Its adaptive in-loop deblocking filter comes to smooth the edges between pixel blocks. This filter is adaptive on three levels – slice level, block edge level, and sample level.
  * [之字扫描（ZigZag扫描）](https://blog.51cto.com/u_7335580/2068407)：二维到一维。经过量化，非零值基本集中在矩阵的左上角，经过ZigZag扫描之后，将二维的矩阵变为一个一维的串以后，最前边的便是非零值，靠后边的便是较多的零值；方便后续的熵编码。
  * 熵编码（Entropy coding），分为CAVLC 压缩和CABAC 压缩，见下文。

* [CAVLC 压缩](https://blog.csdn.net/jubincn/article/details/6948334)：基于上下文的自适应可变长编码。概况来说就是各种查表得到不同的编码。本文含有一个例子。
  * 计算非零系数（TotalCoeffs）和拖尾系数（TrailingOnes）的数目；
  * 通过考虑左边的宏块和上方的宏块来计算出自身的 nC（numberCurrent）
  * 查表获得coff_token的编码
  * 编码每个拖尾系数的符号，按zig-zag的逆序进行编码
  * 编码除拖尾系数之外非零系数的level（Levels）。这里其实讲得很不清楚。
  * 编码最后一个非零系数之前0的个数
  * 编码每个系数前面0的数目

* [ ] [CABAC 压缩](https://www.cnblogs.com/TaigaCon/p/5304563.html)：无损压缩。CABAC在不同的上下文环境中使用不同的概率模型来编码。其编码过程大致是这样：首先，将欲编码的符号用二进制bit表示；然后对于每个bit，编码器选择一个合适的概率模型，并通过相邻元素的信息来优化这个概率模型；最后，使用算术编码压缩数据。算术编码采用的并非传统的单符号映射模式进行编码，它不是将单个符号映射成一个码字，而是从全序列出发，将输入的符号依据它的概率映射到[0,1)内的一个小区间上，如此递归地进行区间映射，最后得到一个小区间，从该区间内选取一个代表性的小数作为实际的编码输出。
  * 二值化。CABAC使用的算术编码是基于二进制的算术编码，因此非二进制形式的编码首先要转化为二进制的形式表示。
  * 选择上下文模型。“上下文模型”是指对二值化后的符号中的bit位进行编码时使用的概率模型。概率模型与最近编码的符号相关，会有多个概率模型可供选择。
  * 算术编码。算术编码器根据第2步选择的概率模型对每个bit进行编码。需要注意的是每个bit的子范围只有两个数：0和1。
  * 更新预测模型。根据实际编码的值来更新所选择的预测模型。例如，如果所编码的二进制bit为1,则预测模型中的1计数要增加。
  * [ ] 具体的挺复杂的，没特别仔细看。但这篇文章讲得非常细。