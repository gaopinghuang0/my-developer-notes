# Computer Vision

## Introduction to CV
[Course on Udacity 810](https://classroom.udacity.com/courses/ud810) by Aaron Bobick. The problem sets can be found in the [ CS4495 Spring 2015](https://www.cc.gatech.edu/~afb/classes/CS4495-Spring2015-OMS/) from Georgia Tech.
* [ ] I have created a Github repo for the problem sets. I have only done the first problem set. Revisit sometime later.
* [Smoothing and Blurring](https://classroom.udacity.com/courses/ud810/lessons/3490398569/concepts/35009385470923)
  * To blur an image, we can compute convolution of a Gaussian function with the original image.
  * Since the Fourier Transform of a convolution is actually the multiplication of individual Fourier Transform, a fat Gaussian that is supposed to blur the image heavily has a Fourier Transform that almost only contains the low-frequency part.
  * g(x) = f(x) * h(x)  => G(u) = F(u)H(u). Here, * means convolution.
  * In other words, these two concepts are connected in this way. Fourier Transform explains what happened in the frequency domain while doing convolution.

## Feature tracking and plane detection
* [ ] [Good features to track (1993)](https://users.cs.duke.edu/~tomasi/papers/shi/TR_93-1399_Cornell.pdf)
* [ ] [SIFT (Scale Invariant Feature Transform)](https://link.springer.com/article/10.1023/B:VISI.0000029664.99615.94) 2004 by David G. Lowe.
* [ ] [SURF (Speeded Up Robust Features)](https://link.springer.com/chapter/10.1007/11744023_32) 2006 by H. Bay et al.
* [ ] [BRISK (Binary Robust Invariant Scalable Keypoints)](https://www.research-collection.ethz.ch/bitstream/handle/20.500.11850/43288/eth-7684-01.pdf?sequence=1) 2011. It can provide better overall performance than SIFT and SURF. It is often based on [FAST algorithm (2006)](https://link.springer.com/chapter/10.1007/11744023_34) by Rosten and Drummond.
  * FAST uses the circular surroundings of each pixel `p` (e.g., 16 pixels in a circle around `p`) to detect a corner. If a certain number of connected pixels are brighter or darker than the central pixel `p`, then the algorithm found a corner.
  * In BRISK, at least 9 consecutive pixels in the 16-pixel circle must be sufficiently brighter or darker than the central pixel `p`. In addition, BRISK also uses down-sized images (scale-space pyramid) to achieve better invariance to scale.
  * *Keypoints*: the algorithm must find the feature again in a different image (with different perspective, lightning, etc). BRISK descriptor is a binary string with 512 bits, which is a concatenation of brightness comparison results between different samples surrouding the center keypoint.
* A blog post: [Basics of AR: Anchors, Keypoints & Feature Detection (Aug 2018)](https://www.andreasjakl.com/basics-of-ar-anchors-keypoints-feature-detection/) describes a little more about BRISK.
  * It also includes code of Python + OpenCV to visualize the feature points in an image.

## Optical flow
* [光流法(optical flow)简介](https://blog.csdn.net/qq_41368247/article/details/82562165)
  * 约束方程只有一个，而方程的未知量有两个。
  * 有不同种类的求解方法
    * 基于梯度（微分）的方法：Horn-Schunck算法与Lucas-Kanade(LK)算法；
    * 基于匹配的方法、
    * 基于能量（频率）的方法、
    * 基于相位的方法
    * 神经动力学方法。
* 基本假设
  * 亮度恒定不变。即同一目标在不同帧间运动时，其亮度不会发生改变。这是基本光流法的假定（所有光流法变种都必须满足），用于得到光流法基本方程；
  * 时间连续或运动是“小运动”。即时间的变化不会引起目标位置的剧烈变化，相邻帧之间位移要比较小。同样也是光流法不可或缺的假定。
* [Lucas-Kanade 算法公式推导](http://image.sciencenet.cn/olddata/kexue.com.cn/upload/blog/file/2010/9/2010929122517964628.pdf)
  * LK光流法在原先的光流法两个基本假设的基础上，增加了一个“空间一致”的假设，即所有的相邻像素有相似的行动。也即在目标像素周围m×m的区域内，每个像素均拥有相同的光流矢量。这个条件使得约束方程可以解。
  * 这里考虑的是仿射变换
  * 梯度下降求模板与采样的最小误差
  * [最小二乘法](https://www.cnblogs.com/AndyJee/p/5053354.html) 和[（伪逆矩阵，左逆）](https://www.cnblogs.com/AndyJee/p/5082508.html)
  * 副产品是Hessian 矩阵
* Lucas-Kanade 算法公式在物体运动快的情况下，“空间一致”的假设不成立。上面[光流法(optical flow)简介](https://blog.csdn.net/qq_41368247/article/details/82562165)里提到一种**金字塔LK光流法**。
  * 基本想法是缩小图像的尺寸，让大范围的运动变成了小范围的，强行满足“小运动”的假设。
  * 每一帧建立一个高斯金字塔，最低分辨率图像在最顶层,原始图片在底层。
  * 顶层的光流计算结果(位移情况)反馈到第Lm-1层

## 相机标定
* [相机标定究竟在标定什么？(2017)](https://zhuanlan.zhihu.com/p/30813733)
  * 通常我们用相机把三维物体变成二维的图片，这个过程是不可逆的。相机标定的目的是找到一个相机的近似模型，既可以把三维物体变成二维图片，也可以求出反函数，用来重建三维物体。
  * 最粗糙的近似是把相机近似成小孔成像模型。利用相似三角形和成像原理，可以得到物点在相机坐标系里的坐标与像点在CCD坐标系上的坐标的关系，一个矩阵变换。
  * 如果考虑相机畸变，还要添加畸变参数。
  * 使用标定板（例如棋盘格和圆点阵列），在不同的角度下采样，加上最小二乘法等等方法，可以求出这个模型的参数。标定板有两个作用，一是方便得出物点和像点之间的一一对应关系，毕竟虽然成像发生了畸变，但相对的拓扑结构没变，比如棋盘的直角在成像后也能找到对应的交叉点。二是标定板的物理尺寸已知，可以得到物点在相机坐标系下的坐标。这要求标定板的平面很平，而且网格的尺寸要一致。