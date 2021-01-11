---
layout: post
title:  "행렬의 소거(Elimination)"
category: Linear Algebra
tags: [Linear Algebra, 선형대수, Gilbert Strang, Gaussian Elimination, 가우시안 소거법]
comments: true
---

<br><br>
**이 글을 읽기 전에**<br>
- _해당 포스팅은 Gilbert Strang 교수님의 Linear Algebra and Learning from Data-Wellesley-Cambridge Press (2019), [Learn Again! 러너게인](https://twlab.tistory.com/)님 블로그, 그리고 [Gilbert Strang 교수님의 MIT OPEN COURSE 강의](https://ocw.mit.edu/courses/mathematics/18-065-matrix-methods-in-data-analysis-signal-processing-and-machine-learning-spring-2018/)를 정리/각색하였습니다._
- _해당 포스팅은 개인적으로 공부한 내용을 정리한 것으로, 일부 내용에 오류가 있을 수 있습니다._
- _포스팅에 오류가 있을 시, 댓글로 남겨주시면 감사하겠습니다._

---

Gilbert Strang 교수님의 수업 2강에서 미리 알아두어야 할 행렬의 인수분해 식 5가지가 있습니다. 그 식들의 목록은 다음과 같습니다.

- $A = LU$
- $A = QR$
- $S = Q \Lambda Q$
- $A = X \Lambda X^-1$
- $A = U \Sigma V^T$

이번 포스팅에서는 필요한 내용 중 하나인 $A=LU$, 즉 **LU 분해(LU Decomposition)**에서 사용되는 **소거(Elimination)**에 대해서 이야기해 보겠습니다.

이번 포스팅에서 다룰 내용은 문자를 이용한 증명보다는 숫자로 예제를 들어 설명하는 것이 좀 더 직관적이기 때문에, 예제를 이용하여 설명하도록 하겠습니다.

어떤 행렬방정식 $A\boldsymbol{x}=\boldsymbol{b}$의 해를 구해야 한다고 합시다. (요즘 교육과정에서는 행렬에 대해서 배우지 않는다고 하지만) 고등교육과정에서 배운대로 생각하면 $x$를 구하기 위해서는 양변에 $A$의 역행렬을 곱해주어 $x=A^{-1}b$를 계산하면 됩니다. 하지만, 컴퓨터는 곱셉을 계산하는 시간이 덧셈을 계산하는 시간보다 훨씬 많이 소요됩니다. 따라서 행렬의 곱셉을 지원하는 프로그램들은 곱연산을 최소화하고 덧셈을 주로 사용합니다. 따라서, 컴퓨터가 $Ax=B$의 해를 구할 때 $A$의 역행렬을 구하고, 그것을 $B$와 곱해주는 방식을 사용하지 않습니다. 대신, 행렬을 **소거**하여 다르게 표현하지요.

그 소거법 중 하나가 **가우시안 소거법(Gaussian Elimination)**입니다.

가우시안 소거법은 연립일차방정식을 풀이하는 알고리즘 중 하나로, 행렬 $A$를 **Upper Triangular Matrix(상삼각행렬)**로 표현합니다. 여기서 Upper Triangular Matrix란, 주대각선을 기준으로 대각항의 아래쪽 항들의 값이 모두 0인 행렬입니다. 줄여서 $U$라고 표현합니다. Upper Triangular Matrix가 있으면 당연히 **Lower Triangular Matrix(하삼각행렬)**도 있겠죠? Lower Triangular Matrix는 주대각선을 기준으로 대각항의 위쪽 항들의 값이 모두 0인 행렬입니다.

<center><img src="/assets/la/02_matrix_elimination/Fig01_LU.png" width="500" height="150"></center>
<center>Fig. 1. Lower and Upper Triangular Matrix</center>

<br>
가우시안 소거법은 기준이 되는 식에 **0이 아닌** 적당한 상수을 곱하여, 제거하고자 하는 항이 있는 식에 기준식을 빼줍니다.

말만 들으면 무슨 이야긴지 도통 이해가 안갑니다. 빠르게 예제로 넘어갑시다.

행렬 $A$가 다음과 같이 구성되어 있다고 합시다.

<p>$$
A
=
\begin{bmatrix}
2 & 4 & -2 \\
4 & 9 & -3 \\
-2 & -3 & 7
\end{bmatrix}
\Rightarrow
\begin{bmatrix}
u_{1,1} & u_{1,2} & u_{1,3} \\
0 & u_{2,2} & u_{2,3} \\
0 & 0 & u_{3,3}
\end{bmatrix} \\
\tag{1}
$$</p>

먼저 $a_{i,j}$를 행렬 $A$의 i행 j열의 성분이라고 합시다. $a_{1,1}$은 2, $a_{3,2}$는 -3이 되는거죠. 우리는 이 행렬 $A$를 Upper Triangular Matrix 형태를 만들어야 합니다. 성분으로 보면 $a_{2,1}$, $a_{3,1}$, $a_{3,2}$ 성분이 0이 되도록 바꾸어야 합니다. 행렬 $A$의 각각의 행을 일차연립방정식에서 사용되는 하나의 식이라고 생각해 봅시다. $a_{2,1}$와 $a_{3,1}$를 0으로 만들기 위해선 1번째 행에 적절한 상수를 곱한 식을 나머지 2번째, 3번째 행을 빼주어야 합니다. 즉, $a_{1,1}$을 $a_{2,1}$, $a_{3,1}$에 맞춰지도록 2번째 행 - 1번째 행 * 2, 3번째 행 - 1번째 행 * (-1)을 연산하면 $a_{2,1}$와 $a_{3,1}$가 0이 되겠죠? 직접 계산해 봅시다.

<p>$$
A
=
\begin{bmatrix}
2 & 4 & -2 \\
4 & 9 & -3 \\
-2 & -3 & 7
\end{bmatrix}
\Rightarrow
\begin{bmatrix}
2 & 4 & -2 \\
0 & 1 & 1 \\
0 & 1 & 5
\end{bmatrix}
=
\begin{bmatrix}
a_{1,1}^\prime & a_{1,2}^\prime & a_{1,3}^\prime \\
0 & a_{2,2}^\prime & a_{2,3}^\prime \\
0 & a_{3,2}^\prime & a_{3,3}^\prime
\end{bmatrix} \\
\tag{2}
$$</p>

자, 어떻게 Upper Triangular Matrix 형태를 만드는 것인지 감이 오시나요? 일반적으로 우리가 알고 있는 일차연립방정식이라고 생각하시면 됩니다. 이 때, 없애고자 하는 식의 중심축이 되는 성분을 **피벗(Pivot)**이라고 합니다. 살려둬야 할 성분이라고 보시면 되겠습니다. 그리고 이 피벗을 포함하는 열을 **Pivot Column**이라고 합니다. 식 (2)에서 사용된 피벗은 $a_{1,1}$이 되겠죠. 피벗의 자세한 정의는, 선형대수학에서 특정 계산을 수행하기 위한 임의의 알고리즘에 의해 먼저 선택된 행렬의 성분을 의미합니다.

이제 1번째 열에 대한 요소들이 $a_{1,1}^\prime$을 제외하고 모두 0이 되었습니다. 이제 $a_{3,2}^\prime$를 0으로 만들기만 하면 행렬 $A$는 Upper Triangular Matrix 형태가 됩니다.

그런데 이 때 다시 1번째 행을 사용하여 3번째 행의 $a_{3,2}^\prime$를 0으로 만드려고 하면 모처럼 0으로 만든 요소 $a_{3,1}^\prime$가 다시 생성되겠지요? 그러니까 이번엔 2번째 행을 사용하여 $a_{3,1}^\prime$를 0으로 만들겠습니다. 따라서 2번째 피벗은 $a_{2,2}^\prime$가 되겠습니다. 이제 3번째 행 - 2번째 행 * 1 을 진행합시다.

<p>$$
A
=
\begin{bmatrix}
2 & 4 & -2 \\
4 & 9 & -3 \\
-2 & -3 & 7
\end{bmatrix}
\Rightarrow
U
=
\begin{bmatrix}
u_{1,1} & u_{1,2} & u_{1,3} \\
0 & u_{2,2} & u_{2,3} \\
0 & 0 & u_{3,3}
\end{bmatrix}
=
\begin{bmatrix}
2 & 4 & -2 \\
0 & 1 & 1 \\
0 & 0 & 4
\end{bmatrix}
\tag{3}
$$</p>

드디어 행렬 $A$가 Upper Triangular Matrix 형태가 되었습니다. 3번째 피벗은 자연스럽게 $u_{3,3}$이 되겠습니다. 즉, 피벗은 $U$의 대각 성분인 $u_{1,1}$, $u_{2,2}$, $u_{3,3}$가 되겠습니다. 이 때 0인 원소는 피벗이 될 수 없음에 유의하시기 바랍니다.

이제 위에서 언급했던, "가우시안 소거법은 기준이 되는 식에 **0이 아닌** 적당한 상수을 곱하여, 제거하고자 하는 항이 있는 식에 기준식을 빼줍니다." 라는 말을 이해하실 수 있을겁니다.

그런데, 가우시안 소거법을 진행하는 도중에 피벗이 0이 되는 경우가 생깁니다. 피벗이 0이 되면 소거법의 진행이 더이상 불가능해집니다. 이럴때는 Row Exchange(행 교환)를 사용한 후에 소거법을 계속 진행하시면 됩니다.

<p>$$
A
=
\begin{bmatrix}
2 & 4 & -2 \\
1 & 2 & 1 \\
-2 & -3 & 7
\end{bmatrix}
\Rightarrow
\begin{bmatrix}
2 & 4 & -2 \\
0 & 0 & 2 \\
0 & 1 & 5
\end{bmatrix}
\Rightarrow
\begin{bmatrix}
2 & 4 & -2 \\
0 & 1 & 5 \\
0 & 0 & 2
\end{bmatrix}
\tag{4}
$$</p>

식 (4)에 보이는 것처럼, 식의 중간 항의 2번째/3번째 행을 바꾸어 소거법을 진행하도록 합니다. 그럼 소거법을 다시 진행할 수 있게 됩니다. 단, 이 때 교환할 행의 Pivot Column이 0이 아닌경우에만 Row Exchange가 가능합니다.

하지만, 아무리 Row Exchange를 적용하거나 다른 행과 연산하여도 피벗이 0이 되는 경우를 피할 수 없는 경우가 생깁니다. 그 예로 하단의 식 (5)를 들 수 있습니다.

<p>$$
A
=
\begin{bmatrix}
1 & 2 & 1 \\
3 & 8 & 1 \\
0 & 4 & -4
\end{bmatrix}
\Rightarrow
\begin{bmatrix}
1 & 2 & 1 \\
0 & 2 & -2 \\
0 & 0 & 0
\end{bmatrix}
\tag{5}
$$</p>

식 (5)와 같은 경우, 3번째 행의 성분이 모두 0이 되면서 피벗이 존재하지 않게 됩니다.. 이러한 경우, 행렬 $A$는 Not Invertible Matrix, 즉 역행렬이 없는 행렬입니다.

행렬 $A$를 가우시안 소거법을 적용하여 Upper Triangular Matrix 형태로 표현하는 방법에 대해 알아보았습니다. 그럼 이제 가우시안 소거법은 어떻게 사용될까요? 위에서 언급했던 것처럼, 어떤 행렬방정식 $A\boldsymbol{x}=\boldsymbol{b}$의 해를 구할 때 사용됩니다.

우리는 지금까지 행렬 $A$에 대해서만 소거법을 적용하였습니다. 이제 해를 구하려면 $b$까지 고려한 소거법을 적용해야 합니다. 식 (1)을 사용하여 행렬방정식 $A\boldsymbol{x}=\boldsymbol{b}$를 다시 정의해보겠습니다.

<p>$$
A\boldsymbol{x}
=
\begin{bmatrix}
2 & 4 & -2 \\
4 & 9 & -3 \\
-2 & -3 & 7
\end{bmatrix}
\begin{bmatrix}
x_1 \\
x_2 \\
x_3
\end{bmatrix}
=
\begin{bmatrix}
2 \\
8 \\
10
\end{bmatrix}
=
\boldsymbol{b}
\Rightarrow
\left[
\begin{array}{ccc|c}
2 & 4 & -2 & 2 \\ 
4 & 9 & -3 & 8 \\ 
-2 & -3 & 7 & 10 \\ 
\end{array}
\right]
\tag{6}
$$</p>

행렬방정식 $A\boldsymbol{x}=\boldsymbol{b}$는 식 (6)에서 가장 오른쪽 항과 같이 표현할 수 있습니다. 이를 **Augmented Matrix**라고 합니다.

이 Augmented Matrix에 대해서 가우시안 소거법을 진행합니다. 이 때 $\boldsymbol{b}$에 대해서도 가우시안 소거법을 적용합니다. 소거법을 적용하는 과정은 다음과 같습니다.

<p>$$
\left[
\begin{array}{ccc|c}
2 & 4 & -2 & 2 \\ 
4 & 9 & -3 & 8 \\ 
-2 & -3 & 7 & 10 \\ 
\end{array}
\right]
\Rightarrow
\left[
\begin{array}{ccc|c}
2 & 4 & -2 & 2 \\ 
0 & 1 & 1 & 4 \\ 
0 & 1 & 5 & 12 \\ 
\end{array}
\right]
c
\left[
\begin{array}{ccc|c}
2 & 4 & -2 & 2 \\ 
0 & 1 & 1 & 4 \\ 
0 & 0 & 4 & 8 \\ 
\end{array}
\right]
\Rightarrow
U\boldsymbol{x}=\boldsymbol{c}
\tag{7}
$$</p>

이렇게 식 (7)처럼 행렬방정식 $A\boldsymbol{x}=\boldsymbol{b}$에서 행렬 $A$를 Upper Triangular Matrix 형태로 바꾸어 $U\boldsymbol{x}=\boldsymbol{c}$로 다시 정의할 수 있습니다.

우리는 $U\boldsymbol{x}=\boldsymbol{c}$의 해를 구하고 싶습니다. 그런데 식 (7)에서 이미 $x_{3}$의 값이 2라는 것을 알 수 있습니다. 이어서, 여기서 구한 $x_{3}$를 통해 2번째 행에 대입하여 $x_{2}$를 구할 수 있고, 마찬가지로 1번째 행의 $x_{1}$도 구할 수 있습니다. 이렇게 구한 해 $\boldsymbol{x}$는 $(-1, 2, 2)$입니다. 이렇게 Upper Triangular Matrix를 통해 아래쪽 행의 해부터 차례대로 풀어나가는 것을 **후방대입법(Back-Substitution)**이라고 합니다.

이어서, 행렬 $A$를 바로 $U$로 치환시켜줄 수 있는 행렬도 존재합니다. 이를 **소거행렬(Elimination Matrix)**라고 합니다. 즉, 소거행렬은 소거과정을 행렬의 곱의 형태로 만들어내는 것이죠.

소거행렬에 관한 예제를 보기 전에, [이전 포스트](https://jiwonkkim.github.io/linear%20algebra/2020/12/23/introduction_column_space/)에서 행렬 $A=CR$로 표현될 수 있다는 사실을 머릿속에 인지하시면 되겠습니다. 여기서는 $U=EA$가 되겠네요. 즉, Upper Triangular Matrix $U$는 소거행렬 $E$의 열 성분들과 행렬 $A$의 행 성분들의 선형 결합으로 표현할 수 있다는 것을 생각해주세요.

예제를 보겠습니다.

<p>$$
E_{1}A
=
\begin{bmatrix}
{\color{Red}1} & {\color{Green}0} & {\color{Blue}0} \\
\\
\\
\end{bmatrix}
\begin{bmatrix}
{\color{Red}{2}} & {\color{Red}{4}} & {\color{Red}{-2}} \\
{\color{Green}{4}} & {\color{Green}{9}} & {\color{Green}{-3}} \\
{\color{Blue}{-2}} & {\color{Blue}{-3}} & {\color{Blue}{7}}
\end{bmatrix}
=
\begin{bmatrix}
2 & 4 & -2 \\
\\
\\
\end{bmatrix}
\tag{8}
$$</p>

소거행렬 $E_{1}$을 행렬 $A$의 $a_{1,1}$은 그대로 유지하고 $a_{2,1}$와 $a_{3,1}$를 0으로 만들어주는 행렬이라고 합시다. 먼저 우리는 행렬 $A$의 1번째 행은 그대로 가져가야 합니다. 그러기 위해선 소거행렬 $E_{1}$의 1번째 행이 $(1, 0, 0)$으로 이루어져 있어야 합니다. 행의 선형 결합을 생각하면, $E_{1}A$의 1번째 행은 ($E_{1}$의 1행 1열 요소 * $A$의 1번째 행 + $E_{1}$의 1행 2열 요소 * $A$의 2번째 행 + $E_{1}$의 1행 3열 요소 * $A$의 3번째 행)으로 나타내기 때문입니다. 비슷한 원리로, 행렬 $A$의 3번째 행을 그대로 가져가기 위해서는 소거행렬 $E_{1}$의 3번째 행이 $(0, 0, 1)$이 되어야겠죠? (물론 행렬 $A$의 3번째 행을 그대로 가져가진 않을겁니다. 그냥 예로 들어본겁니다.)

자, 이제 성분 $a_{2,1}$과 $a_{3,1}$을 지워야 합니다. 그러기 위해서 우리는 앞선 내용에서 $a_{1,1}$을 $a_{2,1}$, $a_{3,1}$에 맞춰지도록, 2번째 행 + 1번째 행 * (-2), 3번째 행 + 1번째 행 * (1)을 연산하여 $a_{2,1}$와 $a_{3,1}$를 0으로 만들었습니다. 이 부분도 선형 결합적으로 생각하면 쉽습니다. 즉, **$E_{1}A$의 2번째 행은 행렬 $A$의 2번째 행 - 1번째 행 * (2)**, **$E_{1}A$의 2번째 행은 행렬 $A$의 3번째 행 + 1번째 행 * (1)**이 된다는 것을 행의 선형 결합쪽으로 생각해보면, $E_{1}$의 2번째 행은 $(-2, 1, 0)$, $E_{1}$의 3번째 행은 $(1, 0, 1)$이 되야 한다는 것을 알 수 있습니다. 따라서, $E_{1}$은 하단의 식 (9)와 같습니다.

<p>$$
E_{1}A
=
\begin{bmatrix}
{\color{Red}1} & {\color{Green}0} & {\color{Blue}0} \\
{\color{Red}{-2}} & {\color{Green}1} & {\color{Blue}0}\\
{\color{Red}{1}} & {\color{Green}0} & {\color{Blue}1}\\
\end{bmatrix}
\begin{bmatrix}
{\color{Red}{2}} & {\color{Red}{4}} & {\color{Red}{-2}} \\
{\color{Green}{4}} & {\color{Green}{9}} & {\color{Green}{-3}} \\
{\color{Blue}{-2}} & {\color{Blue}{-3}} & {\color{Blue}{7}}
\end{bmatrix}
=
\begin{bmatrix}
2 & 4 & -2 \\
0 & 1 & 1\\
0 & 1 & 5\\
\end{bmatrix}
=A^{\prime}
\tag{9}
$$</p>

이제 좀 감이 오시나요? 식 (9)가 이제 식 (2)와 같아졌습니다. **즉, 식 (9)의 의미는 $A^{\prime}$의 1번째 행 = $A$의 1번째 행, $A^{\prime}$의 2번째 행 = $A$의 1번째 행 * (-2) + $A$의 2번째 행, $A^{\prime}$의 3번째 행 = $A$의 1번째 행 + $A$의 3번째 행이 되겠습니다**.

이제 $a_{3,2}^\prime$를 0으로 만들기만 하면 행렬 $A$는 Upper Triangular Matrix 형태가 됩니다.

앞의 내용과 마찬가지로 이번엔 2번째 행을 사용하여 $a_{3,1}^\prime$를 0으로 만들겠습니다. 소거행렬 $E_{2}$을 행렬 $A$의 $a_{2,2}$은 그대로 유지하고 $a_{3,2}$을 0으로 만들어주는 행렬이라고 합시다. 이제 $A^{\prime}$을 기준으로 3번째 행 + 2번째 행 * (-1) 을 진행하여 $E_{2}A^{\prime}=U$가 되는 $E_{2}$는 하단의 식 (10)과 같습니다.

<p>$$
E_{2}A^{\prime}
=
\begin{bmatrix}
{\color{Red}1} & {\color{Green}0} & {\color{Blue}0} \\
{\color{Red}{0}} & {\color{Green}1} & {\color{Blue}0}\\
{\color{Red}{0}} & {\color{Green}{-1}} & {\color{Blue}1}\\
\end{bmatrix}
\begin{bmatrix}
{\color{Red}{2}} & {\color{Red}{4}} & {\color{Red}{-2}} \\
{\color{Green}{0}} & {\color{Green}{1}} & {\color{Green}{1}} \\
{\color{Blue}{0}} & {\color{Blue}{1}} & {\color{Blue}{5}}
\end{bmatrix}
=
\begin{bmatrix}
2 & 4 & -2 \\
0 & 1 & 1\\
0 & 0 & 4\\
\end{bmatrix}
=U
\tag{10}
$$</p>

이제 $U$가 완성되었습니다. 지금까지의 과정을 정리하면, 행렬 $A$에 $E_{1}$이 곱해져 $A^{\prime}$을 만들고, 행렬 $A^{\prime}$에 $E_{2}$가 곱해져서 $U$가 만들어 졌습니다. 즉, 이를 식으로 나타내면 하단의 식 (11)과 같습니다.
<p>$$E_{2}(E_{1}A)=U \quad \Rightarrow \quad (E_{2}E_{1})A=U \quad \Rightarrow \quad EA=U$$</p>

즉, 소거행렬을 구하는 과정에서 구한 부분적인 소거행렬들($E_{2}, E_{1}$)을 나중에 구해진 행렬부터 처음 구한 행렬까지 순서대로 곱한 것이 소거행렬이 됩니다.

여기까지 행렬의 소거법에 대해서 알아보았습니다.

#### 이번 포스팅 정리
- 행렬방정식 $A\boldsymbol{x}=\boldsymbol{b}$의 해를 구하는 방법으로써, $A$의 역행렬을 구하는 것 말고도 소거법을 사용한 방법이 있다.
- 가우시안 소거법은 어떤 행렬 $A$를 Upper Triangular Matrix의 형태로 만드는 것이다.
- 피벗이란, 선형대수학에서 특정 계산을 수행하기 위한 가우시안 소거법과 같은 임의의 알고리즘에 의해 먼저 선택된 행렬의 성분을 의미한다.
- Upper Triangular Matrix와 곱해진 변수의 해는 후방대입법을 통해 구할 수 있다.
- 행렬 $A$를 Upper Triangular Matrix로 만들기 위해 $A$에 곱해지는 행렬을 소거행렬이라고 한다.

##### 출처:
- [Gilbert Strang 교수님의 MIT OPEN COURSE 강의](https://ocw.mit.edu/courses/mathematics/18-065-matrix-methods-in-data-analysis-signal-processing-and-machine-learning-spring-2018/)
- [Learn Agian! 러너게인 (Linear Algebra)](https://twlab.tistory.com/)
- [Wikipedia - 삼각행렬](https://ko.wikipedia.org/wiki/%EC%82%BC%EA%B0%81%ED%96%89%EB%A0%AC)
- [Wikipedia - 피벗](https://ko.wikipedia.org/wiki/%ED%94%BC%EB%B2%97)
