---
layout: post
title: 과적합(Overfitting)과 정규화(Regularization)
category: Machine Learning
tags: [Machine Learning, Andrew Ng, Overfitting, 과적합]
comments: true
---

<br><br>
**이 글을 읽기 전에**<br>
- _해당 포스팅은 Coursera에 올라와 있는 Andrew Ng 교수님의 Machine Learning 강좌를 정리/각색하였습니다._
- _해당 포스팅은 개인적으로 공부한 내용을 정리한 것으로, 일부 내용에 오류가 있을 수 있습니다._
- _포스팅에 오류가 있을 시, 댓글로 남겨주시면 감사하겠습니다._

---

이번 포스팅에서는 기계학습에서 자주 발생하는 문제인 **과적합(Overfitting)**과 이에 대한 해결방법인 **정규화(Regularization)**에 대해 알아보도록 하겠습니다.

과적합에 대해 설명하기 전에, 우리가 늘 사용하는 집값을 선형 회귀로 표현한 그래프를 보시겠습니다.

<center><img src="/assets/ml/08_overfitting_reg/Fig01_housing_prices.png" width="700" height="200"></center>
<center>Fig. 1. Linear Regression of Housing Prices with Various Model</center>

<br>
Fig. 1 의 왼쪽 그래프에서 사용한 모델은 이 데이터에 잘 맞지 않습니다. 사용되는 Feature가 1개 뿐인데 데이터를 매개변수 2개를 사용한 직선의 형태로 억지로 모델링하려다 보니, 알고리즘이 강한 선입견(Preconception)을 가지게 되어, 데이터의 분포가 집의 크기가 커져도 집값이 크게 변하지 않는 평탄한 부분에 대한 예측을 잘 하지 못하게 됩니다. 이러한 문제를 **과소적합(Underfitting)**이라고 합니다. 과소적합을 다른 말로 표현하면, 모델이 높은 **편향(Bias)**을 가지고 있다고 할 수 있습니다. 편향은 어떤 것을 예측할 때 Feature의 영향을 받지 않는 값이지요. $\theta_{0}$ 가 편향에 해당됩니다. 이 편향 수치가 높으면 Feature에 관계 없이 엉뚱한 예측을 하게 될 겁니다. 주로 모델이 단순하거나 학습 Iteration 횟수가 적은 경우 과소적합 문제가 발생합니다.

반면 Fig. 1 의 가운데 그래프는 데이터를 적절하게 모델링하고 있습니다.

마지막으로 Fig. 1 의 오른쪽 그래프는 4차 함수를 만들어, 5개의 매개변수를 사용했습니다. 해당 모델은 데이터에 잘 맞으니 적어도 훈련 데이터셋에서는 잘 동작할 것입니다. 하지만 보시다시피 그래프가 굉장히 꼬여져 있습니다. 이러한 모델이 테스트 데이터셋에서도 집값을 정확히 예측하지는 못할 것입니다. 이러한 문제를 **과적합(Overfitting)**이라고 합니다. 과적합을 다른 말로 표현하면, 높은 **분산(Variance)**을 가지고 있다고 할 수 있습니다. 모델은 어떤 고차 함수를 데이터에 맞추려고 하고, 이에 완벽히 맞는 함수를 만들고자 하면 만들 수 있지만, 그러면 그래프와 같이 커다란 변동성을 갖게 됩니다. 주로 모델이 복잡하거나 학습 Iteration 횟수가 너무 많은 경우 과소적합 문제가 발생합니다.

따라서, 집값을 예측하기에 가장 적절한 모델은 Fig. 1 의 가운데 그래프가 되겠습니다.
 
정리하면, 우리가 수많은 Feature들을 가지고 있을 때, 학습한 가설이 훈련 데이터셋에만 잘 맞아서 훈련 때 사용되는 비용 함수(Cost Function)는 거의 0에 가까운 값을 가지게 되지만, 학습과정에서 본 적 없는 데이터셋에는 잘 예측을 하지 못하는 것을 과적합이라고 합니다. 즉, 모델이 일반화(Generalization)가 잘 안되어서, 새로운 데이터셋에 대해서 가설이 잘 맞지 않는 것이죠.

