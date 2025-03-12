## $logistic$回归

一般用于分类问题

### 分类

#### 二分类

此类分类只有两类，换言之该分类只有两种状态。

### 假设陈述

因为在分类问题中，尤其是在二分类中，状态通常只有0和1两种，但是在线性回归中，$h_\theta(x)$会出现大于1的情况，所以通常不适合于分类问题，所以现在需要一种新的概述，使$h_\theta(x)$处于0到1之间。

在线性回归中我们的$h_\theta(x)$为：
$$
h_\theta(x)=\theta^Tx
$$
在$logistic$回归中，我们对$h_\theta(x)$做出改进，令$h_\theta(x)=g(\theta^Tx)$，其中：
$$
g(x)={1\over1+e^{-x}}
$$
我们将两个方程结合起来，就得到了新的$h_\theta(x)$：
$$
g(x)={1\over1+e^{-\theta^Tx}}
$$
这个函数一般被称为$sigmoid$函数，其图形为：

![](http://oss.pyaxy.xyz/img/p10.png)

有了这个假设，我们只需要一组$\theta$来拟合我们的数据。

#### 模型的解释：

$h_\theta(x)$估计了输入$x$时$y=1$的概率



### 决策界限

$sigmoid$函数已经决定了输入$x$时$y=1$的概率，当$x$大于$0$的时候$h_\theta(x)$大于0.5，表示$y=1$，反之则为$y=0$。

现在问题就转换为拟合$\theta^T$使在$X$时的$\theta^{T}X$大于$0$，如下图。

![](http://oss.pyaxy.xyz/img/p11.png)

图中的点集为数据集，一共有两个特征变量，得出$h_\theta(x)=g(\theta_0+\theta_1x_1+\theta_2x_2)$

假设我们已经通过方法把$\theta$都拟合出了，得出$h_\theta(x)=g(-3+x_1+x_2)$

当我们预测$y=1$时，使$-3+x_1+x_2>0$，根据我们高中学过的线性回归，我们就可以在图上画出一条直线，在直线之上的点都表示$y=1$，这条线就被称为决策界限。

### 代价函数

在上面的问题中我们谈到了决策界限的定义，但是决策界限是基于已经拟合出来的$\theta$来决定了，现在我们的问题来到了如何拟合$\theta$，要拟合$\theta$首先需要把代价函数求出来。

在线性回归中我们的代价函数为：
$$
J(\theta )=\frac{1}{m}\sum_{i=1}^{m}\frac{1}{2}(h_\theta(x^{(i)})-y^{(i)})^2   
$$
但是在$logistic$回归中，因为$h_\theta(x)$的不同，$J(\theta)$为非凸函数，存在多个局部最优解，不可以使用梯度下降法拟合，因此我们将$J(\theta)$设为：
$$
J(\theta)=\frac{1}{m}\sum_{i=1}^{m}Cost(h_\theta(x),y)
$$
其中：
$$
Cost(h_\theta(x),y)=\begin{cases}
-log(h_\theta(x))&if&y=1\\
-log(1-h_\theta(x))&if&y=0
\end{cases}
$$
当$y=1$时：

![](http://oss.pyaxy.xyz/img/p12.png)

代价函数如图所示，当估计的$h_\theta(x)$为1时，实际上的$y$也为$1$，可以很好的拟合，因此我们的代价在图中为0；

当$h_\theta(x)$估计为0时，实际上为$y=1$，与实际不符，所以图中的代价为$\infty$

反之在下图$y=0$时也成立

![](http://oss.pyaxy.xyz/img/p13.png)

### 简化代价函数以及梯度下降

在上面我们得出了$logistic$回归的代价函数，我们现在可以来将它化简为：
$$
Cost(h_\theta(x),y)=-ylog(h_\theta(x))-(1-y)log(1-h_\theta(x))
$$
此式也可以表示为$logistic$回归的代价函数

现在有：
$$
J(\theta)=-\frac{1}{m}[\sum_{i=1}^{m}ylog(h_\theta(x))+(1-y)log(1-h_\theta(x))]
$$
To fit parameters $\theta$:

We need to minimize $J(\theta)$

We use Gradient Descent to minimize $J(\theta)$
$$
\theta_j:=\theta_j-\alpha\frac{\partial}{\partial\theta_j}J(\theta)\\
$$
重复上式，其中
$$
\frac{\partial}{\partial\theta_j}J(\theta)=\frac{1}{m}\sum_{i=1}^{m}(h_\theta(x^{(i)})-y^{(i)})x_j^{(i)}
$$


### 高级优化

我们可以使用matlab中自带的一些优化算法来拟合出$J(\theta)$的$\theta$，下面举个例子：

Example：
$$
\theta=\begin{bmatrix}
 \theta_1\\
\theta_2
\end{bmatrix}\\
J(\theta)=(\theta_1-5)^2+(\theta_2-5)^2\\
\frac{\partial}{\partial\theta_1}J(\theta)=2(\theta_1-5)\\
\frac{\partial}{\partial\theta_2}J(\theta)=2(\theta_2-5)
$$
在matlab中我们可以写这样一个函数：

```matlab
function [jVal, gradient]
			= costFunction(theta)
	jVal = (theta(1)-5)^2 + (theta(2)-5)^2;	//为代价函数
	gradient = zeros(2,1);					//梯度值
	gradient = 2*(theta(1)-5);
	gradient = 2*(theta(2)-5);
```

然后在终端中输入：

```matlab
option = optimset('GradObj', 'on', 'MaxIter', '100');	//优化设置
initialTheta = zeros(2,1);								//初始化theta
[optTheta, functionVal, exitFlag]
	= fminunc(@costFunction, initialTheta, option);
```



### 多元分类问题

我们现在要分多个类别，比如分类邮件为亲人、同事、广告三个，这就涉及到多元分类问题。

我们将上面的类别分别设为1、2、3，多个类别时我们的点集可能如下：

![](http://oss.pyaxy.xyz/img/p14.png)

我们可以将2、3看为负类，将1看为正类，得到y=1时的$h_\theta^{(1)}(x)$

这样我们就可以得到三个分类器，对应三个不同的类别，这样就实现了多元分类问题。



## $logistic$回归

一般用于分类问题

### 分类

#### 二分类

此类分类只有两类，换言之该分类只有两种状态。

### 假设陈述

因为在分类问题中，尤其是在二分类中，状态通常只有0和1两种，但是在线性回归中，$h_\theta(x)$会出现大于1的情况，所以通常不适合于分类问题，所以现在需要一种新的概述，使$h_\theta(x)$处于0到1之间。

在线性回归中我们的$h_\theta(x)$为：
$$
h_\theta(x)=\theta^Tx
$$
在$logistic$回归中，我们对$h_\theta(x)$做出改进，令$h_\theta(x)=g(\theta^Tx)$，其中：
$$
g(x)={1\over1+e^{-x}}
$$
我们将两个方程结合起来，就得到了新的$h_\theta(x)$：
$$
g(x)={1\over1+e^{-\theta^Tx}}
$$
这个函数一般被称为$sigmoid$函数，其图形为：

![](http://oss.pyaxy.xyz/img/p10.png)

有了这个假设，我们只需要一组$\theta$来拟合我们的数据。

#### 模型的解释：

$h_\theta(x)$估计了输入$x$时$y=1$的概率



### 决策界限

$sigmoid$函数已经决定了输入$x$时$y=1$的概率，当$x$大于$0$的时候$h_\theta(x)$大于0.5，表示$y=1$，反之则为$y=0$。

现在问题就转换为拟合$\theta^T$使在$X$时的$\theta^{T}X$大于$0$，如下图。

![](http://oss.pyaxy.xyz/img/p11.png)

图中的点集为数据集，一共有两个特征变量，得出$h_\theta(x)=g(\theta_0+\theta_1x_1+\theta_2x_2)$

假设我们已经通过方法把$\theta$都拟合出了，得出$h_\theta(x)=g(-3+x_1+x_2)$

当我们预测$y=1$时，使$-3+x_1+x_2>0$，根据我们高中学过的线性回归，我们就可以在图上画出一条直线，在直线之上的点都表示$y=1$，这条线就被称为决策界限。

### 代价函数

在上面的问题中我们谈到了决策界限的定义，但是决策界限是基于已经拟合出来的$\theta$来决定了，现在我们的问题来到了如何拟合$\theta$，要拟合$\theta$首先需要把代价函数求出来。

在线性回归中我们的代价函数为：
$$
J(\theta )=\frac{1}{m}\sum_{i=1}^{m}\frac{1}{2}(h_\theta(x^{(i)})-y^{(i)})^2   
$$
但是在$logistic$回归中，因为$h_\theta(x)$的不同，$J(\theta)$为非凸函数，存在多个局部最优解，不可以使用梯度下降法拟合，因此我们将$J(\theta)$设为：
$$
J(\theta)=\frac{1}{m}\sum_{i=1}^{m}Cost(h_\theta(x),y)
$$
其中：
$$
Cost(h_\theta(x),y)=\begin{cases}
-log(h_\theta(x))&if&y=1\\
-log(1-h_\theta(x))&if&y=0
\end{cases}
$$
当$y=1$时：

![](http://oss.pyaxy.xyz/img/p12.png)

代价函数如图所示，当估计的$h_\theta(x)$为1时，实际上的$y$也为$1$，可以很好的拟合，因此我们的代价在图中为0；

当$h_\theta(x)$估计为0时，实际上为$y=1$，与实际不符，所以图中的代价为$\infty$

反之在下图$y=0$时也成立

![](http://oss.pyaxy.xyz/img/p13.png)

### 简化代价函数以及梯度下降

在上面我们得出了$logistic$回归的代价函数，我们现在可以来将它化简为：
$$
Cost(h_\theta(x),y)=-ylog(h_\theta(x))-(1-y)log(1-h_\theta(x))
$$
此式也可以表示为$logistic$回归的代价函数

现在有：
$$
J(\theta)=-\frac{1}{m}[\sum_{i=1}^{m}ylog(h_\theta(x))+(1-y)log(1-h_\theta(x))]
$$
To fit parameters $\theta$:

We need to minimize $J(\theta)$

We use Gradient Descent to minimize $J(\theta)$
$$
\theta_j:=\theta_j-\alpha\frac{\partial}{\partial\theta_j}J(\theta)\\
$$
重复上式，其中
$$
\frac{\partial}{\partial\theta_j}J(\theta)=\frac{1}{m}\sum_{i=1}^{m}(h_\theta(x^{(i)})-y^{(i)})x_j^{(i)}
$$


### 高级优化

我们可以使用matlab中自带的一些优化算法来拟合出$J(\theta)$的$\theta$，下面举个例子：

Example：
$$
\theta=\begin{bmatrix}
 \theta_1\\
\theta_2
\end{bmatrix}\\
J(\theta)=(\theta_1-5)^2+(\theta_2-5)^2\\
\frac{\partial}{\partial\theta_1}J(\theta)=2(\theta_1-5)\\
\frac{\partial}{\partial\theta_2}J(\theta)=2(\theta_2-5)
$$
在matlab中我们可以写这样一个函数：

```matlab
function [jVal, gradient]
			= costFunction(theta)
	jVal = (theta(1)-5)^2 + (theta(2)-5)^2;	//为代价函数
	gradient = zeros(2,1);					//梯度值
	gradient = 2*(theta(1)-5);
	gradient = 2*(theta(2)-5);
```

然后在终端中输入：

```matlab
option = optimset('GradObj', 'on', 'MaxIter', '100');	//优化设置
initialTheta = zeros(2,1);								//初始化theta
[optTheta, functionVal, exitFlag]
	= fminunc(@costFunction, initialTheta, option);
```



### 多元分类问题

我们现在要分多个类别，比如分类邮件为亲人、同事、广告三个，这就涉及到多元分类问题。

我们将上面的类别分别设为1、2、3，多个类别时我们的点集可能如下：

![](http://oss.pyaxy.xyz/img/p14.png)

我们可以将2、3看为负类，将1看为正类，得到y=1时的$h_\theta^{(1)}(x)$

这样我们就可以得到三个分类器，对应三个不同的类别，这样就实现了多元分类问题。