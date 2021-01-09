---
layout: post
title:  경사 하강법(Gradient Descent)
category: Machine Learning
tags: [Machine Learning, Andrew Ng, Gradient Descent, 경사 하강법, 기울기 하강법, Learning Rate, 훈련 계수, Batch]
comments: true
---

<br><br>
**이 글을 읽기 전에**<br>
- _해당 포스팅은 Coursera에 올라와 있는 Andrew Ng 교수님의 Machine Learning 강좌를 정리/각색하였습니다._
- _해당 포스팅은 개인적으로 공부한 내용을 정리한 것으로, 일부 내용에 오류가 있을 수 있습니다._
- _포스팅에 오류가 있을 시, 댓글로 남겨주시면 감사하겠습니다._

---

지난 포스팅에서는 모델 표현과 비용함수에 대하여 알아보았습니다. 이번 포스팅에서는 **경사 하강법(Gradient Descent, 기울기 하강법)**에 대하여 알아보도록 하겠습니다.<br>

우리는 기계 학습에서 모델의 성능을 올리기 위해 비용함수(Cost Function)을 최소화 시켜야 합니다. 그리고 현재 시점을 기준으로 기계 학습에서 비용함수를 최소화 시키기 위해 가장 많이 사용되는 매개변수 최적화 알고리즘은 바로 이 경사 하강법입니다.<br> 