<center><img src="/assets/ml/08_overfitting_reg/Fig02_logistic_regression.png" width="700" height="300"></center>
<center>Fig. 2. Logistic Regression with Various Model</center>

<br>
분류를 위한 로지스틱 회귀 그래프도 함께 보겠습니다. Fig. 2 의 왼쪽 그래프는 단순한 모델을 사용하여 과소적합 문제가 있고, 오른쪽 그래프는 복잡한 모델을 사용하여 Decision Boundary가 복잡한 형태를 띄는 과적합 문제가 있습니다. 따라서 가운데 그래프가 가장 적절하게 Class를 분류할 수 있는 모델입니다.

<br>
과적합 문제를 해결하기 위해서는 크게 2가지 해결법이 있습니다.
1. Feature의 개수를 줄인다.
    + 어떤 Feature가 중요한지 안중요한지 직접 고른다.
    + 알고리즘이 어떤 Feature을 사용할지 고른다.(**Will be posted later**)
    * Feature를 버림으로써 문제에 포함된 정보를 같이 버리게 된다.
2. 정규화(Regularization)
    + 매개변수의 값을 감소시킨다.
    * Featured의 개수를 유지함으로써 예측하기 위한 정보를 살릴 수 있다.

여기서 우린 정규화하는 방법에 대해서 이야기해보겠습니다.

먼저 선형 회귀에서 정규화 하는 방법을 봅시다.

우리는 Fig. 1 의 가운데 그래프에서 매개변수 3개만 사용하여도 집값 예측 데이터를 적절하게 모델링 할 수 있다는 것을 확인하였습니다. 하지만 우리는 Feature를 버리지 않고 더 정확한 예측을 하고 싶습니다. 그러니 Fig. 1 의 오른쪽 그래프에서 사용되었던 가설을 그대로 차용한다고 해보죠. 그 가설은 하단의 식 (1)과 같습니다.

<p>$h_{\boldsymbol{\theta}}(\boldsymbol{x}) = \theta_{0} + \theta_{1} \boldsymbol{x} + \theta_{2} \boldsymbol{x}^2 + \theta_{3} \boldsymbol{x}^3 + \theta_{4} \boldsymbol{x}^4 \tag{1}$</p>

$\theta_{0}$ , $\theta_{1}$ , $\theta_{2}$ 는 고차원의 함수가 아니므로 $\theta_{3}$ 와 $\theta_{4}$ 가 과적합에 영향을 주고 있다고 예상해봅시다. 따라서 우리는 $\theta_{3}$ 와 $\theta_{4}$ 에 주목하여 해당 매개변수들에게 패널티를 줘서 작게 만들겠습니다.

<p>$\min_{\theta} \frac{1}{2m} \sum_{i=1}^m ( h_{\boldsymbol{\theta}(x^{(i)})} - y^{(i)})^{2} + 1000\theta_{3}^2 + 1000\theta_{4}^2 \tag{2}$</p>

매개변수를 작게 하기 위해 일반적으로 선형 회귀에서 사용하는 MSE 비용함수 식을 식 (2)와 같이 수정합니다. 여기서 $\theta$ 앞에 붙은 계수 1000은 예시용으로 사용된 적당히 큰 숫자입니다.

식 (2)를 최소화 하기 위해서는 기본적으로 $\theta_{3}$ 와 $\theta_{4}$ 가 작아져야 되겠지요? 따라서 $\theta_{3}$ 와 $\theta_{4}$ 는 0에 가까운 값을 가지게 되고, Feature를 없애지 않고 그대로 사용하여 학습에 작은 기여를 할 수 있게 됩니다.

