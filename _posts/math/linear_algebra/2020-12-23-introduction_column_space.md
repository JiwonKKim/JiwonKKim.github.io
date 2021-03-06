---
layout: post
title:  "선형대수 기초와 열 공간(Column Space)"
category: Linear Algebra
tags: [Linear Algebra, 선형대수, Gilbert Strang, Column Space, 열 공간]
comments: true
---

<br><br>
**이 글을 읽기 전에**<br>
- _해당 포스팅은 Gilbert Strang 교수님의 Linear Algebra and Learning from Data-Wellesley-Cambridge Press (2019), [Learn Again! 러너게인](https://twlab.tistory.com/)님 블로그, 그리고 [Gilbert Strang 교수님의 MIT OPEN COURSE 강의](https://ocw.mit.edu/courses/mathematics/18-065-matrix-methods-in-data-analysis-signal-processing-and-machine-learning-spring-2018/)를 정리/각색하였습니다._
- _해당 포스팅은 개인적으로 공부한 내용을 정리한 것으로, 일부 내용에 오류가 있을 수 있습니다._
- _포스팅에 오류가 있을 시, 댓글로 남겨주시면 감사하겠습니다._

---

선형대수는 기계학습을 포함한 다양한 분야에서 사용되고 있으나, 구체적으로 어떻게 사용되는지 잘 모르시는 분들이 많을거라고 생각됩니다. 저 또한 학부생때 선형대수 수업을 들었으나, 그냥 단순히 고윳값을 계산한다던지 등의 계산 위주의 공부를 했었습니다. 앞으로의 포스팅에선 선형대수가 어떤 의미를 가지고, 어떻게 사용되는지에 중점을 맞추고자 합니다.<br>

먼저 선형대수라는 용어에서 선형이란 무엇일까요?

[위키피디아](https://ko.wikipedia.org/wiki/%EC%84%A0%ED%98%95%EC%84%B1)에 의하면, 선형성을 가진 함수의 정의는 다음과 같습니다.
- 가산성(Additivity), 즉, 임의의 수 $x$ $y$에 대해 $f(x+y) = f(x) + f(y)$가 항상 성립
- 동차성(Homogeneity), 즉, 임의의 수 $x$와 $\alpha$에 대해 $f(\alpha x)=\alpha f(x)$가 항상 성립

예를 들어서, $f(x)=3x$이라는 함수가 있다고 가정해봅시다.<br>
$f(x)=3x$에서 $f(x+y)=3(x+y)$이고, $f(x) + f(y) = 3x + 3y$이기 때문에 가산성 $f(x+y) = f(x)+f(y)$를 만족합니다.<br>
또, $f(\alpha x)=3(\alpha x)$이고, $\alpha f(x) = \alpha(3x)$이기 때문에 동차성 $f(\alpha x)=\alpha f(x)$ 또한 만족하므로, 함수 입니다.<br>
비슷하게, $f(x,y)=2x+3y$와 같은 함수도 선형입니다.<br>

반대로, $f(x)=x+3$, $f(x)=x^2$와 같은 함수들은 비선형 함수입니다. 직접 식을 대입해보시면 알 수 있습니다.<br>

즉, 선형 함수는 어떠한 함수가 진행하는 모양이 '직선'이라는 의미로 사용됩니다. 물론, $f(x)=x+3$와 같은 예외도 있습니다. $f(x)=x+3$에서 $3$이 $x$에 의해 통제되지 않으므로 생기는 현상으로 보이네요. 즉, 선형함수는 원점을 지나는 직선이어야 합니다.<br>

이어서 선형대수라는 용어에서 대수란 무엇일까요?<br>

대수(代數)라는 말은 수를 대신한다는 뜻으로, 수 대신 문자를 쓴다는 점에 착안한 번역어입니다. 그러니까, 숫자를 대신하는 대수(代數), 즉 문자를 사용한다는 의미입니다.<br>

**결론적으로 선형대수는, 선형성(Linearity)을 가지는 대수(Algebra)로 이루어진 방정식들의 해를 구하는 방법론, 혹은 학문입니다.** 이를 통해 $5x-y=0$과 같은 식을 풀 수 있게 되는 것이죠.

선형대수에 대한 정의는 여기까지 하고, 간단한 행렬의 곱셈부터 짚고 넘어가겠습니다.<br>
$A$라는 행렬과 $\boldsymbol{x}$라는 벡터를 곱해보겠습니다.<br>
<p>$$
A\boldsymbol{x}
=
\begin{bmatrix}
2 & 1 & 3 \\
3 & 1 & 4 \\
5 & 7 & 12
\end{bmatrix}
\begin{bmatrix}
x_{1} \\
x_{2} \\
x_{3}
\end{bmatrix}
=
\begin{bmatrix}
2x_{1}+ x_{2} + 3x_{3} \\
3x_{1}+ x_{2} + 4x_{3}\\
5x_{1}+7x_{2} +12x_{3}
\end{bmatrix}
\tag{1}$$</p>

<p>$$
A\boldsymbol{x}
=
\begin{bmatrix}
2 & 1 & 3 \\
3 & 1 & 4 \\
5 & 7 & 12
\end{bmatrix}
\begin{bmatrix}
x_{1} \\
x_{2} \\
x_{3}
\end{bmatrix}
=
x_{1}
\begin{bmatrix}
2 \\ 3 \\ 5
\end{bmatrix}
+
x_{2}
\begin{bmatrix}
1 \\ 1 \\ 7
\end{bmatrix}
+
x_{3}
\begin{bmatrix}
3 \\ 4 \\ 12
\end{bmatrix}
\tag{2}$$</p>

<br>
단순히 계산적으로만 본다면, $A\boldsymbol{x}$는 (1)과 (2)로 표현할 수 있다는 것입니다. 하지만 $A$와 $\boldsymbol{x}$의 곱을 선형대수적으로 보면 $A\boldsymbol{x}$는 $\boldsymbol{a_{1}}$, $\boldsymbol{a_{2}}$,$\boldsymbol{a_{3}}$의 **선형 결합(Linear Combination)**인 것입니다. 이것이 선형대수의 기본적인 연산입니다. 즉, $A\boldsymbol{x}$는 $A$의 열 벡터들의 선형 결합입니다. 강의 중에서 교수님이 행렬을 각 요소별로만 생각하려하지 말고, 행렬 그 자체를 보라고 하셨는데, 이런 측면에서 말씀하시는거 같네요. (여전히 감은 오지 않습니다.)<br>

이어서 $A$의 **열 공간(Column Space)**에 대해서 다뤄보겠습니다. 어떤 행렬의 열 공간이란, 그 행렬의 열 벡터들의 선형 결합으로 만들 수 있는 모든 집합을 의미합니다. **즉, $A$의 열 공간이란, 모든 벡터 $\boldsymbol{x}$에 대하여 $A\boldsymbol{x}$가 만들어낼 수 있는 공간을 의미합니다.** 기호로는 $C(A)$로 표현합니다.<br>

$\boldsymbol{a_{n}}$을 행렬 $A$의 n번째 열 벡터라고 합시다. 그럼 $A\boldsymbol{x}=x_{1}\boldsymbol{a_{1}}+x_{2}\boldsymbol{a_{2}}+x_{3}\boldsymbol{a_{3}}$의 모든 조합, 즉 $A$의 열 공간은 3차원 공간에서 어떤 부분을 만들어 낼까요? 정답은 평면입니다. 벡터 $\boldsymbol{x}$가 3차원인데, 왜 2차원인 평면을 만들어낼까요? 그 이유는 $\boldsymbol{a_{3}}$가 $\boldsymbol{a_{1}}$과 $\boldsymbol{a_{2}}$의 합으로 나타낼 수 있기 때문입니다. 따라서 $A$의 열 공간은 평면입니다. 행렬 $A$에 따라 $C(A)$는 3차원 공간도 되고, 1차원 직선도 되고, 점도 될 수 있습니다. $\boldsymbol{a_{3}}$는 $\boldsymbol{a_{1}}$와 $\boldsymbol{a_{2}}$의 합으로 구성될 수 있기 때문에 **선형종속(Linearly Dependent)**이며, $\boldsymbol{a_{1}}$과 $\boldsymbol{a_{2}}$는 행렬 $A$의 다른 열 벡터들의 조합으로 구성할 수 없으므로 **선형독립(Linearly Independent)**입니다. 그리고 **기저(Basis)**란, 그 벡터공간을 선형생성하는 선형독립인 벡터들을 의미하는데, 따라서 행렬 $A$의 기저는 $\boldsymbol{a_{1}}$과 $\boldsymbol{a_{2}}$가 됩니다. 그리고 행렬의 **계수(Rank)**는 어떤 행렬의 독립적인 열의 개수, 즉 행렬의 기저의 개수가 되므로 $rank(A)=2$가 됩니다. 행렬 $A$의 열 공간은 행렬 $A$의 랭크에 해당하는 차원의 공간을 만들 수 있습니다.<br>

열 공간이 있으니 당연히 **행 공간(Row Space)**도 존재합니다. 열 공간과 마찬가지로 어떤 행렬의 행 공간이란, 그 행렬의 행 벡터들의 선형 결합으로 만들 수 있는 모든 집합을 의미합니다. 열 공간과 행 공간은 분명 다른 공간이지만, 일반적으로 행렬의 열 벡터를 중점적으로 다루기 때문에 행 공간에 대한 표기를 $C(A^T)$라고 합니다. (행렬 $A$의 행 공간은 행렬 $A$의 전치행렬 $A^T$의 열 공간과 같기 때문이고, MATLAB이나 Python 등에서 표현하는 벡터는 보통 열 벡터를 의미합니다.)<br>

예제에서 우리는 $\boldsymbol{a_{3}}$가 $\boldsymbol{a_{1}}$과 $\boldsymbol{a_{2}}$의 합으로 나타내짐을 우연히 알았습니다. 하지만 행 공간의 기저를 알아내는 것은 쉽지 않아보입니다. 따라서 열 공간의 기저를 통해 행 공간의 기저를 구해보도록 하겠습니다.<br>

<p>$$
A\boldsymbol{x}
=
CR
=
\begin{bmatrix}
2 & 1 & 3 \\
3 & 1 & 4 \\
5 & 7 & 12
\end{bmatrix}
=
\begin{bmatrix}
2 & 1 \\
3 & 1 \\
5 & 7 
\end{bmatrix}
\begin{bmatrix}
1 & 0 & 1 \\
0 & 1 & 1
\end{bmatrix}
\tag{3}
$$</p>

여기서 $C$와 $R$은 각각 $A$의 열 공간 기저와 행 공간 기저를 의미합니다. 앞서 구해놓은 $C$를 통해 (3)처럼 행 공간 기저 $R$을 구할 수 있습니다. 그렇다면 이렇게 구한 $R$이 실제로 행 공간 기저가 될 수 있을까요?<br>

$\boldsymbol{b_{n}}$을 행렬 $A$의 n번째 행 벡터, $\boldsymbol{r_{n}}$을 $R$의 행 벡터라고 합시다. $R$로 $A$의 첫번째 행 성분을 표현 하려면 어떻게 해야할까요? $\boldsymbol{b_{1}}=2\boldsymbol{r_{1}}+\boldsymbol{r_{2}}$로 표현할 수 있습니다. 여기서 $\boldsymbol{b_{1}}$의 계수가 된 (2,1)은 어떻게 찾았을까요? 답은 간단합니다. $C$의 첫 번째 행 벡터에서 찾을 수 있었습니다. $A$의 나머지 행 벡터들도 똑같은 방식으로 구할 수 있습니다.<br>

**즉, 우리는 $A=CR$을 두 가지 표현으로 생각할 수 있습니다.**
<ol>
  <li>$A$의 열 벡터들을 만들어내기 위한 $C$의 열 벡터들의 조합</li>
  <li>$A$의 행 벡터들을 만들어내기 위한 $R$의 행 벡터들의 조합</li>
</ol>

이 행렬의 인수분해 $A=CR$은 중요한 아이디어입니다. 위에서 행렬의 랭크는 행렬의 독립적인 열의 개수라고 정의했습니다. 하지만 $A=CR$을 통해서 **행렬 $A$의 독립적인 열 벡터 개수는 행렬 $A$의 독립적인 행 벡터 개수와 동일**함을 알 수 있습니다. 따라서 랭크는 행렬의 독립적인 행의 개수라고도 정의할 수 있습니다. **즉, 행렬의 랭크는 행렬이 나타낼 수 있는 벡터 공간에서 기저의 개수를 의미하고, 이 기저는 서로 독립인 행 또는 열의 벡터의 개수에 의해서 결정됩니다.**<br>

만약에 우리가 $10^5$크기의 행렬을 데이터로 사용한다고 가정해봅시다. 이 데이터를 모두 사용하기엔 부담이 적지 않기 때문에, 이 데이터를 무작위로 추출할 수 있도록 랜덤 샘플링을 합니다. 랜덤 샘플링을 위해 $A$를 ${10^5}\times{10^5}$크기의 데이터 행렬, $\boldsymbol{x}$를 ${10^5}\times{1}$의 랜덤 벡터라고 둡시다. 그러면 $A\boldsymbol{x}$를 데이터 행렬 $A$에서 하나의 데이터를 랜덤 샘플링하는 것으로 볼 수 있습니다. 어디서 많이 보던 수식입니다. 이것은 $A\boldsymbol{x}$의 열 공간입니다. **즉, 데이터 $A$를 무작위 샘플링하면 이것은 $A$의 열 공간이 되므로, $A$의 대략적인 열 공간을 볼 수 있다는 생각입니다.**<br>

그렇다면, $ABC\boldsymbol{x}$는 $A$의 열 공간에 있는걸까요? 정답은 '그렇다' 입니다. $A(BC\boldsymbol{x})$와 같이 $A$에 무언가를 곱한 형태가 되기 때문이죠. 이렇게 괄호를 어디에 두느냐가 선형대수의 핵심입니다.<br>
$A=CR$을 다시 봅시다. $C$는 그대로 $A$의 열 공간이라고 가정하고, $R$을 $A$의 행 벡터 일부라고 두면, 등호가 성립하지 않을 것입니다. 이 등호가 성립하기 위해서는 중간에 어떤 행렬 ${2}\times{2}$크기의 행렬 $U$가 필요할 것입니다. 즉, $A=CUR^{\prime}$이 되는거죠. 이 $CUR^{\prime}$이 매우 중요한 역할을 한다고 합니다.<br>

교수님은 기존의 행렬 곱 대신, 위에서 언급된 $A=CUR^{\prime}$같은 방식으로 새로운 행렬 곱 시스템을 생각하고 계신 것 같습니다. 이에 대한 자세한 내용은 이후의 수업에서 알려주실것 같네요.<br>

#### 이번 포스팅 정리
<ul>
  <li>기저란, 벡터공간을 선형생성하는 선형독립인 벡터들을 의미한다.</li>
  <li>행렬의 곱셉은 행렬의 기저들을 통해 표현할 수 있다.</li>
  <li>열 공간이란, 그 행렬의 열 벡터들의 선형 결합으로 만들 수 있는 모든 집합을 의미한다.</li>
  <li>어떤 행렬에서 열 공간의 기저 개수와 행 공간의 기저 개수는 같다.</li>
  <li>랭크란, 행렬이 나타낼 수 있는 벡터 공간에서 기저의 개수를 의미한다.</li>
  <li>새로운 행렬 곱의 패러다임..?</li>
</ul>

##### 출처:
- [Gilbert Strang 교수님의 MIT OPEN COURSE 강의](https://ocw.mit.edu/courses/mathematics/18-065-matrix-methods-in-data-analysis-signal-processing-and-machine-learning-spring-2018/)
- [Learn Agian! 러너게인 (Linear Algebra)](https://twlab.tistory.com/)
- [Wikipedia - 선형성](https://ko.wikipedia.org/wiki/%EC%84%A0%ED%98%95%EC%84%B1)
- [Wikipedia - 대수학](https://ko.wikipedia.org/wiki/%EB%8C%80%EC%88%98%ED%95%99)