[비용함수 포스트](https://jiwonkkim.github.io/machine%20learning/2021/01/08/ml_model_rep_cost_func/)에서도 언급했듯이, 우리가 사용하는 모델은 수 천, 수 만 이상의 매개변수들을 사용합니다. 하지만 설명을 위해 우리의 모델이 매개변수 $\theta_{0}$, $\theta_{1}$ 2개만 가지고 있다고 가정합니다. 우리의 목표는 비용함수 $J(\theta_{0}, \theta_{1})$을 최소화 하는 것입니다. 이제 경사 하강법을 적용하여 비용함수 $J(\theta_{0}, \theta_{1})$의 최솟값을 구하는 과정에 대해 이야기해 보겠습니다.<br>

먼저, 매개변수 $\theta_{0}$, $\theta_{1}$의 초기값을 추측해야 합니다. 사실 어떤 값인지는 중요하지 않지만, 일반적으로 0으로 초기화합니다. 그리고 조금씩 $\theta_{0}$, $\theta_{1}$을 바꾸어 $J(\theta_{0}, \theta_{1})$를 줄여나갑니다.<br>

<center><img src="/assets/ml/04_gradient_descent/Fig01_local_minimum_01.png" width="600" height="300"></center>
<center>Fig. 1. Finding Local Minimum with Gradient Descent (1)</center>

<br>
목적함수 $J(\theta_{0}, \theta_{1})$가 Fig. 1 처럼 표현된다고 합시다. 그래프가 마치 언덕이 있는 지형같이 생겼군요. 우리가 이 지형의 어딘가에 있다고 할 때, 어떻게 하면 이 언덕을 작은 걸음걸이로 최대한 빠르게 내려갈 수 있을까요? 주위를 둘러보고 가장 경사가 가파른 곳으로 몇 걸음 내딛고, 다시 주위를 둘러보고 가장 경사가 가파른 곳으로 몇 걸음 내딛고, 이를 반복하면 가장 빠르게 언덕을 내려갈 수 있습니다.<br>
만약 우리가 Fig. 1 의 빨간색 동그라미 부분을 시작으로 언덕을 내려가기 시작하면, 그림에 표시된 선을 따라가다가 초록색 동그라미 부분에 도착하게 되겠지요.<br>

<center><img src="/assets/ml/04_gradient_descent/Fig02_local_minimum_02.png" width="600" height="300"></center>
<center>Fig. 2. Finding Local Minimum with Gradient Descent (2)</center>

<br>
만약 우리가 Fig. 2 의 오른쪽 빨간색 동그라미 부분을 시작으로 언덕을 내려가기 시작하면, 그림에 표시된 선을 따라가다가 초록색 동그라미 부분에 도착하게 되겠지요. **즉, 시작 위치에 따라 내려가는 방향과 목적지가 달라질 수 있습니다.**<br>

경사 하강법은 이러한 언덕 내려가기와 유사합니다. 목적함수의 어떤 위치에서 시작하여, 그 목적함수를 작게 만드는 방향으로 위치, 즉 매개변수를 변경해 나가는 것이죠. 이를 수학적으로 표현하면 다음과 같습니다.<br>

repeat until convergence {<br>
$\quad \theta_{j} := \theta_{j} - \alpha \frac{\partial}{\partial \theta_{j}} J(\theta_{0}, \theta_{1})$ $\qquad$ (for $j=0$ and $j=1$) $\qquad \qquad \qquad \qquad \qquad \qquad \;(1)$<br>
}<br>

여기서 $:=$(Colon Equal)는 할당한다(Assign)는 기호입니다. 컴퓨터에서는 값을 덧씌운다는 의미가 되죠. 예를 들면, $a:=b$는 $a$에 $b$의 값을 씌운다는 의미가 됩니다. 그리고 컴퓨터에서 $a=b$는 Truth Assertion이라고 합니다. (처음 들어봐서 한글로는 뭐라고 하는지 모르겠네요.) 즉, $a$와 $b$의 값이 같다고 주장하는 겁니다. 일반적으로 우리가 일상에서 사용하는 코드에서는 $a$에 $a+1$을 할당한다는 의미를 $a=a+1$과 같이 표현하지만, 여기서는 $=$을 Truth Assertion으로 보고 $a=a+1$과 같은 표현을 틀린 표현으로 보겠습니다. ($a$는 $a+1$이 될 수 없으니까요.)<br>

위의 식 (1)에 나와있는 $\alpha$는 **Learning Rate(훈련 비율)**라고 합니다. 경사 하강법을 사용할 때 어떤 폭으로 움직일지에 대한 변수입니다. 언덕 내려가기로 비유한다면 내려갈 때의 보폭을 의미합니다. 따라서 $\alpha$가 매우 크다면 꽤 공격적인 경사 하강법을 적용하는 것이고, $\alpha$가 매우 작다면 아주 조금씩 경사 하강법을 적용하게 되는거죠. 그리고 $\frac{\partial}{\partial \theta_{j}} J(\theta_{0}, \theta_{1})$의 의미는 비용함수를 매개변수 $\theta_{j}$에 대해 편미분한다는 것입니다. 즉, 매개변수에 대한 비용함수의 기울기입니다.

**결론적으로, 경사 하강법이란 매개변수를 비용함수 $J(\boldsymbol{\theta})$가 해당 매개변수에 대해 감소하는 방향으로 모든 매개변수에 대해 반복적으로 업데이트하여, 비용함수가 적절한 Local Minimum에 수렴할 때까지 반복하는 매개변수 최적화 기법입니다.** 저는 개인적으로 매개변수에 대한 목적함수의 미분값이 0이 되는 지점, 즉 극값을 찾을 때까지 매개변수를 바꾸어 나간다고 생각하니 이해가 되더군요.<br>

이 때 주의해야할 점은 매개변수의 업데이트는 동시에 이루어져야 한다는 것입니다. 이를 위의 예제를 사용하여 수식으로 나타내면 다음과 같습니다.<br>
$temp0 := \theta_{0} - \alpha \frac{\partial}{\partial \theta_{0}} J(\theta_{0}, \theta_{1})$<br>
$temp1 := \theta_{1} - \alpha \frac{\partial}{\partial \theta_{1}} J(\theta_{0}, \theta_{1})$<br>
$\theta_{0} := temp0$<br>
$\theta_{1} := temp1$<br>

즉, 미리 업데이트될 매개변수의 값을 미리 계산해놓은 다음, 모든 매개변수에 대해 계산이 끝난 이후에 매개변수를 업데이트 해야 합니다. 만약 매개변수가 동시에 업데이트하지 않는다고 하면, $\theta_{0}$을 업데이트 한 후에 $\theta_{1}$에 대한 새로운 매개변수를 계산하는 과정에서 비용함수 $J(\theta_{0},\theta_{1})$의 값이 변경되기 때문입니다. (왜 값이 동시에 바뀌지 않으면 안된다는 설명은 하지 않으셨습니다. 그냥 이것이 경사 하강법의 방식이라고만 설명하셨습니다. 아마 이렇게 하는것이 자연스럽기 때문일까요..?)<br>

경사 하강법을 좀 더 직관적으로 이해하기 위해, 매개변수 $\theta_{1}$만 가지고 있는 모델을 가정해보겠습니다.<br>

<center><img src="/assets/ml/04_gradient_descent/Fig03_gradient_descent.png" width="400" height="300"></center>
<center>Fig. 3. Gradient Descent with One Parameter</center>

<br>
Fig. 3 의 위쪽 그래프를 보시죠. 그래프에 나와있는 빨간 점이 현재 $\theta_{1}$의 위치라고 합시다. 빨간 점에서의 $J(\theta_{1})$의 미분값은 **양수**입니다. 따라서 $\theta_{1} := \theta_{1} - \alpha \times (양수)$가 되므로, 새로 업데이트될 $\theta_{1}$의 값은 작아지고, 이를 통해 $J(\theta_{1})$의 값이 Local Minimum에 가까워지게 됩니다.<br>

이어서 Fig. 3 의 아래쪽 그래프를 보시죠. 그래프에 나와있는 빨간 점이 현재 $\theta_{1}$의 위치라고 합시다. 빨간 점에서의 $J(\theta_{1})$의 미분값은 **음수**입니다. 따라서 $\theta_{1} := \theta_{1} - \alpha \times (음수)$가 되므로, 새로 업데이트될 $\theta_{1}$의 값은 커지고, 이를 통해 $J(\theta_{1})$의 값이 Local Minimum에 가까워지게 됩니다.<br>

<center><img src="/assets/ml/04_gradient_descent/Fig04_learning_rate.png" width="450" height="300"></center>
<center>Fig. 4. Effect of Learning Rate</center>

<br>
이제 그림을 통해 Learning Rate가 경사 하강법에 미치는 영향에 대해서 알아보겠습니다.<br>
Fig. 4 의 위쪽 그래프는 Learning Rate가 작을 때의 경사 하강법 과정, 아래쪽 그래프는 Learning Rate가 클 때의 경사 하강법 과정입니다. 각각의 포인트와 화살표는 매개변수와 그 매개변수가 업데이트될 값을 나타냅니다. Learning Rate가 너무 작으면 비용함수가 Local Minimum에 도달하기 까지 많은 이동이 필요합니다. 반대로 Learning Rate가 너무 크면 적절한 Local Minimum을 찾기가 어려워지고, 수렴은 커녕 발산하는 경우가 생길수도 있습니다. 이렇게 매개변수가 적절한 Local Minimum을 찾지 못하고 사방으로 튀는 현상을 **Overshooting**이라고 합니다.<br>

<center><img src="/assets/ml/04_gradient_descent/Fig05_convergence.png" width="500" height="300"></center>
<center>Fig. 5. Global Minimum and Local Minimum</center>

<br>
Fig. 5 처럼 경사 하강법을 적용하여 목적함수의 Local Minimum에 도달한 매개변수 $\theta_{1}$가 있다고 합시다. 이 때 목적함수가 이 Local Minimum보다 작은 값을 가진 Global Minimum이 존재한다고 하여도, 이 $\theta_{1}$에 경사 하강법을 더 적용해도 Global Minimum에는 도달하지 않고 계속 Local Minimum에 빠져있게 됩니다. **즉, 경사 하강법을 사용하면 목적함수가 Local Minimum에는 도달할 수 있지만, Global Minimum을 찾는다는 보장은 없습니다.** <br>

이제 비용함수를 매개변수 $\theta_{j}$에 대해 편미분 해 보겠습니다. MSE를 사용하는 비용함수의 편미분 식은 다음과 같습니다.

$\frac{\partial}{\partial \theta_{j}} J(\theta_{0}, \theta_{1}) = \frac{\partial}{\partial \theta_{j}} \frac{1}{2m} \sum_{i=1}^M (h_{\theta}(x^{(i)}) - y^{(i)})^2$
$\qquad \qquad \quad \; = \frac{\partial}{\partial \theta_{j}} \frac{1}{2m} \sum_{i=1}^M (\theta_{0} + \theta_{1}x^{(i)} - y^{(i)})^2 \qquad \qquad \qquad \qquad \qquad \qquad \qquad \; \; \, (2)$
$j = 0: \; \frac{\partial}{\partial \theta_{0}} J(\theta_{0}, \theta_{1}) = \frac{1}{m} \sum_{i=1}^M (h_{\theta}(x^{(i)}) - y^{(i)}) \tag{3}$
$j = 1: \; \frac{\partial}{\partial \theta_{1}} J(\theta_{0}, \theta_{1}) = \frac{1}{m} \sum_{i=1}^M (h_{\theta}(x^{(i)}) - y^{(i)})x^{(i)} \tag{4}$

이를 경사 하강법에 적용하려면 식 (3) 식 (4)를 식 (1)에 대입하면 되겠습니다. MSE의 분모에 넣지 않아도 되는 2를 넣는 이유가 미분을 하면 없어지기 때문인게 맞았네요.

그런데 이 때, 경사 하강법을 사용해서 모델이 정상적으로 동작할 수 없는 Local Minimum을 찾게 될 수도 있지 않을까요? 교수님은 **선형회귀 문제에 대해서는** 괜찮다고 하십니다. 왜냐하면, 선형회귀의 비용함수는 항상 하단의 Fig. 6 와 같은 활모양으로 나타나기 때문입니다. 이렇게 아래로 볼록한 활모양 함수를 **Convex Function(볼록함수)** 라고 합니다. (이유에 대해서는 잘 모르겠네요... 추후에 알게 되면 포스팅하도록 하겠습니다.) Convex Function은 Global Minimum만 가지고 있고, Local Minimum을 가지고 있지 않습니다. 따라서 선형회귀에서 경사 하강법을 사용하면 목적함수는 항상 Global Minimum을 향하게 됩니다. 물론 다른 문제에서는 Local Minimum으로 인해 생기는 문제가 있겠지요. 이에 대해서는 추후 포스팅하도록 하겠습니다.

<center><img src="/assets/ml/04_gradient_descent/Fig06_cost_func_3D.png" width="500" height="300"></center>
<center>Fig. 6. Cost Function $J(\theta_{0},\theta_{1})$</center>

<br>
그리고 마지막으로, **Batch(일괄)**에 대해서 이야기해 보겠습니다. 경사 하강법을 사용할 때, 비용함수를 일정 개수의 데이터셋에 대해 합으로 사용해야 합니다. 그렇지 않고 데이터 1개 1개에 대해서 경사 하강법을 적용한다면, 컴퓨터가 이것을 계산하는데 오랜 시간이 걸리게 되겠죠. 그래서 경사 하강법을 한 번 적용하는데 사용되는 훈련 데이터셋의 개수를 의미합니다. 하지만 반대로 Batch가 너무 크면 컴퓨터에서 사용되는 메모리 제약이 커지겠지요. 따라서 적절한 Batch 크기를 골라주는 것도 기계학습에 있어 중요합니다.

다음 포스팅에서는 이 선형대수 기초에 대해서 이야기해보도록 하겠습니다. (본래 이 블로그에서 선형대수에 대해서도 다루고 있지만, 다루고 있는 내용이 어렵고 새로운 관점으로 선형대수를 보고 있기도 하고, 교수님이 강의의 내용 중 하나로 선형대수를 간단하게 복습하시는 관계로 짧게 선형대수에 대해 포스팅하도록 하겠습니다.)

#### 이번 포스팅 정리
- 경사 하강법이란, 모델의 성능을 올리기 위해 비용함수를 최소화 할 때 사용되는 알고리즘이다.
- 경사 하강법은 비용함수의 Global Minimum을 보장하지 않으며, Local Minimum을 보장한다. (Local Minimum을 빠져 나와 Global Minimum을 찾을 수 있도록 도와주는 방법이 여러가지 있다.)
- 경사 하강법은 비용함수를 매개변수에 대해 편미분하여, 비용함수를 줄일 수 있는 방향으로 매개변수를 업데이트한다. (식 (1) 참조)
- 경사 하강법에서 Learning Rate가 너무 작으면 Local Minimum에 도달하는데 많은 계산이 요구될 수 있고, Learning Rate가 너무 크면 Local Minimum에 도달하지 못하고 비용함수가 사방으로 튀는 Overshooting 문제가 발생할 수 있다.
- 선형회귀 문제에서 비용함수는 항상 Convex Function 이므로, 경사 하강법을 사용하면 비용함수는 항상 Global Minimum에 도달할 수 있다.
- Batch란 경사 하강법을 한 번 적용할 때 사용되는 훈련 데이터셋의 개수이다.

##### 출처:
- [Andrew Ng - Machine Learning](https://www.coursera.org/learn/machine-learning)
