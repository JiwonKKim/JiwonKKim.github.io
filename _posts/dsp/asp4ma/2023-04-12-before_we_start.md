---
layout: post
title: 들어가기에 앞서
category: Audio Signal Processing for Music Applications
tags: [Audio Signal Processing, Xavier Serra, Coursera]
data: 2023-04-12 22:00:00+0900
comments: true
---

#### 들어가기에 앞서
회사에 들어가고 Speech Representation Model 개발에 관한 업무를 하고 있다. 그래서 요즘 적절한 모델 학습을 위해서 논문들을 읽고 있는데, 이 분야의 발전이 너무 빠른 것 같다. 어떤 구조의 블록을 사용해야 모델이 더 좋은 성능을 내는지, 어떤 손실 함수를 사용해야 모델이 적절히 수렴하는지와 같은 내용을 어렴풋이나마 파악하기 바쁘다.


딥 러닝 학계는 하루가 멀다고 기존의 SOTA 모델 성능을 뛰어넘었다고 서로 앞다투어 보고한다. 그 과정에서, 다들 어떻게든 자신들이 먼저 이러한 아이디어를 고안했으니 감히 넘보지 말라는 식의 깃발 꽂기식 연구가 진행되고 있는 것 같다. 이러한 흐름에 떠밀려 본인도 허겁지겁 논문을 작성했었다. (물론 대차게 망했다.)


겨우 4년 차 딥 러닝 엔지니어인 본인이 감히 이런 말을 하기에는 다소 조심스럽다. 하지만, 대부분의 딥 러닝 논문들을 읽어보면 이것이 왜 잘 되는지 명확하게 설명된 논문들의 거의 없다고 생각된다. 성능이 잘 나오는 이유는 '경험적으로 이렇게 해보니 더 잘 되더라', '이렇게 하면 더 잘될 것 같아서 실험해보니 실제로 좋더라'라는 설명들이 많이 보였다. 하지만 본인도 이쪽에 종사하는 사람으로서, 이러한 성능 향상들은 수많은 실험 중 잘된 실험만 추려서 얻어낸 결과라는 것을 알고 있다. 


딥 러닝 모델이 왜 좋은지, 왜 이런 결과를 보이는지를 명확하게 알 수 없기 때문에 개인적으로는 조금 답답하다. 좋은 성능에 대한 근거를 파악하기 어려운 이유는 확률과 통계, 신호처리와 같은 배경지식이 깊지 않기 때문이라고 생각한다. 따라서 본인은 '다시금' 기초부터 갈고 닦아보자 한다. 오디오 도메인의 기초부터 신호처리, 확률과 통계, 그리고 중간에 포스팅하다가 말았던 딥 러닝까지 다시 한번 정리하고자 한다. 당분간은 내가 Coursera에서 들었던 Xavier Serra 교수의 [Audio Signal Processing for Music Applications](https://www.coursera.org/learn/audio-signal-processing)부터 시작하고자 한다. 이후에는 Tom Bäckström 교수의 [Introduction to Speech Processing](https://speechprocessingbook.aalto.fi/)에 나오는 내용, 그리고 [공돌이의 수학정리노트 블로그](https://angeloyeo.github.io/)에서 다루는 내용, 다 진행하지 못한 Andrew Ng 교수의 [Machine Learning](https://www.coursera.org/learn/machine-learning) 강의도 정리해보고자 한다.
