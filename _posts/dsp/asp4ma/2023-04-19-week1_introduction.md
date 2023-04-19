---
layout: post
title: Week 1 - 오디오 신호 처리의 소개
category: Audio Signal Processing for Music Applications
tags: [Audio Signal Processing, Xavier Serra, Coursera]
data: 2023-04-19 23:00:00+0900
comments: true
---

**이 글을 읽기 전에**

- _해당 포스팅은 Coursera에 올라와 있는 Xavier Serra 교수님의 Audio Signal Processing for Music Applications 강좌를 정리/각색하였습니다._
- _해당 포스팅은 개인적으로 공부한 내용을 정리한 것으로, 일부 내용에 오류가 있을 수 있습니다._
- _포스팅에 오류가 있을 시, 댓글로 남겨주시면 감사하겠습니다._

---

#### Week 1 - Introduction to Audio Signal Processing

>Audio signal processing is an engineering field that focuses on the computational methods for intentionally altering the sounds.<br>
오디오 신호 처리는 소리를 의도적으로 바꾸기 위한 계산 방법에 중점을 둔 공학 분야이다.

<center><img src="/assets/dsp/asp4ma/01_introduction/asp.png" width="500" height="200"></center>
<center>Fig. 1. 신호 처리 개요</center>

<br>
Fig. 1 은 오디오 신호 처리의 블록 다이어그램을 나타냅니다. 즉, 오디오 신호 처리란, 어떤 오디오 신호를 입력으로 받았을 때 어떠한 처리를 통해서 다른 오디오 신호 혹은 다른 유형의 정보를 생성하는 것입니다. 어떠한 처리는 음높이의 변화, 잡음 제거와 같은 동작을 예로 들 수 있겠습니다. Xavier Serra 교수님의 정의에 따르면 [Speech2Face](https://arxiv.org/abs/1905.09773)와 같은 작업도 오디오 신호 처리라고 볼 수 있겠네요.

<center><img src="/assets/dsp/asp4ma/01_introduction/ad.png" width="1000" height="500"></center>
<center>Fig. 2. 아날로그 신호와 디지털 신호</center>

<br>
오디오 신호는 디지털 혹은 아날로그 형식으로 전자적으로(electronically) 표현될 수 있습니다. 아날로그 신호 처리기는 전기 신호에서 직접적으로 동작하고, 디지털 신호 처리기는 신호의 이진 표현(binary representation)을 통해 수학적으로 계산되어 동작합니다.
소리는 매질 입자의 진동으로 인해 발생하는 파형입니다. 우리는 일반적으로 소리를 공기를 통해 듣게 되므로, 소리를 기압 파형이라고 볼 수 있습니다. 아날로그 소리는 이러한 기압 파형을 전기적으로, 즉 전압으로 표현합니다. 아날로그 소리는 연속 함수(continuous function)라는 특징을 가지고 있습니다. 반대로 디지털 소리는 기압 파형을 이진 표현으로 나타내며, 이산 함수(discrete function)입니다. 아날로그에서 디지털로의 변환은 변환 과정에서 원래 정보의 일부가 손실되기 쉽지만, 대부분의 현대 오디오 시스템은 디지털 신호 처리 기술이 아날로그에 기반한 기술보다 훨씬 더 강력하교 효율적이기 때문에, 디지털 신호 처리 방식을 사용합니다. 오디오 신호 처리를 통해 오디오 신호를 디지털 표현으로 녹음하거나, Wav 파일을 더 적은 용량의 MP3 파일로 인코딩, 소리 변환 및 합성 등 다양한 태스크에 적용할 수 있습니다.

<center><img src="/assets/dsp/asp4ma/01_introduction/analysis.png" width="500" height="200"></center>
<center>Fig. 3. 오디오 신호 처리를 통한 소리 분석</center>

<br>
무엇보다, 오디오 신호 처리를 통해 소리가 가진 의미있는 특징을 분석할 수 있습니다. 이러한 분석은 음악 정보 검색(music information retrieval)이라는 분야와도 관련이 있습니다. Fig 3. 에 나와있는 대략적인 알고리즘을 적용하여 다음과 같은 Key들을 얻을 수 있습니다.
- Low-level: loudness, timbre, pitch, ...
- Mid-level: rhythm, harmony, melody, ...
- High-level: genre, emotions, similarity, ...

해당 강의에서는 Mid-level이나 High-level 특징의 신호 분석에 대해 다루지 않고 Low-level 특징에 대해 다룹니다. 소리의 분석은 이 강의에서 중요하게 다루어지며, 자세한 내용은 강의 후반부에 다룹니다.

다음 주차 강의에서는 이산 푸리에 변환(discrete fourier transform, DFT)에 대해 다루겠습니다.

<!-- #### 이번 포스팅 정리
- 로지스틱 회귀로 일부 논리 연산자를 구현할 수 있지만, 출력이 비선형인 연산자는 구현할 수 있다.
- 하지만 신경망은 Hidden Layer를 사용하여 XNOR 연산을 포함한 비선형 연산/함수를 표현할 수 있고, 이를 통해 좋은 성능을 이끌어 낼 수 있다.
- 이러한 특성을 이용해 Multi-Class Classification과 같은 복잡한 문제도 신경망을 통해 해결할 수 있다.
- Label을 0 아니면 1로만 나타내는 것을 One-Hot Encoding이라고 한다. -->

##### 출처:
- [Audio Signal Processing for Music Applications](https://www.coursera.org/learn/audio-signal-processing)