매개변수가 작은 값을 가진다는 것은 가설이 좀 더 단순화된다는 의미입니다. 식 (1)에서 $\theta_{3}$ 와 $\theta_{4}$ 가 0에 가까운 값을 가지게 됨으로써 식이 거의 2차 함수에 가까운 형태가 되니까요. 그리고 가설이 단순화되면 과적합되는 경향이 줄어들게 됩니다. 하지만 어떤 Feature에 대응되는 매개변수가 작아져야 하는지 알기는 어렵습니다. 따라서, 어떤 매개변수를 작게 만들어야 하는지 알 수 없기 때문에, 우리는 비용함수 식을 수정하여 모든 매개변수를 작게 만들겁니다. 그 식은 하단의 식 (3)과 같습니다.

<p>$J(\boldsymbol{\theta}) = \frac{1}{2m} [ \sum_{i=1}^m ( h_{\boldsymbol{\theta}(x^{(i)})} - y^{(i)})^{2} + \lambda \sum_{j=1}^n \theta_{j}^2 ] \tag{3}$</p>

여기서 $m$ 은 훈련 데이터셋의 개수, $n$ 은 편향을 제외한 매개변수의 개수입니다. 또한 $\lambda \sum_{j=1}^n \theta_{j}^2$ 는 정규화 항이라 불리우고, $\lambda$ 는 정규화 매개변수(Regularization Parameter)라고 합니다. 여기서 헷갈리지 말아야 할 것은 $\theta_0$ , 즉 편향은 정규화 과정에 포함되지 않는다는 것입니다.

$\lambda$ 의 목적은 가설과 예측의 차이를 계산하는 항, 즉 $h_{\boldsymbol{\theta}(x^{(i)})} - y^{(i)}$ 와 매개변수의 정규화 항 둘 중 어떤 항이 비용함수 최소화에 더 큰 영향을 줄지 결정해줍니다. $\lambda$ 가 너무 작으면 매개변수 정규화가 잘 되지 않을 것이고, $\lambda$ 가 너무 크면 편향을 제외한 모든 매개변수가 0에 수렴하여 가설의 예측이 입력값에 관계없이 편향값만을 가르키게 되고, 이는 과소적합 문제를 일으킬 것입니다.

식 (3)에서 비용함수 $J(\boldsymbol{\theta})$ 를 편미분하여 구한 경사 하강법 알고리즘은 하단의 식 (4)와 같습니다.

Repeat {<br>
<p>$$\eqalign{
\quad \theta_{0} &:= \theta_{0} - \alpha \frac{1}{m} \sum_{i=1}^m (h_{\theta}(\boldsymbol{x}^{(i)}) - y^{(i)} )\boldsymbol{x}_{0}^{(i)} \\
\quad \theta_{j} &:= \theta_{j} - \alpha[\frac{1}{m} \sum_{i=1}^m (h_{\theta}(\boldsymbol{x}^{(i)}) - y^{(i)} )\boldsymbol{x}_{j}^{(i)} + \frac{\lambda}{m} \theta_{j}]   \qquad (j = 1, 2, 3, \cdots, n) \\
&= \theta_{j}(1 - \alpha \frac{\lambda}{m}) - \alpha \frac{1}{m} \sum_{i=1}^m (h_{\theta}(\boldsymbol{x}^{(i)}) - y^{(i)} )\boldsymbol{x}_{j}^{(i)} \tag{4}
}$$</p>
}

식 (4)에서 $1 - \alpha \frac{\lambda}{m} < 1$ 입니다. 따라서, 정규화를 통해 매개변수 $\theta_{j}$ 를 업데이트 시, $\theta_{j}$ 가 조금 작아지는 효과를 얻을 수 있습니다.

또한 식 (3)에서 비용함수 $J(\boldsymbol{\theta})$ 를 편미분하여 구한 정규방정식 알고리즘은 하단의 식 (5)와 같습니다.

<p>$$\min_{\theta} J(\theta) \Longrightarrow \frac{\partial}{\partial \theta_{j}} J(\boldsymbol{\theta}) = 0$$</p>
<p>$$
\Rightarrow \boldsymbol{\theta} = (\boldsymbol{X}^T \boldsymbol{X} + \lambda \cdot L)^{-1} \boldsymbol{X}^T y \quad \mbox {where} \quad L = \begin{bmatrix}
0 & 0 & 0 & \cdots & 0 \\
0 & 1 & 0 & \cdots & 0 \\
0 & 0 & 1 & \cdots & 0 \\
\vdots & \vdots & \vdots & \ddots & 0 \\
0 & 0 & 0 & 0 & 1 
\end{bmatrix}
\tag{5}
$$</p>

