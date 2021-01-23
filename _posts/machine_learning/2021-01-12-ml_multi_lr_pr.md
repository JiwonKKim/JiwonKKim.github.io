---
layout: post
title:  다변량 선형회귀(Multivariate Linear Regression)과 다항식 회귀(Polynomial Regression)
category: Machine Learning
tags: [Machine Learning, Andrew Ng, Multivariate Linear Regression, 다변령 선형회귀, Polynomial Regression, 다항식 회귀]
comments: true
---

<br><br>
**이 글을 읽기 전에**<br>
- _해당 포스팅은 Coursera에 올라와 있는 Andrew Ng 교수님의 Machine Learning 강좌를 정리/각색하였습니다._
- _해당 포스팅은 개인적으로 공부한 내용을 정리한 것으로, 일부 내용에 오류가 있을 수 있습니다._
- _포스팅에 오류가 있을 시, 댓글로 남겨주시면 감사하겠습니다._

---

지금까지 우리는 매개변수 1개 혹은 2개를 가정하여 선형회귀 분석 기법을 사용했습니다. 이제 매개변수를 1개 이상의 경우를 생각한 **다변량 선형회귀(Multivariate Linear Regression)**에 대해 알아보도록 하겠습니다.

[지난 포스트](https://jiwonkkim.github.io/machine%20learning/2020/12/23/ml_introduction_01/)에서는 집의 크기라는 1개의 Feature만 사용하여 집값을 예측했습니다. 하지만 실제 집값이 집의 크기라는 특징만으로 매겨질리 없겠죠? 실제로는 침실의 개수, 집의 나이 등을 고려할 수 있겠지요. 우리가 집을 고를때 고려하는 요소들처럼 말이죠.

우리는 이제 매개변수를 여러개 사용할 것이므로, 가설함수를 재정의하도록 하겠습니다.

<p>$$h_{\theta}(\boldsymbol{x}) = \theta_{0}x_{0} + \theta_{1}x_{1} + \theta_{2}x_{2} + \cdots + \theta_{n}x_{n} \tag{1}$$</p>
<p>$$
\boldsymbol{x}
=
\begin{bmatrix}
x_{0} \\
x_{1} \\
x_{2} \\
\vdots \\
x_{n}
\end{bmatrix}
\in
\mathbb{R}^{n+1}
,
\boldsymbol{\theta}
=
\begin{bmatrix}
\theta_{0} \\
\theta_{1} \\
\theta_{2} \\
\vdots \\
\theta_{n}
\end{bmatrix}
\in
\mathbb{R}^{n+1}
\tag{2}$$</p>

이제 우리가 사용할 입력 $\boldsymbol{x}$ 와 매개변수 $\boldsymbol{\theta}$ 는 여러가지 값을 가지고 있는 $n+1$ 차원의 벡터가 됩니다. 여기서 계산의 편의를 위해 $x_{0}$ 를 1이라고 두겠습니다.

<p>$$h_{\theta}(\boldsymbol{x}) = \theta_{0} + \theta_{1}x_{1} + \theta_{2}x_{2} + \cdots + \theta_{n}x_{n} = \boldsymbol{\theta}^{T}\boldsymbol{x} \tag{3}$$</p>

식 (3)에서 T는 행렬의 전치(Transpose)를 의미합니다. 즉, 가설함수 $h_{\theta}(\boldsymbol{x})$ 는 벡터 $\boldsymbol{x}$ 와 벡터 $\boldsymbol{\theta}$ 의 내적(Inner Product)가 되는거죠. 식 (3)이 여러가지 Feature를 가지고 있을 때의 추론의 형태입니다.

가설함수를 재정의함에 따라 Mean Square Error를 사용한 비용함수도 하단의 식 (4)와 같이 재정의됩니다.

<p>$$J(\boldsymbol{x}) = \frac{1}{2m} \sum_{i=1}^m (h_{\theta}(\boldsymbol{x}^{(i)}) - y^{(i)})^{2} \tag{4}$$</p>

그리고 경사 하강법도 모든 매개변수에 대해서 적용해야합니다.

Repeat {<br>
$\quad \theta_{j} := \theta_{j} - \alpha \frac{\partial}{\partial \theta_{j}} J(\boldsymbol{\theta}) \quad (\frac{\partial}{\partial \theta_{j}} J(\boldsymbol{\theta}) = \frac{1}{m} \sum_{i=1}^m (h_{\theta}(\boldsymbol{x}^{(i)})-y^{(i)})x_{j}^{(i)}) \qquad \qquad \qquad \quad \, (5)$<br>
} (simultaneously update for every $j = 0, \ldots, n)$<br>

여기서 $x_{j}^{(i)}$는 $i$ 번째 훈련 데이터셋의 $j$ 번째 Feature를 의미합니다.

그런데, 우리가 입력으로 사용하는 Feature들은 범위가 굉장히 다양합니다. 예를들어, 방의 크기는 0-2000 $feet^2$ 와 같이 정의되고, 침실의 수는 1-5 와 같이 정의됩니다. 이를 가설함수에 그대로 적용하면, 어떤 Feature가 함수에 미치는 영향은 매우 크고, 또 어떤 Feature가 함수에 미치는 영향은 매우 작은 경우가 생길 수 있습니다. 물론 이를 막기 위해 각각의 Feature에 곱해지는 매개변수가 스스로 값을 조절하겠지만, 일반적으로 Feature들이 비슷한 Scale(크기)를 가지는 것이 모델 성능 향상에 좋습니다. 이를 **Feature Scaling**이라고 합니다. 서로 다른 Feature라도 가질 수 있는 값의 범위가 같다면 경사 하강법은 더욱 빠르게 수렴할 수 있습니다.

<center><img src="/assets/ml/05_multi_lr_pr/Fig01_feature_scaling.png" width="700" height="300"></center>
<center>Fig. 1. Contour Plots of Cost Function without/with Feature Scaling</center>

<br>
Fig. 1 은 늘상의 집값 예제를 사용하여 Feature Scaling을 적용하지 않은 비용함수와 적용한 비용함수를 등고선으로 나타낸 것입니다. Fig. 1 을 보면, 집의 크기와 방의 개수가 대략 2000:5 의 비율인 것을 볼 수 있습니다. 이렇게 될 경우, 비용함수는 Fig. 1 의 왼쪽 등고선과 같이 매우 찌그러져있는 타원 형태를 갖게 됩니다. (2000:5이면 그림보다 훨씬 찌그러져 있을겁니다.) 이 경우엔 비용함수가 수렴하기 위해 타원의 중앙을 찾아가는 과정이 굉장히 불안하고 오래걸리겠지요. 이제 집의 크기와 방의 개수를 1:1로 맞추어 주면, 비용함수는 Fig. 1의 오른쪽 등고선과 같이 안정적인 원형의 형태를 갖게 되어 정확하고 똑바른 방향으로 비용함수의 최솟값을 찾을 것입니다.

일반적으로 Feature Scaling을 할 때, Feature를 Feature가 가지고 있는 값의 최댓값으로 나누어 모든 Feature가 대략 -1과 1 사이의 값을 가지도록 만듭니다. 상수로 만든 $x_{0}$도 그 범위 안에 있죠. 값이 항상 이 범위 안에 있어야 하는 것은 아니지만, 어떤 Feature의 범위와 또 다른 Feature의 범위의 차이가 너무 크지 않도록만 해주시면 됩니다.

Feature를 Feature으로 평균값을 빼고, Feature가 가지고 있는 범위로 나누어, Scaling된 Feature의 평균 값이 0이 되도록 하기도 합니다. 즉, $x_{i} := (x_{i}-\mu_{i})/(\max(x_{i})-\min(x_{i}))$ 가 되는 것이죠. 이렇게 Scaling된 Feature의 평균 값이 0이 되도록 만드는 것을 Mean Normalization이라고 합니다. Mean Normalization을 적용하면, Feature는 -0.5과 0.5 사이의 값을 가지게 됩니다. ($x_{0}$에는 적용하지 않습니다.) 집의 크기 Feature를 Mean Normalization 하면, 평균값을 대략 1000이라고 가정하면 새로운 Feature $Size_{new}=\frac{Size_{old}-1000}{2000 - 0}$ 가 되겠지요.

<center><img src="/assets/ml/05_multi_lr_pr/Fig02_gd_debugging.png" width="400" height="300"></center>
<center>Fig. 2. Cost Function with the Number of Iterations (Good)</center>

<br>
경사 하강법이 잘 동작하는지 확인하려면 어떻게 해야할까요? 경사 하강법을 적용할 때마다 실제로 비용함수가 감소하고 있는지 확인해보면 됩니다. Fig. 2 는 경사 하강법을 적용할 때마다 비용함수가 어떻게 계산되는지 나타낸 그래프입니다. 매번 경사 하강법을 적용함에 따라 비용함수가 꾸준히 감소하는 것을 볼 수 있습니다. 만약 잘 동작하지 않는다면, 값이 증가하거나 튀는 현상이 발상하겠죠?

Fig. 2 에서는 경사 하강법을 약 300회 수행하였을 때 비용함수가 거의 수렴함을 볼 수 있습니다. 하지만, 모델에 따라 수렴할 때까지 걸리는 시간은 다양합니다. 어떤 모델은 300회, 심지어는 300만번을 수행하여 수렴할 수도 있습니다. 그래서 경사 하강법을 수행했을 때의 비용함수에 대한 그래프를 보면서 실제로 경사 하강법이 잘 동작하고 있는지 주기적으로 확인해주는 것이 중요합니다.

<center><img src="/assets/ml/05_multi_lr_pr/Fig03_lr.png" width="700" height="300"></center>
<center>Fig. 3. Cost Function with the Number of Iterations (Bad)</center>

<br>
경사 하강법이 잘 동작하지 않으면 Fig. 3 의 좌측 상단 그래프와 같이 경사 하강법을 적용하여도 비용함수가 증가할 것입니다. 이 때 비용함수가 증가하는 원인은, 비용함수를 잘 설계했다는 가정 하에서 Learning Rate가 너무 커서 발생하는 현상입니다. [경사 하강법 포스트](https://jiwonkkim.github.io/machine%20learning/2021/01/09/ml_gradient_descent/)에서도 언급되었지만, Learning Rate가 너무 크면 Fig. 3 의 오른쪽 그래프처럼 Local Minimum을 찾지 못하고 사방으로 튀면서 비용함수가 커지게 되거나, Fig. 3 의 좌측 하단 그래프와 같이 비용함수가 커졌다가 작아졌다를 반복하게 될 수 있습니다. 이러한 경우 Learning Rate를 적절하게 감소시켜야 합니다. (교수님이 증명은 따로 하시지 않았습니다.) 물론 Learning Rate를 너무 많이 감소시키면 수렴하기까지 많은 시간이 걸리게 되므로 적절한 Learning Rate를 적용해야합니다.

<br>
이어서 오늘 다룰 주제 중 하나인 **다항식 회귀(Polynomial Regression)**에 대해 이야기해보겠습니다. 길진 않습니다.

다시 집값 예제를 생각해봅시다. 이번 예제에서는 집의 크기 대신 집의 정면 너비(Frontage)와 집의 측면 너비(Depth)를 사용한다고 해봅시다. 이 때의 가설 함수는 하단의 식 (6)과 같습니다.
<p>$$h_{\theta}(\boldsymbol{x}) = \theta_{0} + \theta_{1} \times frontage + \theta_{2} \times depth \tag{6}$$</p>
가설함수를 이렇게 설계하는 경우, $frontage$ 를 Feature $x_{1}$ , $depth$ 를 Feature $x_{2}$ 로 볼 수 있습니다. 하지만 굳이 이렇게 Feature를 분리해서 가설함수를 설계할 필요가 없습니다. 두 Feature를 곱하면 집의 크기가 되게 때문이죠. 따라서 이러한 경우 $x_{1}$와 $x_{2}$를 합쳐서 새로운 Feature를 만드는게 낫습니다. 새로운 Feature $x$ 를 $frontage \times depth$ , 즉 집의 크기로 다시 정의하여 가설함수를 만들면 $h_{\theta} = \theta_{0} + \theta_{1}x$ 가 되겠습니다. 이런 식으로, 문제에 따라서 기존의 Feature를 사용하여 다른 Feature를 만들어내는 방법을 고안하여도 좋습니다. 그러한 예 중 하나가 바로 다항식 회귀입니다.

<center><img src="/assets/ml/05_multi_lr_pr/Fig04_polynomial_regression.png" width="500" height="300"></center>
<center>Fig. 4. Polynomial Regression</center>

<br>
Fig. 4 의 왼쪽 그래프는 집의 크기에 따른 집값 그래프입니다. 이 때, 파란색 화살표처럼 최고차항이 2인 경우로 가설함수를 설계하는 경우, 최고차항이 2이기 때문에 가설함수가 위로 볼록(Concave)한 형태를 띄우게 되므로, 적절하지 못한 가설함수가 되게 됩니다. 집의 크기가 넓어지는데 가격이 떨어지는 것은 이상하니까요. 따라서 우리는 초록색 화살표처럼 최고차항이 3인 다항식을 가설함수로 설계하는 것이 낫습니다.

Fig. 4 의 하단부에 나와있는 식을 보면, $x_{1}$ 을 $size$ 로, $x_{2}$ 를 $size^2$ 로, $x_{3}$ 를 $size^3$ 로 표현하여 가설함수를 설계했습니다. 이러한 경우, 각각의 Feature은 서로 매우 다른 범위를 가지게 되므로, Feature Scaling을 적용시키는 것이 중요합니다.

#### 이번 포스팅 정리
- 거의 대부분의 모델들은 2개 이상의 매개변수를 가진 가설함수를 가지고 있고, 이 때의 가설함수는 $h_{\theta}(\boldsymbol{x}) = \boldsymbol{\theta}^{T}\boldsymbol{x}$ 와 같이 벡터의 내적으로 표현할 수 있다.
- 경사 하강법을 통한 매개변수 업데이트 식 : $\theta_{j} := \theta_{j} - \alpha \frac{1}{m} \sum_{i=1}^m (h_{\theta}(\boldsymbol{x}^{(i)})-y^{(i)})x_{j}^{(i)})$
- Feature들이 비슷한 크기를 가지게 하는 것을 Feature Scaling이라고 한다.
- 경사 하강법이 정상적으로 동작하고 있는지 확인하려면 경사 하강법을 적용할 때마다 실제로 비용함수가 감소하고 있는지 확인하면 된다.
- 만약 경사 하강법이 정상적으로 동작하고 있지 않으면, 비용함수가증가하거나, 작아졌다 커졌다를 반복한다.
- 기존의 어떤 Feature를 사용하여 다른 Feature를 만들어서 가설함수를 설계할 수도 있다. 그 중 하나가 다항식 회귀이다.

##### 출처:
- [Andrew Ng - Machine Learning](https://www.coursera.org/learn/machine-learning)
