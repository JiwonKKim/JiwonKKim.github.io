---
layout: post
title:  "선형대수(Linear Algebra) 기초 (1)"
category: Linear Algebra
permalink: /study/math/la/:year/:month/:day/:title/
tags: [Linear Algebra, Machine Learning, Andrew Ng, Supervised learning, Unsupervised Learning, 머신러닝, 기계학습, 지도학습, 비지도학습]
comments: true
---

<br><br>
**이 글을 읽기 전에**<br>
- _해당 포스팅은 Gilbert Strang 교수의 Linear Algebra and Learning from Data-Wellesley-Cambridge Press (2019) 를 정리/각색하였습니다._
- _해당 포스팅은 개인적으로 공부한 내용을 정리한 것으로, 일부 내용에 오류가 있을 수 있습니다._
- _포스팅에 오류가 있을 시, 댓글로 남겨주시면 감사하겠습니다._

---

선형대수는 기계학습을 포함한 다양한 분야에서 사용되고 있으나, 구체적으로 어떻게 사용되는지 잘 모르시는 분들이 많을거라고 생각됩니다. 저 또한 학부생때 선형대수 수업을 들었으나, 그냥 단순히 고윳값을 계산한다던지 등의 계산 위주의 공부를 했었습니다. 앞으로의 포스팅에선 선형대수가 어떤 의미를 가지고, 어떻게 사용되는지에 중점을 맞추고자 합니다.<br>

이번 포스팅에선 선형대수의 여러가지 기초 지식들을 다루므로, 글이 다소 중구난방할 수 있습니다.

간단한 행렬의 곱셈부터 짚고 넘어가겠습니다.<br>
$A$라는 행렬과 $\boldsymbol{x}$라는 벡터를 곱해보겠습니다.<br>
<p>$$
A\boldsymbol{x}
=
\begin{bmatrix}
2 & 3 \\
2 & 4 \\
3 & 7
\end{bmatrix}
\begin{bmatrix}
x_{1} \\
x_{2}
\end{bmatrix}
=
\begin{bmatrix}
2x_{1}+3x_{2} \\
2x_{1}+4x_{2} \\
3x_{1}+7x_{2}
\end{bmatrix}
\tag{1}$$</p>

<p>$$
A\boldsymbol{x}
=
\begin{bmatrix}
2 & 3 \\
2 & 4 \\
3 & 7
\end{bmatrix}
\begin{bmatrix}
x_{1} \\
x_{2}
\end{bmatrix}
=
x_{1}
\begin{bmatrix}
2 \\ 2 \\ 3
\end{bmatrix}
+
x_{2}
\begin{bmatrix}
3 \\ 4 \\ 7
\end{bmatrix}
\tag{1}$$</p>

<br>
단순히 계산적으로만 본다면, $A\boldsymbol{x}$는 (1)과 (2)로 표현할 수 있다는 것입니다. 하지만 $A$와 $\boldsymbol{x}$의 곱을 선형대수적으로 보면 $A\boldsymbol{x}$는 $\boldsymbol{a_{1}}$과 $\boldsymbol{a_{2}}$의 **선형 결합(Linear Combination)**인 것입니다. 이것이 선형대수의 기본적인 연산입니다. 즉, $A\boldsymbol{x}$는 $A$의 열 벡터들의 선형 조합입니다.<br>

이어서 $A$의 **열 공간(Column Space)**에 대해서 다뤄보겠습니다. 어떤 행렬의 열 공간이란, 그 행렬의 선형 조합으로 만들 수 있는 모든 집합을 의미합니다. 당연히 **행 공간(Row Space)**도 존재합니다. $A\boldsymbol{x}$는 (1)을 통해 알 수 있듯이, 3차원 공간입니다. 이는 $\mathbf{R}^{3}$로 표현합니다. ($\mathbf{R}$은 실수를 나타냅니다.) 그럼 $A\boldsymbol{x}=x_{1}\boldsymbol{a_{1}}+x_{2}\boldsymbol{a_{2}}$의 모든 조합은 3차원 공간에서 어떤 부분을 만들어 낼까요? 정답은 평면입니다. 구체적으로 말하자면, $\boldsymbol{a_{1}}$로 표현할 수 있는 선과 $\boldsymbol{a_{2}}$로 표현할 수 있는 선을 포함한 평면입니다. 따라서 $A$의 열 공간은 평면입니다.<br>

행렬의 계수(Rank)란, 어떤 행렬의 독립적인 열의 갯수입니다. 

##### 출처:
