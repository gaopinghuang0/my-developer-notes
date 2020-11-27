
## 概率分布
* [泊松分布](https://en.wikipedia.org/wiki/Poisson_distribution)
  * 离散概率分布
  * 适用于描述单位时间内随机事件发生的次数的概率分布。如某一服务设施在一定时间内受到的服务请求的次数，电话交换机接到呼叫的次数、汽车站台的候客人数、机器出现的故障数、自然灾害发生的次数、DNA序列的变异数、放射性原子核的衰变数、激光的光子数分布等等。
  * 期望和方差都是 λ
  * <img width="100" alt="typeof-vs-instanceof" style="margin:auto;" src="./poisson.svg">
  * [ ] 极大似然估计（MLE）
* 二项分布
  * 伯努利分布是二项分布在n = 1时的特殊情况。
  * 当试验的次数趋于无穷大，而乘积np固定时，二项分布收敛于泊松分布。
* 几何分布
  * 有两种分布，不应该混淆：
    * 在伯努利试验中，得到一次成功所需要的试验次数X。X的值域是{ 1, 2, 3, ... }
    * 在得到第一次成功之前所经历的失败次数Y = X − 1。Y的值域是{ 0, 1, 2, 3, ... }
  * 比如，假设不停地掷骰子，直到得到1。投掷次数是随机分布的，取值范围是无穷集合{ 1, 2, 3, ... }，并且是一个p = 1/6的几何分布。


## 极大似然估计

## EM算法（期望最大化算法）
见我总结的 [EM算法](./EM.md)

## 高斯混合模型（GMM）
* [scikit-learn里的高斯混合](https://scikit-learn.org/stable/modules/mixture.html)。
  * 底层实现了EM算法。
  * Gaussian Mixture需要提供 component 的个数，优点是速度最快的。
* 当不提供component个数时，可以用 Variational Bayesian Gaussian Mixture
  * 底层算法依然是EM算法，但基于之前的分布信息来添加正则化。
  * 最重要的参数是`weight_concentration_prior`。当取值小的时候，模型会把大部分权重放在少量的component上；而当取值大的时候，允许更多的component。
  * [ ] 狄利克雷过程(Dirichlet Process），见[这篇文章](https://zhuanlan.zhihu.com/p/76991275)和[这篇](https://www.zhihu.com/question/26751755)。