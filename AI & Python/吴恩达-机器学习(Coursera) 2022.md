# Link 🔗

视频：[2022吴恩达机器学习Deeplearning.ai课程_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Pa411X76s/?spm_id_from=333.337.search-card.all.click&vd_source=bf4c8577fb22820a80ba9f8a7e04169a)

代码实践开放：https://github.com/kaieye/2022-Machine-Learning-Specialization

Coursera链接：[Machine Learning Specialization | Coursera](https://www.coursera.org/specializations/machine-learning-introduction)

# 1. Intro

Machine learning algorithms

- Supervised learning    :   used most in real-world applications
- Unsupervised learning

Semi-supervised learning

Reinforcement learning



## Supervised learning

  X             &rArr;             Y

Input                  Output Label

> Learn from data labeled with the "right answers'.

There are many applications:

- spam filtering
- speech recognition
- machine translation
- online advertising
- self-driving car
- visual inspection



Major Tasks:

- Regression:     predict a number  from   infinitely many possible outputs.
- Classification:  predict  a small number of possible output categories (or “classes”).



## Unsupervised learning

Data only comes with inputs x, but not output labels y.     Algorithm has to find structure in the data.

> Find  something  interesting (or what patterns or structures) in  unlabeled data.

example algorithms:  

- Clustering:  group similar data points together.
- Anomaly detection:  find unusual data points.
- Dimensionality reduction:  compress data using fewer numbers.



## Terminology

​	training set

​	    &dArr;

learning algorithm

​	   &dArr;

$\Large x \quad \rightarrow \quad Function ~~  f \quad \rightarrow \quad \hat y$

[feature]                [model]                        [prediction] (“y-hat” is the estimated value of y) (y is “target”, the true value in the training set)

> The key is how to represent $f$ .



## Linear Regression Model

> Linear regression is one example of a regression model.

Linear regression with one variable:     $\Large f_{w,b}(x) = w\cdot x + b$

​	also called “Univariate linear regression”   (with a single feature $x$)

> $w, b:$   parameters /  coefficients  
>
> $w:$  weight.
>
> $b:$  bias.



Cost function:     

  Squared error cost function:    $\Large  J(w,b) = \frac{1}{2m}\sum_{i=1}^m(\hat y^i - y^i)^2 = \frac{1}{2m}\sum_{i=1}^m(f_{w,b}(x^i) - y^i)^2$

> $(\hat y - y)$ is “error”.      $m$ = number of training examples.

 we want to find  $w,b$  , so that $\hat y^i$  is close to  $y^i$ for all $(x^i, y^i)$.

goal:  $\large  \underset{w,b}{minimize} ~ J(w,b)$



## Gradient Descent

Now let we have cost function $J(w,b)$,    we want   $\Large \underset{w,b}{min} ~~ J(w,b)$.    or  $min ~~ J(w_1, w_2, ..., w_n,b)$

Outline:

- Start with some $w,b$   (e.g.  set $w=0, b=0$ )
- Keep changing $w,b$ to reduce   $ J(w,b)$
- Until we settle at or near a minimum.   (there may be more than one possible minimum,  called “local minima” / “local minimum”)   



Gradient Descent algorithm:

Repeat until convergence:       {

​	$\Large temp\_w = w - \alpha\frac{\partial}{\partial w}J(w,b)$ ;

​	$\Large temp\_b = b - \alpha\frac{\partial}{\partial b}J(w,b)$ ;

​	$\Large w = temp\_w$

​	$\Large b = temp\_ b$

}

> $\large \alpha$   here is “learning rate”, usually between 0 and 1.   Larger $\alpha$​ &rArr; agressive gradient descent procedure.
>
> We need simultaneously update  $w$ and $b$ .

Choosing $\large \alpha$ - learning rate is important.

​	If α is too small:  Gradient descent may be slow.

​	If α is too large:   Gradient descent may: 

- ​						Overshoot, never reach minimum.
- ​						Fail to converge and even diverge.

Gradient descent can reach local minimum with fixed learning rate.



Gradient Descent algorithm for Linear Regression:

repeat until convergence {

​	$\Large w = w - \alpha\frac{1}{m}\sum_{i=1}^m(f_{w,b}(x^i)-y^i) x^i = w - \alpha\frac{1}{m}\sum_{i=1}^m(wx^i+b-y^i) x^i$

​	$\Large b = b - \alpha\frac{1}{m}\sum_{i=1}^m(f_{w,b}(x^i)-y^i) $

}

> For a squared error cost function with linear regression, the cost function never has multiple local minima, only has a single global minima as it is a “convex function”.

“Batch” gradient descent

“Batch” : Each step of gradient descent  uses all the training examples  instead of a subset of the training data.



# 2.Multiple Features

model with n features :  $\Large f_{w, b}(x) = w_1x_1 + w_2x_2+...+w_nx_n + b$

> parameters of the model:  $\Large \vec{w} = [w_1, w_2, ..., w_n]$    .
>
> ​					  $\Large b$    (is a number).
>
> features :  $$\large \begin{bmatrix} x_1  \\ ... \\ x_n  \end{bmatrix} $$

Multiple linear regression:     $\Large f_{\vec{w}, b}(\vec{x}) = \vec{w} \cdot \vec{x} + b$  .



## Vectorization

use `NumPy ` to run your code faster.

```py
import numpy as np	

w = np.array([1.0, 2.5, -3.3])   # w[0], w[1], w[2]
b = 4
x = np.array([10, 20, 30])       # x[0], x[1], x[2]
# Vectorization
f = np.dot(w, x) + b     # f(x) = w · x + b     [If both a and b are 1-D arrays, it is inner product of vectors]
```



## Gradient Descent for Multiple Regression

​              for n features (n $\geq$ 2) :

Repeat  {

​	$\Large w_1 = w_1 - \alpha\frac{1}{m}\sum^m_{i=1}(f_{\vec{w}, b}(\vec{x}^i) - y^i)x_1^i$

​	...

​	$\Large w_n = w_n - \alpha\frac{1}{m}\sum^m_{i=1}(f_{\vec{w}, b}(\vec{x}^i) - y^i)x_n^i$

​	$\Large b = b - \alpha\frac{1}{m}\sum^m_{i=1}(f_{\vec{w}, b}(\vec{x}^i) - y^i)$

}        【simultaneously update $\large w_j ~~(for ~~ j =1, ..., n)  ~~~ and ~~~ b$】



An alternative to gradient descent:  **Normal equation**     (no need to know details)

- **Only for linear regression**

- Solve for w, b without iterations

Disadvantages:

- Doesn't generalize to otherlearning algorithms.
- Slow when number of features is large (> 10,000)

> **Normal equation** method may **be used in machine learning libraries** that **implement linear regression**.
>
> **Gradient descent is the recommended method** for finding parameters $w,b$ .



## FeatureScaling
