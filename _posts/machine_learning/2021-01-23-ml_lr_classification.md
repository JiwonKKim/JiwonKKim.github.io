---
layout: post
title:  로지스틱 회귀(Logistic Regression)와 분류(Classification)
category: Machine Learning
tags: [Machine Learning, Andrew Ng, Logistic Regression, 로지스틱 회귀, Classfication, 분류]
comments: true
---

<br><br>
**이 글을 읽기 전에**<br>
- _해당 포스팅은 Coursera에 올라와 있는 Andrew Ng 교수님의 Machine Learning 강좌를 정리/각색하였습니다._
- _해당 포스팅은 개인적으로 공부한 내용을 정리한 것으로, 일부 내용에 오류가 있을 수 있습니다._
- _포스팅에 오류가 있을 시, 댓글로 남겨주시면 감사하겠습니다._

---

이번 포스팅에서는 **분류(Classification)**와 분류에 사용되는 **로지스틱 회귀(Logistic Regression)**에 대해 알아보겠습니다.

분류 문제는 [기계학습 기초 (1)](https://jiwonkkim.github.io/machine%20learning/2020/12/23/ml_introduction_01/)에서 잠깐 언급한 적이 있습니다. 종양의 크기에 따라 유방암이 악성인지 양성인지 분류하는 예를 들어 설명했었지요. 이외에도 자신에게 이메일이 도착했을 때 그 메일이 스팸인지 아닌지 구분하는 것도 분류 문제에 포함됩니다. 이러한 문제들 모두 0 또는 1 둘 중의 하나의 값을 가지는 변수 y를 예측하는 문제입니다. 0으로 표현하는 분류를 보통 음성 분류((Negative Class, e.g. 양성 종양)라고 부르고, 1로 표현하는 분류를 양성 분류(Positive Class, e.g. 악성 종양)라고 합니다. 물론 항상 1을 양성, 0을 음성으로 표현하는 것은 아니지만 0은 보통 찾고자 하는 대상이 없는 경우로 두고, 1은 찾고자 하는 대상이 있는 경우로 두는 것이 직관적입니다. 이렇게 2개의 Class를 분류하는 것을 **이진 분류(Binary Classification)**이라고 합니다. 3개 이상의 Class를 분류하는 것은 **다중 분류(Multiclass Classification)**라고 하구요.
 
<center><img src="/assets/ml/07_lr_classification/Fig01_BreastCancer_01.png" width="500" height="300"></center>
<center>Fig. 1. Breast Cancer with Linear Regression (1)</center>

<br>
Fig. 1 은 종양의 크기에 따라 유방암이 악성인지 양성인지를 표현한 데이터 그래프입니다. 종양의 크기에 따라 유방암이 악성인지 양성인지를 분류하기 위한 가설함수를 식 (1)과 같이 선형 회귀(Linear Regression) 방식으로 설계한다고 합시다.
<p>$$h_{\theta}(x)=\boldsymbol{\theta}^{T} \boldsymbol{x} \tag{1}$$</p>
<p><center>If $h_{\theta}(x) \geq 0.5$ , predict "$y=1$" $\quad$ If $h_{\theta}(x) < 0.5$ , predict "$y=0$"</center></p>

지금까지 사용했던 선형 회귀와 달리, 입력된 종양 크기를 입력으로 사용한 가설함수를 통해 양성(0)인지 악성(1)인지 분류해야 합니다. 따라서, $h_{\theta}(x)$ 의 결과값이 0.5과 되는 곳을 **Threshold(결정 임계값)**로 설정하여, 가설함수가 0.5보다 크거나 같은 경우는 y를 1, 즉 악성으로 보고, 가설함수가 0.5보다 작으면 y를 0, 즉 양성으로 봅니다. 따라서, Fig. 1 에서 Threshold가 0.5가 되는 종양의 크기를 중앙으로 하여 왼쪽 부분은 양성, 오른쪽 부분은 악성이 됩니다.

이렇게만 보면 선형 회귀가 마치 합리적으로 작동하는 것처럼 보일 수 있습니다. 그러면 이제 Fig. 1 의 데이터에서 종양의 크기가 매우 큰 악성 종양 데이터가 추가되었다고 합시다.

<center><img src="/assets/ml/07_lr_classification/Fig02_BreastCancer_02.png" width="700" height="300"></center>
<center>Fig. 2. Breast Cancer with Linear Regression (2)</center>

<br>
Fig. 2 와 같이 데이터가 추가될 경우, Threshold가 핑크색 점에서 파랑색 점의 위치로, 즉 오른쪽으로 이동하게 됩니다. 이렇게 되면 일부 악성 종양을 음성 종양으로 잘못 판단하게 되는 경우가 생겨버립니다.

그러므로, 선형 회귀를 분류 문제에 적용하는 것은 좋지 않습니다. 첫 번째 예시에서 데이터를 추가하기 전에는 **운 좋게** 특정 예시들에 대해 잘 동작하는 가설함수를 설계했지만, 선형 회귀를 분류 데이터에 적용하면 대부분은 잘못될 것입니다.
 
이진 분류 문제는 y가 0 아니면 1의 값만을 가지지만, 선형 회귀를 사용해 설계한 가설함수는 1보다 크거나 0보다 작을 수 있습니다. 따라서, 우리는 0과 1 사이의 값을 갖는 가설함수를 사용하는 로지스틱 회귀를 사용하여 분류 문제를 해결할 것입니다.

설명에 앞서, 지금까지 가설을 나타내는 함수, 즉 가설 함수라는 용어를, 가설을 표현하기 위해 사용하는 함수인 **가설 표현(Hypothesis Representation)**이라고 정정하여 칭하겠습니다. (저도 왜인진 모르겠네요. 교수님이 가설함수를 이렇게 바꿔서 부르기 시작하시네요.)

<p>$h_{\theta}(x) = g(\boldsymbol{\theta}^{T} \boldsymbol{x}), \; g(\boldsymbol{z}) = \frac{1}{1 + e^{-\boldsymbol{z}}} \quad (0 \leq h_{\theta}(x) \leq 1) \tag{2}$</p>
<p>$h_{\theta}(x) = \frac{1}{1 + e^{-\boldsymbol{\theta}^T \boldsymbol{x}}} \tag{3}$</p>

식 (2)는 로지스틱 회귀의 가설 표현입니다. 식에서 $g$ 는 **Sigmoid** 함수 또는 **Logistic** 함수라 불리우며, 이 용어로 인해 로지스틱 회귀분석이라는 이름이 탄생했습니다. 식 (2)는 $\boldsymbol{z}$ 를 $\boldsymbol{\theta}^T \boldsymbol{x}$로 치환하여 식 (3)과 같이 표현할 수도 있습니다.

<center><img src="/assets/ml/07_lr_classification/Fig03_sigmoid.png" width="400" height="300"></center>
<center>Fig. 3. Sigmoid Function</center>

<br>
Fig. 3 은 Sigmoid 함수를 그린 것입니다. 보시다시피 Sigmoid 함수는 0과 1 사이의 값을 가지며, $\boldsymbol{z}$ 가 음의 무한대 값을 가지면 $g(\boldsymbol{z})$ 는 0으로 수렴하고, $\boldsymbol{z}$ 가 양의 무한대 값을 가지면 $g(\boldsymbol{z})$ 는 1로 수렴합니다. Fig. 3 의 Sigmoid 함수는 $\boldsymbol{z}$ 가 0이 되는 지점을 기준으로 Threshold 0.5의 값을 가지고 있습니다. 이 함수로 Threshold가 0.5보다 크거나 같은 값들, 즉 $\boldsymbol{z}$ 가 0보다 크거나 같은 값들은 1로 분류할 수 있고, Threshold가 0.5보다 작은 값들, 즉 $\boldsymbol{z}$ 가 0보다 작은 값들은 0로 분류할 수 있습니다.

이제 예제와 함께 가설 표현을 해석해 보겠습니다. 가설 표현 $h_{\theta}(x)$ 는 입력 $x$ 에 따라 y가 1, 즉 악성 종양으로 분류될 확률이라고 볼 수 있습니다. 예를 들어, 어떤 입력 $x$에 따라 가설 표현 $h_{\theta}(x)=0.7$ 이라고 한다면, Feature x를 가지고 있는 환자의 y가 1일 확률은 0.7이라는 것이 됩니다. 즉, 이 환자는 악성 종양을 가지고 있을 확률이 70%인 것이지요. 따라서 이러한 가설 표현을 식 (4)와 같이 재정의할 수 있습니다.
<p>$h_{\theta}(x) = p(y=1|x;\theta) \tag{4}$</p>

식 (4)는 가설 표현 $h_{\theta}(x)$ 의 매개변수가 $\theta$ 이고, x라는 Feature를 가지고 있을 때 y가 1일 확률이라는 의미입니다. 이 식은 probability that y=1, given x, parameterized by theta 라고 읽습니다.

식 (4)처럼 Feature x를 가지고 있는 환자의 y가 1일 확률과 반대되는 개념인 y가 0일 확률 또한 존재합니다. 여기서 우리는 y가 0 또는 1의 값만 가질 수 있기 때문에, y=0일 확률과 y=1일 확률을 더하면 1이 됩니다. 이를 식으로 표현하면 하단의 식 (5)와 같습니다.

<p>$h_{\theta}(x) = p(y=0|x;\theta) + p(y=1|x;\theta) = 1, \quad p(y=0|x;\theta) = 1 - p(y=1|x;\theta) \tag{5}$</p>

이제 가설 표현이 무엇을 기준으로 Class를 분류하는지 알아봅시다.

앞서 언급했던 것처럼, Sigmoid 함수에서 $\boldsymbol{z}$ 가 0보다 크거나 같은 값들은 1로 분류할 수 있고, $\boldsymbol{z}$ 가 0보다 작은 값들은 0로 분류할 수 있습니다. 여기서 $\boldsymbol{z}$ 는 $\boldsymbol{\theta}^{T} \boldsymbol{x}$ 이구요. 즉, $\boldsymbol{\theta}^{T} \boldsymbol{x}$ 에서 매개변수 $\boldsymbol{\theta}$ 를 잘 조정하여 y값을 예측해야 합니다.

<center><img src="/assets/ml/07_lr_classification/Fig04_decision_boundary.png" width="400" height="300"></center>
<center>Fig. 4. Decision Boundary</center>

<br>
Fig. 4 와 같이 데이터가 O와 X로 분포해 있다고 가정합시다. 이 때의 가설 표현를 하단의 식 (6)처럼 정의합니다.

<p>$h_{\theta}(x) = g({\theta}_{0} + {\theta}_{1} x_{1} + {\theta}_{2} x_{2} ) , \quad \boldsymbol{\theta} = \begin{bmatrix} -3 & 1 & 1 \end{bmatrix}^{T} \tag{6}$</p>

가설 표현 $h_{\theta}(x)$ 에 매개변수 $\boldsymbol{\theta}$ 를 대입하면, 가설 표현 $h_{\theta}(x)$는 $ \boldsymbol{z} = \boldsymbol{\theta}^T \boldsymbol{x} = -3 + x_{1} + x_{2} \geq 0 $ 이면 y=1로 예측합니다. 즉, Fig. 4 에서 $x_{1} + x_{2} \geq 3 $ 인 부분이 곧 y=1이 되는 부분입니다. 따라서, 우리는 $x_{1} + x_{2} = 3 $ 이 되는 직선을 찾고, 그 직선을 기준으로 y가 0이 되는 영역과 1이 되는 영역을 구분할 수 있게 됩니다. $x_{1} + x_{2} = 3 $ 이 되는 직선은 Fig. 4 에 그려져 있는 분홍색 직선입니다. 이 분홍색 직선의 윗 영역이 y=1인 영역이 되겠고, 해당 영역은 X라는 데이터로만 구성되어 있습니다. 반대로 분홍색 직선의 아랫 영역이 y=0인 영역이 되겠고, 해당 영역은 O라는 데이터로만 구성되어 있습니다. 이렇게 분홍색 직선과 같이 어떤 데이터를 분류할 때 기준이 되는 선을 **Decision Boundary(결정 경계)**라고 합니다. 

직선형 Decision Boundary와 달리 Non-Linear Decison Boundary도 있습니다.

<center><img src="/assets/ml/07_lr_classification/Fig05_non-linear_db.png" width="400" height="300"></center>
<center>Fig. 5. Non-Linear Decision Boundary</center>

<br>
Fig. 5 와 같이 데이터가 O와 X로 분포해 있다고 가정합시다. 이 때 [다항식 회귀](https://jiwonkkim.github.io/machine%20learning/2021/01/12/ml_multi_lr_pr/)에서 보았던 것처럼 가설 표현을 다항식으로 나타낸다고 하면, 이 때의 가설 표현은 하단의 식 (7)처럼 정의합니다.
<p>$h_{\theta}(x) = g({\theta}_{0} + {\theta}_{1} x_{1} + {\theta}_{2} x_{2} + {\theta}_{3} x_{1}^{2} + {\theta}_{4} x_{2}^{2}) , \quad \boldsymbol{\theta} = \begin{bmatrix} -1 & 0 & 0 & 1 & 1 \end{bmatrix}^{T} \tag{7}$</p>

위에서와 마찬가지로 가설 표현 $h_{\theta}(x)$ 에 매개변수 $\boldsymbol{\theta}$ 를 대입하면, 가설 표현 $h_{\theta}(x)$는 $ \boldsymbol{z} = \boldsymbol{\theta}^T \boldsymbol{x} = -1 + x_{1}^{2} + x_{2}^{2} \geq 0 $ 이면 y=1로 예측합니다. 즉, Fig. 5 에서 $x_{1}^{2} + x_{2}^{2} \geq 1 $ 인 부분이 곧 y=1이 되는 부분입니다. $x_{1}^{2} + x_{2}^{2} = 1$ 이 되는 선은 Fig. 5 에 그려져 있는 분홍색 원입니다. 이 분홍색 선의 바깥쪽 영역이 y=1인 영역이 되겠고, 해당 영역은 X라는 데이터로만 구성되어 있습니다. 반대로 분홍색 원의 안쪽 영역이 y=0인 영역이 되겠고, 해당 영역은 O라는 데이터로만 구성되어 있습니다.

이러한 Decision Boundary는 훈련 데이터셋을 통해 학습되겠지만, Decision Boundary는 훈련 데이터셋의 특성이 아닌 가설 표현의 특성입니다. 또한 다양한 매개변수 개수와 차수를 사용함으로써 더 복잡한 Decision Boundary를 설계할 수 있습니다.

이제 이러한 가설 표현이 우리의 데이터에 맞도록 매개변수들을 설정해주어야 합니다.

매개변수 탐색을 위해 선형 회귀에서 사용했던 Mean Square Error 비용함수(Cost Function)를 생각해봅시다.

<p>$J(\boldsymbol{\theta}) = \frac{1}{m} \sum_{i=1}^m \frac{1}{2} (h_{\theta}(x^{(i)}) - y^{(i)})^{2} \tag{8}$</p>
<p><center>$Cost(h_{\theta}(\boldsymbol{x}), \boldsymbol{y}) = \frac{1}{2} (h_{\theta}(\boldsymbol{x}) - \boldsymbol{y})^{2}$</center></p>

식 (8)에서 $\frac{1}{2} (h_{\theta}(x^{(i)}) - y^{(i)})^{2}$ 를 위첨자를 제거하여 $Cost(h_{\theta}(\boldsymbol{x}), \boldsymbol{y})$ 로 치환한 후 Cost라는 함수로 대체하여 식을 표기합니다. 즉, 비용함수 $J(\boldsymbol{\theta})$ 는 $Cost$ 라는 함수를 훈련 데이터셋 개수만큼 합하여 평균을 낸 값이 됩니다. 이 비용함수는 선형 회귀를 다룰 때에는 문제될 것이 없었습니다. 하지만 우리는 지금 로지스틱 회귀를 사용하고 있기 때문에, $Cost$ 함수는 매개변수 $\boldsymbol{\theta}$ 에 대해 Convex Function(볼록 함수)이 아닙니다. $Cost$ 함수는 $h_{\theta}(\boldsymbol{x})$ 가 선형성을 가질 때만 Convex Function이고, $\frac{1}{1 + e^{-\boldsymbol{\theta}^T \boldsymbol{x}}}$ 와 같은 비선형성을 가지는 함수에 대해서는 Non-Convex Function이기 때문입니다. $h_{\theta}$ 가 Sigmoid 함수일 때의 비용함수 그래프는 Fig. 6 와 같이 수많은 Local Minimum(극솟값)을 가지는 형태를 가지게 됩니다.

<center><img src="/assets/ml/07_lr_classification/Fig06_cost_function.png" width="400" height="300"></center>
<center>Fig. 6. Cost Function $J(\boldsymbol{\theta})$ when $h_{\theta} = \frac{1}{1 + e^{-\boldsymbol{\theta}^T \boldsymbol{x}}}$</center>

<br>
따라서, 로지스틱 회귀에서는 MSE와 같은 비용함수를 사용하는 대신 다른 비용함수를 사용합니다.

<p>$Cost(h_{\theta}(\boldsymbol{x}), \boldsymbol{y})  = \begin{cases}
-\log (h_{\theta}(\boldsymbol{x})) & \mbox{if }y = 1 \\
-\log (1 - h_{\theta}(\boldsymbol{x})) & \mbox{if }y = 0
\end{cases} \tag{9}$</p>

<center><img src="/assets/ml/07_lr_classification/Fig07_cost_function_y=1.png" width="700" height="300"></center>
<center>Fig. 7. $Cost$ when $y=1$</center>

<br>
Fig. 7 의 왼쪽 그래프는 $y=1$ 일 때 $h_{\theta}(\boldsymbol{x})$ 에 따른 $Cost$ 의 값입니다. $Cost$ 가 왜 이러한 형태를 가지게 되는지는 Fig. 7 의 오른쪽 그래프를 참고하면 알 수 있습니다. $h_{\theta}(\boldsymbol{x})$ 는 0과 1 사이의 값을 갖기 때문에, $- \log (h_{\theta}(\boldsymbol{x}))$ 는 Fig. 7 의 오른쪽 그래프에 분홍색 곡선과 같은 형태를 띄게 되는 것이죠.

우리가 만약 $h_{\theta}(\boldsymbol{x})$ 를 통해 y를 정확하게 예측했다면 $Cost$ 는 0이 됩니다. 즉, $y=1$ 이고 $h_{\theta}(\boldsymbol{x})$ 가 1일 때 $Cost$ 또한 0이 됩니다. 반대로, $h_{\theta}(\boldsymbol{x})$ 가 예측에 완전히 실패했다면 $Cost$ 는 양의 무한대로 발산하게 됩니다. 즉, $y=1$ 이고 $h_{\theta}(\boldsymbol{x})$는 0으로 수렴할 때 $Cost$ 는 양의 무한대로 발산하게 됩니다. 예측한 결과가 맞으면 $Cost$ 을 0으로 하여 패널티를 주지 않고, 틀리면 $Cost$ 가 양의 무한대로 만들어 패널티를 줍니다. 우리가 원하던 결과입니다. 

<center><img src="/assets/ml/07_lr_classification/Fig08_cost_function_y=0.png" width="700" height="300"></center>
<center>Fig. 8. $Cost$ when $y=0$</center>

이어서, Fig. 8 의 왼쪽 그래프는 $y=0$ 일 때 $h_{\theta}(\boldsymbol{x})$ 에 따른 $Cost$ 의 값입니다. $y=1$ 일 때와 마찬가지로, $Cost$ 가 왜 이러한 형태를 가지게 되는지는 Fig. 8 의 오른쪽 그래프를 참고하면 알 수 있습니다. $h_{\theta}(\boldsymbol{x})$ 는 0과 1 사이의 값을 갖기 때문에, $\log (1 - h_{\theta}(\boldsymbol{x}))$ 는 Fig. 7 의 오른쪽 그래프와 같은 형태를 띄게 되는 것이죠.

$y=1$ 일 때와 마찬가지로, $y=0$ 이고 $h_{\theta}(\boldsymbol{x})$ 가 0일 때 $Cost$ 또한 0이 됩니다. 반대로, $y=0$ 이고 $h_{\theta}(\boldsymbol{x})$는 1으로 수렴할 때 $Cost$ 는 양의 무한대로 발산하게 됩니다. 예측한 결과가 맞으면 $Cost$ 을 0으로 하여 패널티를 주지 않고, 틀리면 $Cost$ 가 양의 무한대로 만들어 패널티를 줍니다. 이 또한 우리가 원하던 결과입니다. 

하지만 이렇게 $Cost$ 를 2가지로 나누어 비용함수를 계산하면 복잡하게 생각될 수 있습니다. 따라서 케이스를 나누는 것을 하나의 방정식으로 압축하도록 하겠습니다.

<p>$Cost(h_{\theta}(\boldsymbol{x}), \boldsymbol{y}) = -y \log (h_{\theta}(\boldsymbol{x})) - (1-y) \log (1-h_{\theta}(\boldsymbol{x})) \tag{10}$</p>
<p>$\mbox{if } y=1 : Cost(h_{\theta}(\boldsymbol{x}), \boldsymbol{y}) = - \log(h_{\theta}(\boldsymbol{x})) \tag{11}$</p>
<p>$\mbox{if } y=0 : Cost(h_{\theta}(\boldsymbol{x}), \boldsymbol{y}) = - \log(1 - h_{\theta}(\boldsymbol{x})) \tag{12}$</p>

식 (10)에 $y=1$ 을 대입하면 식 (11)이 되고, 식 (10)에 $y=0$ 를 대입하면 식 (12)가 되기 때문에 식 (10)은 식 (9)를 만족하는 것을 확인할 수 있습니다.

식 (10)을 통해 비용함수 $J(\boldsymbol{\theta})$ 를 재정의하면 식 (13)과 같습니다.

<p>$\eqalign{
J(\boldsymbol{\theta})
&=
\frac{1}{m} \sum_{i=1}^m Cost(h_{\theta}(\boldsymbol{x}^{(i)}), y^{(i)}) \\
&= - \frac{1}{m} [ \sum_{i=1}^m y^{(i)} \log h_{\theta}(\boldsymbol{x}^{(i)}) + (1-y^{(i)}) \log (1 - h_{\theta}(\boldsymbol{x}^{(i)})) ]
}
\tag{13}
$</p>

이러한 식이 어떻게 도출되었는지 당장은 설명하지 않지만, 이 식은 후에 다룰 내용인 **최대 가능도 추정법(Maximu Likelihood Estimation)(Will be posted later)**의 원리를 이용한 통계로부터 유도되었습니다. 그리고 이러한 식을 **Cross Entropy(교차 엔트로피)(Will be Posted Later)**라고 합니다. 지금은 용어만 알아두시면 되겠습니다. 이렇게 표현한 비용함수 $J(\boldsymbol{\theta})$ 는 Convex한 특성을 가지고 있습니다.

이제 매개변수 탐색을 위해 비용함수 $J(\boldsymbol{\theta})$ 를 경사 하강법을 통하여 줄여나가 보겠습니다.

Repeat {<br>
$\quad \theta_{j} := \theta_{j} - \alpha \frac{\partial}{\partial \theta_{j}} J(\boldsymbol{\theta}) \quad (\frac{\partial}{\partial \theta_{j}} J(\boldsymbol{\theta}) = \frac{1}{m} \sum_{i=1}^m (h_{\theta}(\boldsymbol{x}^{(i)})-y^{(i)})x_{j}^{(i)}) \qquad \qquad \qquad \quad (14)$<br>
} (simultaneously update for every $j = 0, \ldots, n$)<br>

비용함수 $J(\boldsymbol{\theta})$ 의 형태가 바뀌었어도, $\theta_{j}$에 대한 편미분 값은 선형 회귀에서 사용했던, MSE를 비용함수로 설정했을 때와 동일합니다. 이에 대한 증명은 부록으로 첨부하겠습니다. 

즉, 매개변수 조정을 위해 로지스틱 회귀에서 사용하는 경사 하강법은 선형 회귀에서 사용하는 경사 하강법과 큰 차이가 없습니다. 차이점이 있다면, 가설 표현 $h_{\theta}(\boldsymbol{x})$ 가 다르지요. (선형 회귀: $h_{\theta}(\boldsymbol{x}) = \boldsymbol{\theta}^T \boldsymbol{x}$ , 로지스틱 회귀: $h_{\theta}(\boldsymbol{x}) = \frac{1}{1 + e^{- \boldsymbol{\theta}^T \boldsymbol{x}}}$ ) 따라서, 선형 회귀에서 사용했던 매개변수 업데이트 방식을 로지스틱 회귀에서도 동일하게 적용해주시면 되겠습니다.

정리하면, 업데이트될 매개변수 $\theta$ 는 식 (15)와 같습니다.
<p>$\theta := \theta - \frac{\alpha}{m} \boldsymbol{X}^T(g(\boldsymbol{X} \boldsymbol{\theta}) - \boldsymbol{\vec{y}}) \tag{15} $</p>

<br>
마지막으로 다중 분류에 대해서 다뤄보겠습니다.

<center><img src="/assets/ml/07_lr_classification/Fig09_multi-class.png" width="700" height="400"></center>
<center>Fig. 9. Multi-Class Classification</center>

<br>
지금까지 다뤄왔던 이진 분류와는 달리, Fig. 9 와 같이 3개 이상의 Class를 분류하고자 하는 경우에 대해서 생각해봅시다. 이 주어진 데이터를 학습 알고리즘에 어떻게 적용할 수 있을까요?

Fig. 9 의 세모를 Class 1, 네모를 Class 2, X를 Class 3라고 둡시다. 우리는 이 훈련 데이터셋을 3가지로 나누어진 이진 분류 문제로 바꿀 수 있습니다. Fig. 9 의 우상단부터 순차적으로 보겠습니다. 1번째 그림은 Class 1을 Positive로, 이 외의 것들은 Negative로 설정합니다. 그리고 이러한 설정을 바탕으로 로지스틱 회귀 분류를 하도록 훈련시킵니다. 그렇다면 그림과 같이 분홍색 직선과 같은 Decision Boundary를 학습하여 Class를 이진 분류 하겠지요. 이와 같은 작업을 Class 2, Class 3에 대해서도 똑같이 적용합니다. 이렇게 학습된 가설 표현을 $h_{\theta}^{(i)}(x)$ 라고 하겠습니다. 즉, 예측한 값이 (i)라는 클래스에 포함될 확률을 의미합니다.

요약하자면, 이러한 작업들은 3가지의 Class를 분류하도록 학습시킨 것입니다. 어떤 1개의 Class를 기준으로 이 Class에 포함되느냐 아니냐만 생각하는 것이죠. 이러한 분류법을 **One-vs-all 분류** 라고 합니다. 최종적으로, 새로운 입력값 $x$ 가 주어진다면, 학습된 3개의 가설함수에 입력값 $x$ 를 넣어 최댓값이 나오는 Class i를 고르면 됩니다. 수식으로 표현하면 하단의 식 (16)과 같습니다.
<p>$\max_{i} h_{\theta}^{(i)}(\boldsymbol{x}) \tag{16}$</p>

#### 이번 포스팅 정리
- 선형 함수만을 사용하여 Class를 분류하는 것은 모델이 오류를 범할 확률이 높으므로, 비선형인 Sigmoid 함수를 이용하여 Class를 분류한다.
- 어떤 데이터를 분류할 때 기준이 되는 선을 Decision Boundary라고 한다.
- 가설 표현에 Sigmoid 함수가 포함되어 있을 때 비용함수로 MSE를 사용하면 비용함수는 Convex하지 않다.
- 따라서, 이진 분류에서 가설 표현에 Sigmoid 함수가 포함되어 있을 때 비용함수로 Cross Entropy를 사용하면 비용함수는 Convex하게 된다.
- 어떤 1개의 Class를 기준으로 이 Class에 포함되느냐 아니냐만 생각함으로써 여러가지 Class를 분류하는 방법을 One-vs-all 분류라고 한다.

---

## Appendix: Partial derivative of J(θ)

<p>$
\begin{equation}
\begin{split}
\sigma(x)'
&=\left(\frac{1}{1+e^{-x}}\right)'=\frac{-(1+e^{-x})'}{(1+e^{-x})^2}=\frac{-1'-(e^{-x})'}{(1+e^{-x})^2}=\frac{0-(-x)'(e^{-x})}{(1+e^{-x})^2}=\frac{-(-1)(e^{-x})}{(1+e^{-x})^2}=\frac{e^{-x}}{(1+e^{-x})^2} \newline &=\left(\frac{1}{1+e^{-x}}\right)\left(\frac{e^{-x}}{1+e^{-x}}\right)=\sigma(x)\left(\frac{+1-1 + e^{-x}}{1+e^{-x}}\right)=\sigma(x)\left(\frac{1 + e^{-x}}{1+e^{-x}} - \frac{1}{1+e^{-x}}\right)=\sigma(x)(1 - \sigma(x))
\end{split}
\end{equation}
$</p>

<br>

<p>$
\begin{equation}
\begin{split}
\frac{\partial}{\partial \theta_j} J(\theta) 
&= \frac{\partial}{\partial \theta_j} \frac{-1}{m}\sum_{i=1}^m \left [ y^{(i)} \log (h_\theta(x^{(i)})) + (1-y^{(i)}) \log (1 - h_\theta(x^{(i)})) \right ] \\
&= - \frac{1}{m}\sum_{i=1}^m \left [     y^{(i)} \frac{\partial}{\partial \theta_j} \log (h_\theta(x^{(i)}))   + (1-y^{(i)}) \frac{\partial}{\partial \theta_j} log (1 - h_\theta(x^{(i)}))\right ] \\
&= - \frac{1}{m}\sum_{i=1}^m \left [     \frac{y^{(i)} \frac{\partial}{\partial \theta_j} h_\theta(x^{(i)})}{h_\theta(x^{(i)})}   + \frac{(1-y^{(i)})\frac{\partial}{\partial \theta_j} (1 - h_\theta(x^{(i)}))}{1 - h_\theta(x^{(i)})}\right ] \\
&= - \frac{1}{m}\sum_{i=1}^m \left [     \frac{y^{(i)} \frac{\partial}{\partial \theta_j} \sigma(\theta^T x^{(i)})}{h_\theta(x^{(i)})}   + \frac{(1-y^{(i)})\frac{\partial}{\partial \theta_j} (1 - \sigma(\theta^T x^{(i)}))}{1 - h_\theta(x^{(i)})}\right ] \\
&= - \frac{1}{m}\sum_{i=1}^m \left [     \frac{y^{(i)} \sigma(\theta^T x^{(i)}) (1 - \sigma(\theta^T x^{(i)})) \frac{\partial}{\partial \theta_j} \theta^T x^{(i)}}{h_\theta(x^{(i)})}   + \frac{- (1-y^{(i)}) \sigma(\theta^T x^{(i)}) (1 - \sigma(\theta^T x^{(i)})) \frac{\partial}{\partial \theta_j} \theta^T x^{(i)}}{1 - h_\theta(x^{(i)})}\right ] \\
&= - \frac{1}{m}\sum_{i=1}^m \left [     \frac{y^{(i)} h_\theta(x^{(i)}) (1 - h_\theta(x^{(i)})) \frac{\partial}{\partial \theta_j} \theta^T x^{(i)}}{h_\theta(x^{(i)})}   - \frac{(1-y^{(i)}) h_\theta(x^{(i)}) (1 - h_\theta(x^{(i)})) \frac{\partial}{\partial \theta_j} \theta^T x^{(i)}}{1 - h_\theta(x^{(i)})}\right ] \\
&= - \frac{1}{m}\sum_{i=1}^m \left [     y^{(i)} (1 - h_\theta(x^{(i)})) x^{(i)}_j - (1-y^{(i)}) h_\theta(x^{(i)}) x^{(i)}_j\right ] \\
&= - \frac{1}{m}\sum_{i=1}^m \left [     y^{(i)} (1 - h_\theta(x^{(i)})) - (1-y^{(i)}) h_\theta(x^{(i)}) \right ] x^{(i)}_j \\
&= - \frac{1}{m}\sum_{i=1}^m \left [     y^{(i)} - y^{(i)} h_\theta(x^{(i)}) - h_\theta(x^{(i)}) + y^{(i)} h_\theta(x^{(i)}) \right ] x^{(i)}_j \\
&= - \frac{1}{m}\sum_{i=1}^m \left [ y^{(i)} - h_\theta(x^{(i)}) \right ] x^{(i)}_j  \\
&= \frac{1}{m}\sum_{i=1}^m \left [ h_\theta(x^{(i)}) - y^{(i)} \right ] x^{(i)}_j
\end{split}
\end{equation}
$</p>

##### 출처:
- [Andrew Ng - Machine Learning](https://www.coursera.org/learn/machine-learning)
- [Wikidocs - Logistic Regression](https://wikidocs.net/4289)
