---
layout: post
title:  모델 표현(Model Representation)과 비용함수(Cost Function)
category: Machine Learning
tags: [Machine Learning, Andrew Ng, Model Representation, 모델 표현, Linear Regression, 선형 회귀, Cost Function, 비용함수 ]
comments: true
---

<br><br>
**이 글을 읽기 전에**<br>
- _해당 포스팅은 Coursera에 올라와 있는 Andrew Ng 교수님의 Machine Learning 강좌를 정리/각색하였습니다._
- _해당 포스팅은 개인적으로 공부한 내용을 정리한 것으로, 일부 내용에 오류가 있을 수 있습니다._
- _포스팅에 오류가 있을 시, 댓글로 남겨주시면 감사하겠습니다._

---

지난 포스팅에서는 비지도학습에 대하여 알아보았습니다. 이번 포스팅에서는 **모델 표현(Model Representation)**과 **비용함수(Cost Function)**에 대하여 알아보도록 하겠습니다.<br>

먼저 모델이란, 어떤 현상을 수학적으로 표현하는 것입니다. 이에 대한 자세한 설명은 [가우시안 혼합 모델 (Gaussian Mixture Model)](https://jiwonkkim.github.io/probability%20and%20statistics/2020/12/26/pas_gmm/) 포스팅의 앞부분에서 확인해주세요.

모델 표현 설명에 앞서, 이전 포스트인 [기계학습(Machine learning) 기초 (1)](https://jiwonkkim.github.io/machine%20learning/2020/12/23/ml_introduction_01/)에서 다루었던 지도학습 중 하나인 집 가격 책정 예제를 다시 가져와보겠습니다.
<center><img src="/assets/ml/03_model_rep_cost_func/Fig01_housing_prices.png" width="800" height="300"></center>
<center>Fig. 1. Housing Prices Prediction</center>

<br>
Fig. 1 에 나와있듯이, 데이터의 분포를 통해 집의 사이즈가 주어지면, 그에 따른 집의 가격을 회귀(Regression)을 통해 예측할 수 있습니다. 이 과정을 도식도로 만들면 하단의 Fig. 2 와 같이 표현할 수 있습니다.

<center><img src="/assets/ml/03_model_rep_cost_func/Fig02_process.png" width="300" height="300"></center>
<center>Fig. 2. Prediction Process</center>

<br>
먼저 주어진 훈련 데이터셋을 입력으로 넣어, 집의 사이즈에 따른 집의 가격을 예측하도록 Learning Algorithm을 통해 학습시킵니다. 그리고 학습된 결과를 집의 사이즈에 따른 집의 가격을 예측할 수 있는 가설(Hypothesis, h)라고 합니다. (Andrew Ng 교수님이 이러한 가설이라는 표현은 애매한 용어라고 하시네요. 크게 담아둘 필요는 없어 보입니다.) 그리고 어떤 집의 크기 $x$가 주어지면 이에 대한 예측된 집의 가격 $y$가 출력됩니다.<br>

그러면 우리는 이 가설 $h$를 어떻게 표현해야 할까요? 위의 예제를 기반으로 하여 $h$를 수식적으로 다음과 같이 표현할 수 있습니다.
<p>$$h_{\theta}(x) = {\theta}_{0} + {\theta}_{1}x \tag{1}$$</p>

이 가설 $h_{\theta}(x)$는 선형함수입니다. 선형이란 무엇인지에 대해서 먼저 설명드리면, 선형성을 가진 함수는 다음과 같습니다.
- 가산성(Additivity), 즉, 임의의 수 $x$ $y$에 대해 $f(x+y) = f(x) + f(y)$가 항상 성립
- 동차성(Homogeneity), 즉, 임의의 수 $x$와 $\alpha$에 대해 $f(\alpha x)=\alpha f(x)$가 항상 성립

만약 $h_{\theta}(x) = \theta_{0} + \theta_{1}x^{2}$와 같이 구성되어 있다면 선형이 아닌 비선형함수가 되겠지요. 물론 가설 $h$를 비선형함수로도 표현할 수 있지만, 선형함수가 더 간단하고 많이 쓰이기 때문에 이 경우엔 선형함수로 사용하였습니다. 이러한 선형함수를 가설로 사용한 회귀모델을 **선형회귀(Linear Regression)** 모델이라고 합니다.<br>

이러한 선형회귀 모델을 만들기 위해서, 즉 모델 표현을 위해서는 가설 식에서 사용된 **매개변수(Parameter)**인 $\theta_{0}$와 $\theta_{1}$을 구해야합니다. Fig. 2 에 나와있는 Learning Algorithm은 이러한 매개변수를 구하는 것과 같습니다. 학습을 통해 적절한 매개변수를 구하여 이를 고정시켜야 집의 사이즈 $x$가 주어졌을 때 이에 맞는 적절한 집의 가격 $y$를 예측할 수 있기 때문이죠. 매개변수의 역할을 좀 더 알기 쉽도록 그래프로 표현하면, 매개변수 $\theta_{0}$와 $\theta_{1}$에 따른 $h_{\theta}(x)$는 Fig. 3 과 같습니다.

<center><img src="/assets/ml/03_model_rep_cost_func/Fig03_parameters.png" width="600" height="300"></center>
<center>Fig. 3. $h_{\theta}(x)$ with parameters $\theta_{0}$ and $\theta_{1}$</center>

<br>
보시는 바와 같이 매개변수의 값에 따라 $y$의 값이 변합니다. 따라서 우리는 이 매개변수를 조절하여, 훈련 데이터셋 $(x,y)$에서 집의 사이즈 $x$가 주어지면 그 $x$를 입력으로 한 가설 $h_{\theta}(x)$가 $x$와 함께 주어진 집의 가격(레이블) $y$에 가까워지도록 해야합니다. 즉, $h_{\theta}(x)-y$를 최소화하는 $\theta_{0}$와 $\theta_{1}$를 찾아야 하는거죠. 그래서 이 오차(Error) $h_{\theta}(x)-y$를 모든 데이터들에 대해 적용하여 합산하고 평균값을 적용하여, 그 값이 최소가 되는 매개변수 $\theta_{0}$와 $\theta_{1}$를 찾으면 됩니다. 하지만 오차 $h_{\theta}(x)-y$는 양수도, 음수도 될 수 있으므로, 이 오차를 제곱해서 계산하는 것이 합리적이고 좋습니다.<br>

**정리하면, 모든 훈련 데이터셋에 대한 오차 $h_{\theta}(x)-y$의 제곱의 합의 평균을 최소화시킨다면, 우리는 매개변수를 잘 학습시킨 것이고, 적절한 모델 표현을 한 것이라 볼 수 있습니다.** 이를 식으로 표현하면 다음과 같습니다.
<p>$$\min_{\theta_{0},\theta_{1}} \frac{1}{2m} \sum_{i=1}^m (h_{\theta}(x^{(i)}) - y^{(i)})^{2} \tag{2}$$</p>
여기서 $m$은 전체 훈련 데이터셋의 개수이고, $(x^{i},y^{i})$는 $i$번째 훈련 데이터셋입니다. 여기서 오차 제곱의 합을 2로 나눈 이유는 계산을 편하게 하기 위함이라고 하네요. (뭔가 이 식을 미분하면 분모에 숫자 없이 깔끔해지니까 일거같기도 하네요. 지극히 개인적인 생각입니다.) 따라서 이 최소화에 필요한 식을 $J(\theta_{0},\theta_{1})$라는 변수로 다시 표현해보겠습니다.
<p>$$J(\theta_{0},\theta_{1}) = \frac{1}{2m} \sum_{i=1}^m (h_{\theta}(x^{(i)}) - y^{(i)})^{2} \tag{3}$$</p>

이 식 (3)이 바로 최적화된 매개변수들을 구하기 위한 **비용함수(Cost Function)**중 하나인 **평균 제곱 오차(Mean Square Error, MSE)**입니다. **즉 비용함수란, 설계된 가설함수가 데이터셋에 잘 맞는지 확인하는데 사용하는 수치입니다.**<br>

이제, 이 비용함수에 대해 직관적으로 이해하기 위해 예제를 들어보겠습니다. 예를 들기 전에, 가설함수 $h_{\theta}(x)$에서 $\theta_{0}$을 0으로 두어 가설함수 $h_{\theta}(x)$를 간략화 하겠습니다. 즉, 이후의 예제에서는 $h_{\theta}(x) = \theta_{1}x$가 됩니다.<br>

먼저, 매개변수 최적화에 사용되는 중요한 두 가지 함수인 가설함수 $h_{\theta}(x)$와 비용함수 $J(\theta_{0},\theta_{1})$를 비교해 보겠습니다. 훈련 데이터셋이 $(1,1)$, $(2,2)$, $(3,3)$으로 구성되어있다고 하면, 어떤 매개변수 $\theta_{1}$에 따른 가설함수 $h_{\theta}(x)$와 비용함수 $J(\theta_{1})$는 다음과 같습니다.<br>

- $\theta_{1}=1 \qquad \Rightarrow \quad h_{\theta}(x) = x \qquad J(\theta_{1}) = 0$
- $\theta_{1}=0.5 \quad \Rightarrow \quad h_{\theta}(x) = 0.5x \quad J(\theta_{1}) = 0.58$
- $\theta_{1}=0 \qquad \Rightarrow \quad h_{\theta}(x) = 0 \qquad J(\theta_{1}) = 2.3$

중요한 부분은, 가설함수 $h_{\theta}(x)$는 고정된 매개변수 $\theta_{1}$을 가지고 있으며, $x$에 대한 함수이고, 비용함수 $J(\theta_{1})$는 $\theta_{1}$에 대한 함수라는 것입니다. 그리고, 우리가 원하는 것은 비용함수 $J(\theta_{1})$를 최소화 하는 것이지요. 비용함수 $J(\theta_{1})$에 대한 그래프는 Fig. 4 와 같습니다.

<center><img src="/assets/ml/03_model_rep_cost_func/Fig04_cost_func.png" width="300" height="300"></center>
<center>Fig. 4. Cost Function $J(\theta_{1})$</center>

<br>
이 그래프를 통해, 우리는 $\theta_{1} = 1$일 때 비용함수가 최소화되는 것을 확인할 수 있습니다. 즉, $\theta_{1} = 1$일 때 가설함수 $h_{\theta}(x)$가 예측한 값이 훈련 데이터셋의 레이블 $y$와 가장 유사했다는 것이죠.<br>

이제 다시 매개변수를 2개를 사용하는 예제를 들어보겠습니다. 매개변수가 1개인 비용함수는 2차원 그래프로 그려지니, 매개변수가 2개인 비용함수는 당연히 3차원 그래프로 그려지게 됩니다. 매개변수가 2개인 비용함수의 그래프 중 하나의 예는 Fig. 5 와 같습니다.<br>

<center><img src="/assets/ml/03_model_rep_cost_func/Fig05_cost_func_3D.png" width="300" height="300"></center>
<center>Fig. 5. Cost Function $J(\theta_{0},\theta_{1})$</center>

<br>
매개변수가 1개일 때와 목표는 다르지 않습니다. 비용함수가 최소화 되는 매개변수를 찾는 것입니다. 그래프로 봤을 때, 그래프의 중앙 부분이 비용함수가 최소가 되겠네요.<br>
직관적으로 이해하기에는 Fig. 5 처럼 3차원으로 표현하는게 좋겠지만, 이를 좀 더 간단하게 표현하기 위해 3차원 그래프를 2차원 평면으로 정사영시킨 등고선 그래프(Contour Plots, 혹은 Contour Figures)를 사용하겠습니다.<br>

<center><img src="/assets/ml/03_model_rep_cost_func/Fig06_contour_not_fitted.png" width="600" height="300"></center>
<center>Fig. 6. $h_{\theta}(x)$ and $J(\theta_{0},\theta_{1})$ with Contour (Not Fitted)</center>

<br>
등고선 그래프는 같은 선에 있는 값들은 서로 같은 값들이며, 바깥쪽 타원일수록 비용함수가 크고 안쪽 타원일수록 비용함수가 작습니다.<br>
Fig. 6 는 적절하지 못한 매개변수($\theta_{0}=800$,$\theta_{1}=-0.15$)를 찾았을 때의 가설함수와 비용함수입니다. 가설함수의 그래프를 보아도 설정한 매개변수들을 통해 만든 가설함수가 훈련 데이터셋과 전혀 맞지 않음을 볼 수 있습니다. 따라서, 비용함수도 매우 큽니다.

<center><img src="/assets/ml/03_model_rep_cost_func/Fig07_contour_fitted.png" width="600" height="300"></center>
<center>Fig. 7. $h_{\theta}(x)$ and $J(\theta_{0},\theta_{1})$ with Contour (Fitted)</center>

<br>
Fig. 7 은 적절한 매개변수($\theta_{0}=250$,$\theta_{1}=0.12$)를 찾았을 때의 가설함수와 비용함수입니다. 가설함수의 그래프를 통해 설정한 매개변수들을 통해 만든 가설함수가 훈련 데이터셋과 상당히 유사함을 볼 수 있습니다. 따라서, 비용함수는 매우 작습니다. (예제에서 주어진 매개변수를 통한 비용함수가 최솟값은 아닙니다.)<br>

물론 실제 사용되는 모델에서 매개변수가 2개밖에 될 리가 없습니다. 수 천, 수 만 이상의 매개변수들이 모델 안에 존재하며, 이 많은 양의 매개변수를 통해 만든 비용함수를 시각화하는것도 어려워집니다.<br>

**결론적으로, 우리는 매개변수의 개수가 많아도, 이에 따른 비용함수가 최소화되는 점은 분명 존재하며, 그 부분을 찾아내어 주어진 데이터에 적합한 모델을 모델링하는 것이 우리의 목표입니다.**

그렇다면, 우리는 비용함수가 최소화되는 매개변수를 어떻게 찾을 수 있을까요? 그 방법 중 하나가 바로 그 유명한 **경사 하강법(Gradient Descent)**입니다.

다음 포스팅에서는 이 경사 하강법에 대해서 이야기해보도록 하겠습니다.

#### 이번 포스팅 정리
- 기계학습 모델을 모델링할 때, 적절한 가설함수를 설계하여 입력에 따른 가설함수의 출력이 우리가 원하는 값이 되도록 해야 한다.
- 비용함수란, 설계된 가설함수가 데이터셋에 잘 맞는지 확인하는데 사용하는 수치이며, 대표적인 예로는 MSE가 있다.
- 비용함수가 최소화되는 점은 분명 존재하며, 그 부분을 찾아내어 주어진 데이터에 적합한 모델을 모델링하는 것이 우리의 목표이다.

##### 출처:
- [Andrew Ng - Machine Learning](https://www.coursera.org/learn/machine-learning)
- [Wikipedia - 선형성](https://ko.wikipedia.org/wiki/%EC%84%A0%ED%98%95%EC%84%B1)
