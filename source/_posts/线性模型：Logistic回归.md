---
title: 机器学习算法：Logistic回归
date: 2019-01-28 00:30:07
tags:
    - 机器学习
    - 算法
    - 逻辑回归
categories:
    - 机器学习
mathjax: true
---

## Logistic回归

- **Logistic回归算法解决了什么问题？公式表示是什么？**

    Logistic回归事实上是一种分类算法，因为历史原因起名叫“回归”这两个字可能有点疑惑，但是它是种分类算法。

    以肿瘤是否为恶性来说：

    ![逻辑回归-肿瘤-1.png](https://i.loli.net/2019/01/28/5c4de0563df00.png)

    1：是恶性肿瘤

    0：是良性肿瘤

    无论什么情况这个分类问题总是有：$0\leq h_{\theta}(x) \leq 1$

    这个算法的性质是：**它的输出值永远在0到 1 之间。**

    所以，我们引入一个新的模型：**逻辑回归模型（Logistic Regression）**

    逻辑回归模型的假设是：
    
     $h_\theta \left( x \right)=g\left(\theta^{T}X \right)$ 
     
     其中： $X$ 代表特征向量 $g$ 代表逻辑函数（logistic function)是一个常用的逻辑函数为S形函数（Sigmoid function），公式为： $g\left( z \right)=\frac{1} {1+{ {e}^{-z} } }$。

    S形函数就是为了将回归模型函数的值域限制在$(0,1)$之间。

    ![逻辑回归公式.png](https://i.loli.net/2019/01/28/5c4de05611f62.png)

- **判定边界是什么？**

    **判定边界就是逻辑回归要求的最终目标。**

    在逻辑回归中，我们预测：

    当${h_\theta}\left( x \right)>=0.5$时，预测 $y=1$。

    当${h_\theta}\left( x \right)<0.5$时，预测 $y=0$ 。

    根据上面绘制出的 S 形函数图像，我们知道当

    $z=0$ 时 $g(z)=0.5$

    $z>0$ 时 $g(z)>0.5$

    $z<0$ 时 $g(z)<0.5$

    又 $z={\theta^{T} }x$ ，即： ${\theta^{T} }x>=0$ 时，预测 $y=1$ ${\theta^{T} }x<0$ 时，预测 $y=0$



    如下图，黄色线就是决策边界：

    线性判定边界：

    ![逻辑回归-决策边界1.png](https://i.loli.net/2019/01/28/5c4de0565cae5.png)

    非线性判定边界：

    ![逻辑回归-决策边界2.png](https://i.loli.net/2019/01/28/5c4de05655765.png)

- **逻辑回归的代价函数是什么？可以简化么？**

    线性回归的代价函数为：$J\left( \theta \right)=\frac{1}{m}\sum\limits_{i=1}^{m}{\frac{1}{2}{ {\left( {h_\theta}\left({x}^{\left( i \right)} \right)-{y}^{\left( i \right)} \right)}^{2} } }$ 。
    
    逻辑回归的代价函数为：

    ![逻辑回归-代价函数.png](https://i.loli.net/2019/01/28/5c4de0562d331.png)

    ${h_\theta}\left( x \right)$与 $Cost\left( {h_\theta}\left( x \right),y \right)$之间的关系如下图所示：

    ![逻辑回归-代价函数图像.png](https://i.loli.net/2019/01/28/5c4de05657f02.png)

    这样构建的$Cost\left( {h_\theta}\left( x \right),y \right)$函数的特点是：
     
    当实际的 $y=1$ 且${h_\theta}\left( x \right)$也为 1 时误差为 0
     
    当 $y=1$ 但${h_\theta}\left( x \right)$不为1时误差随着${h_\theta}\left( x \right)$变小而变大；
     
    当实际的 $y=0$ 且${h_\theta}\left( x \right)$也为 0 时代价为 0
     
    当$y=0$ 但${h_\theta}\left( x \right)$不为 0时误差随着 ${h_\theta}\left( x \right)$的变大而变大。 
     
    将构建的 $Cost\left( {h_\theta}\left( x \right),y \right)$简化如下：
     
    $Cost\left( {h_\theta}\left( x \right),y \right)=-y\times log\left( {h_\theta}\left( x \right) \right)-(1-y)\times log\left( 1-{h_\theta}\left( x \right) \right)$ 带入代价函数得到： $J\left( \theta \right)=\frac{1}{m}\sum\limits_{i=1}^{m}{[-{ {y}^{(i)} }\log \left( {h_\theta}\left( { {x}^{(i)} } \right) \right)-\left( 1-{ {y}^{(i)} } \right)\log \left( 1-{h_\theta}\left( { {x}^{(i)} } \right) \right)]}$ 
    
    **即代价函数可以简化为：**$J\left( \theta \right)=-\frac{1}{m}\sum\limits_{i=1}^{m}{[{ {y}^{(i)} }\log \left( {h_\theta}\left( { {x}^{(i)} } \right) \right)+\left( 1-{ {y}^{(i)} } \right)\log \left( 1-{h_\theta}\left( { {x}^{(i)} } \right) \right)]}$

- **怎么用梯度下降法来求解$\theta？$**

    在得到代价函数以后，我们便可以用梯度下降算法来求得能使代价函数最小的参数了。算法为：

    Repeat {
        
    $\theta_j := \theta_j - \alpha \frac{\partial}{\partial\theta_j} J(\theta)$ (simultaneously update all ) 
    
    }

    求导后得到：

    Repeat { 
        
    $\theta_j := \theta_j - \alpha \frac{1}{m}\sum\limits_{i=1}^{m}{ {\left( {h_\theta}\left( \mathop{x}^{\left( i \right)} \right)-\mathop{y}^{\left( i \right)} \right)} }\mathop{x}_{j}^{(i)}$ (simultaneously update all )
    
    }

    在这个视频中，我们定义了单训练样本的代价函数，凸性分析的内容是超出这门课的范围的，但是可以证明我们所选的代价值函数会给我们一个凸优化问题。代价函数$J(\theta)$会是一个凸函数，并且没有局部最优值。

- **高级优化的算法？**

    一些梯度下降算法之外的选择： 除了梯度下降算法以外，还有一些常被用来令代价函数最小的算法，这些**算法更加复杂和优越**，而且通常不需要人工选择学习率，通常比梯度下降算法要更加快速。
    
    这些算法有：**共轭梯度（Conjugate Gradient）**，**局部优化法(Broyden fletcher goldfarb shann,BFGS)**和**有限内存局部优化法(LBFGS)** ，fminunc是 matlab和octave 中都带的一个最小值优化函数，使用时我们需要提供代价函数和每个参数的求导。，它们需要有一种方法来计算 $J\left( \theta \right)$，以及需要一种方法计算导数项，然后使用比梯度下降更复杂的算法来最小化代价函数。这三种算法的具体细节超出了本门课程的范畴。实际上你最后通常会花费很多天，或几周时间研究这些算法，你可以专门学一门课来提高数值计算能力，不过让我来告诉你他们的一些特性：

    这三种算法有许多优点：

    一个是使用这其中任何一个算法，你**通常不需要手动选择学习率 $\alpha$**，所以对于这些算法的一种思路是，给出计算导数项和代价函数的方法，你可以认为算法有一个智能的内部循环，而且，事实上，他们确实有一个智能的内部循环，称为线性搜索(line search)算法，它可以自动尝试不同的学习速率 $\alpha$，并自动选择一个好的学习速率 $a$，因此它甚至可以为每次迭代选择不同的学习速率，那么你就不需要自己选择。这些算法实际上在做更复杂的事情，不仅仅是选择一个好的学习速率，所以它们往往最终比梯度下降收敛得快多了，不过关于它们到底做什么的详细讨论，已经超过了本门课程的范围。

- **当遇到多分类的时候怎么样操作算法？**

    使用One-vs-all（一对多）方法可以有效解决这个问题。

    我用3种不同的符号来代表3个类别，问题就是给出3个类型的数据集，我们如何得到一个学习算法来进行分类呢？

    我们现在已经知道如何进行二元分类，可以使用逻辑回归，对于直线或许你也知道，可以将数据集一分为二为正类和负类。用一对多的分类思想，我们可以将其用在多类分类问题上。

    ![一对多.png](https://i.loli.net/2019/01/28/5c4de05652feb.png)
    现在我们有一个训练集，好比上图表示的有3个类别，我们用三角形表示 $y=1$，方框表示$y=2$，叉叉表示 $y=3$。我们下面要做的就是使用一个训练集，将其分成3个二元分类问题。

    我们先从用三角形代表的类别1开始，实际上我们可以创建一个，新的"伪"训练集，类型2和类型3定为负类，类型1设定为正类，我们创建一个新的训练集，如下图所示的那样，我们要拟合出一个合适的分类器。

    ![一对多-2.png](https://i.loli.net/2019/01/28/5c4de05660bd9.png)

    这里的三角形是正样本，而圆形代表负样本。可以这样想，设置三角形的值为1，圆形的值为0，下面我们来训练一个标准的逻辑回归分类器，这样我们就得到一个正边界。

    为了能实现这样的转变，我们**将多个类中的一个类标记为正向类（$y=1$）**，然后**将其他所有类都标记为负向类**，这个模型记作$h_\theta^{\left( 1 \right)}\left( x \right)$。接着，类似地第我们选择另一个类标记为正向类（$y=2$），再将其它类都标记为负向类，将这个模型记作 $h_\theta^{\left( 2 \right)}\left( x \right)$,依此类推。 最后我们得到一系列的模型简记为： $h_\theta^{\left( i \right)}\left( x \right)=p\left( y=i|x;\theta \right)$其中：$i=\left( 1,2,3....k \right)$

    最后，在我们需要做预测时，我们将所有的分类机都运行一遍，然后对每一个输入变量，都选择最高可能性的输出变量。

    总之，我们已经把要做的做完了，现在要做的就是训练这个逻辑回归分类器：$h_\theta^{\left( i \right)}\left( x \right)$， 其中 $i$ 对应每一个可能的 $y=i$，最后，为了做出预测，我们给出输入一个新的 $x$ 值，用这个做预测。我们要做的就是在我们三个分类器里面输入 $x$，然后我们选择一个让 $h_\theta^{\left( i \right)}\left( x \right)$ 最大的$ i$，即
    $\mathop{\max}\limits_i,h_\theta^{\left( i \right)}\left( x \right)$。