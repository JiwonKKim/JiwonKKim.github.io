---
layout: post
title:  정규방정식(Normal Equation)
category: Machine Learning
tags: [Machine Learning, Andrew Ng, Normal Equation, 정규방정식]
comments: true
---

<br><br>
**이 글을 읽기 전에**<br>
- _해당 포스팅은 Coursera에 올라와 있는 Andrew Ng 교수님의 Machine Learning 강좌를 정리/각색하였습니다._
- _해당 포스팅은 개인적으로 공부한 내용을 정리한 것으로, 일부 내용에 오류가 있을 수 있습니다._
- _포스팅에 오류가 있을 시, 댓글로 남겨주시면 감사하겠습니다._

---

특정 선형 회귀 문제에서 최적화된 매개변수 $\boldsymbol{\theta}$ 를 구하는 방법은 **[경사 하강법(Gradient Descent)](https://jiwonkkim.github.io/machine%20learning/2021/01/09/ml_gradient_descent/)**만 존재하는 것이 아닙니다. 이번 포스팅에서는 최적화된 매개변수 $\boldsymbol{\theta}$ 를 구하는 방법 중 하나인 **정규방정식(Normal Equation)**에 대해서 알아보겠습니다.

경사 하강법은 매개변수 $\boldsymbol{\theta}$ 를 **반복적으로** 바꾸어 주면서 비용함수(Cost Function)가 최소가 되는 매개변수를 탐색했습니다. 정규방정식은 경사 하강과는 달리 이렇게 반복적인 계산 없이 비용함수(Cost Function)가 최소가 되는 매개변수를 한 번에 구합니다.

<center><img src="/assets/ml/06_normal_eq/Fig01_room_to_mat.png" width="600" height="200"></center>
<center>Table 1. Room Information</center>

<br>
늘상의 예제처럼 집값 구하기 예제를 사용하겠습니다. 집의 정보가 Table 1 과 같이 구성되어있는 데이터에 Feature $x_{0}$ 를 추가하겠습니다. 이 데이터를 행렬로 표현한 행렬 $\boldsymbol{X}$ 와 $\boldsymbol{y}$ 를 만들어 보겠습니다.

<p>$$
\boldsymbol{X}
=
\begin{bmatrix}
1 & 2104 & 5 & 1 & 45 \\
1 & 1416 & 3 & 2 & 40 \\
1 & 1534 & 3 & 2 & 30 \\
1 & 852  & 2 & 1 & 36
\end{bmatrix}
,
\quad
\boldsymbol{y}
=
\begin{bmatrix}
460 \\
232 \\
315 \\
178
\end{bmatrix}
\tag{1}$$</p>

식 (1)에서 행렬 $\boldsymbol{X}$ 는 훈련 데이터의 모든 Feature를 가지고 있고, 행렬 $\boldsymbol{y}$ 는 훈련 데이터의 라벨(Label)을 가지고 있습니다.

결론부터 말씀드리면, 정규방정식은 하단의 식 (2)를 계산함으로써 비용함수를 최소화하는 매개변수 $\boldsymbol{\theta}$ 를 구할 수 있습니다.
<p>$\boldsymbol{\theta}=(\boldsymbol{X}^{T} \boldsymbol{X})^{-1} \boldsymbol{X}^{T} \boldsymbol{y} \tag{2}$</p>

좀 더 나아가, 더 일반화된 식으로 정규방정식을 다뤄보겠습니다.

우리가 $m$ 개의 훈련 데이터셋을 가지고 있고, 데이터셋은 각각 $n$ 개의 Feature를 가지고 있다고 합시다. 이 때 훈련 데이터셋은 하단의 식 (3)과 (4)로 나타낼 수 있습니다.

<p>$$
\boldsymbol{x}^{(i)}
=
\begin{bmatrix}
x_{0}^{(i)} \\
x_{1}^{(i)} \\
x_{2}^{(i)} \\
\vdots \\
x_{n}^{(i)} \\
\end{bmatrix}
\in
\mathbb{R}^{n+1}
\tag{3}$$</p>

<p>$$
\boldsymbol{X}
=
\begin{bmatrix}
(\boldsymbol{x}^{(1)})^{T} \\
(\boldsymbol{x}^{(2)})^{T} \\
(\boldsymbol{x}^{(3)})^{T} \\
\vdots \\
(\boldsymbol{x}^{(m)})^{T} \\
\end{bmatrix}
,
\quad
\boldsymbol{y}
=
\begin{bmatrix}
y^{(1)} \\
y^{(2)} \\
y^{(3)} \\
\vdots \\
y^{(m)}
\end{bmatrix}
\tag{4}$$</p>

여기서 식 (4)에 등장하는 $\boldsymbol{X}$ 를 Design Matrix라고 합니다. 그리고 $\boldsymbol{X}$ 는 $m \times (n+1)$ 차원이 되죠. 식 (3)과 (4)를 정규방정식인 식 (2)에 대입하면 매개변수를 구할 수 있지요. 또, 비용함수 최소화를 위해 정규방정식을 사용한다면 이전에 다뤘던 [Feature Scaling](https://jiwonkkim.github.io/machine%20learning/2021/01/12/ml_multi_lr_pr/)은 필요하지 않습니다.

우리는 비용함수 최소화를 위해 경사 하강법과 정규방정식 이렇게 2개를 배웠는데, 그러면 이 2개의 특징을 나열하면서 어떨 때 경사 하강법을 사용하고, 어떨 때 정규방정식을 사용하는지 파악해 보겠습니다.

|경사 하강법|정규방정식|
|:--|:--|
|Learning Rate $\alpha$를 정해야 한다|Learning Rate $\alpha$를 정하지 않아도 된다|
|반복적인 단계가 필요하다|반복적인 단계가 필요없다|
|Feature의 수가 많을 때도 잘 동작한다|$(\boldsymbol{X}^{T} \boldsymbol{X})^{-1}$를 계산해야 하므로, Feature의 수가 많으면 계산이 느려진다.|

즉, Feature의 개수에 따라서 경사 하강법을 사용할지, 정규방정식을 사용할지의 여부가 갈립니다. 만약 Feature의 개수 $n$ 이 100, 1000개이면 정규방정식을 사용해도 좋습니다. 하지만, $n$이 10000개가 넘어가기 시작하면 컴퓨터도 행렬계산이 느려지기 시작합니다.

뜬금없이 식 하나만 주어주고 이것이 정규방정식이다 라고 이야기 하는데.. 교수님이 일단 이렇게만 설명하시네요... 사실 비용함수 최소화는 경사 하강법이 보편적으로 많이 쓰이고 정규방정식은 자주 쓰이지 않기 때문이 일단은 그러려니 하고 넘어가겠습니다. 추후에 기회가 되면 추가적으로 설명을 써놓겠습니다.

#### 이번 포스팅 정리
- 특정 선형 회귀 문제에서 최적화된 매개변수 $\boldsymbol{\theta}$ 를 구하는 방법은 경사 하강법 이외에도 정규방정식이 있다.
- 정규방정식은 경사 하강법과 달리 반복적인 단계가 없다.
- Feature의 수가 많으면 계산이 오래걸린다.

##### 출처:
- [Andrew Ng - Machine Learning](https://www.coursera.org/learn/machine-learning)