식 (5)에서 $L$ 은 $(n+1) \times (n+1)$ 차원의 행렬입니다.

<br>
지금까지 선형 회귀에 대한 정규화 과정을 보았으니, 로지스틱 회귀에서 사용되는 정규화 과정 또한 보겠습니다.

<p>$$\eqalign{
    J(\boldsymbol{\theta}) = &- [\frac{1}{m} \sum_{i=1}^m y^{(i)} \log h_{\theta}(\boldsymbol{x}^{(i)}) + (1-y^{(i)}) \log (1 - h_{\theta}(x^{(i)}))] \\
    &+ \frac{\lambda}{2m} \sum_{j=1}^n \theta_{j}^2 \qquad \qquad (j = 1, 2, \cdots, n) \tag {6}
}$$</p>

로지스틱 회귀에서는 비용함수로 MSE 대신 Binary Cross Entropy를 사용하므로, 로지스틱 회귀에서 정규화가 포함된 비용함수는 식 (6)과 같습니다. 식 (6)에서 비용함수 $J(\boldsymbol{\theta})$ 를 편미분하여 구한 경사 하강법 알고리즘은 하단의 식 (7)와 같습니다.

Repeat {<br>
<p>$$\eqalign{
\quad \theta_{0} &:= \theta_{0} - \alpha \frac{1}{m} \sum_{i=1}^m (h_{\theta}(\boldsymbol{x}^{(i)}) - y^{(i)} )\boldsymbol{x}_{0}^{(i)} \\
\quad \theta_{j} &:= \theta_{j} - \alpha[\frac{1}{m} \sum_{i=1}^m (h_{\theta}(\boldsymbol{x}^{(i)}) - y^{(i)} )\boldsymbol{x}_{j}^{(i)} + \frac{\lambda}{m} \theta_{j}]   \qquad (j = 1, 2, 3, \cdots, n) \\
&= \theta_{j}(1 - \alpha \frac{\lambda}{m}) - \alpha \frac{1}{m} \sum_{i=1}^m (h_{\theta}(\boldsymbol{x}^{(i)}) - y^{(i)} )\boldsymbol{x}_{j}^{(i)} \tag{7}
}$$</p>
}

식 (7)은 가설 표현(Hypothesis Representation)이 다른 것을 제외하고는 식 (4)와 동일합니다. MSE의 편미분과 Binary Cross Entropy의 편미분 결과는 동일하기 때문입니다. 이에 대한 증명은 [로지스틱 회귀 포스트](https://jiwonkkim.github.io/machine%20learning/2021/01/23/ml_lr_classification/)를 참고해주세요.

Fig. 2 의 오른쪽 그래프와 같이 과적합된 모델이 정규화 과정을 거치고 나면 Fig. 2 의 가운데 그래프와 같은 형태로 바뀔 것입니다.

#### 이번 포스팅 정리
- 주어진 문제에 대해 과도한 편향치를 갖고 있어서 발생하는 문제를 과소적합이라고 한다.
- 주어진 문제에 대해 너무 과도하게 적합되어서 발생하는 문제를 과적합이라고 한다.
- 일반적으로 우리 맞부딪히게 되는 문제는 과적합이며, 이는 정규화를 통해 해결할 수 있다.
- 정규화란, 비용함수에 매개변수들의 합으로 구성되어 있는 항을 추가하여, 매개변수가 비용함수에 영향을 주는 비율을 올린다. 이 비율을 정규화 매개변수라고 하고, $\lambda$ 로 표기한다.
- 정규화가 포함된 매개변수 업데이트 식의 형태는 선형 회귀와 로지스틱 회귀가 같다. 단, 가설 표현은 다르다.

##### 출처:
- [Andrew Ng - Machine Learning](https://www.coursera.org/learn/machine-learning)
