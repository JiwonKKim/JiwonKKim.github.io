---
layout: post
title:  "기계학습(Machine learning) 기초 (2)"
category: Machine Learning
tags: [Machine Learning, Andrew Ng, Supervised Learning, Unsupervised Learning, 머신러닝, 기계학습, 지도학습, 비지도학습]
comments: true
---

<br><br>
**이 글을 읽기 전에**<br>
- _해당 포스팅은 Coursera에 올라와 있는 Andrew Ng 교수님의 Machine Learning 강좌를 정리/각색하였습니다._
- _해당 포스팅은 개인적으로 공부한 내용을 정리한 것으로, 일부 내용에 오류가 있을 수 있습니다._
- _포스팅에 오류가 있을 시, 댓글로 남겨주시면 감사하겠습니다._

---

지난 포스팅에서는 기계학습의 정의와 기계학습의 유형 중 하나인 지도학습에 대하여 알아보았습니다. 이번 포스팅에서는 지도학습과 상반되는 개념인 **비지도학습(Unsupervised Learning)**에 대하여 알아보도록 하겠습니다.<br>

지도학습에서는 학습에 사용될 데이터의 레이블(Label), 즉 정답이 주어져 있습니다. 반대로 비지도학습은 레이블이 없는 데이터를 사용합니다.<br>

<center><img src="/assets/ml/02_introduction/Fig01_unsupervised_learning.png" width="400" height="400"></center>
<center>Fig. 1. Clustering with Unsupervised Learning</center>

<br>
Fig. 1 에서 레이블이 없는 데이터의 분포를 볼 수 있습니다. 각각의 데이터는 $x_{1}$과 $x_{2}$ 두 가지의 정보만 가지고 있을 뿐, 이 두 가지의 정보가 무엇을 의미하는지는 포함하고 있지 않습니다. 우리는 데이터의 분포만 그래프를 통해 알 수 있습니다. 레이블이 주어져있지 않아도, 우리는 그 데이터의 분포를 이용하여 비지도학습을 통해 다양한 정보를 유추할 수 있습니다. 그 대표적인 예로는 **클러스터링(군집화, Clustering)**가 있습니다. 군집화란, 어떤 특정 데이터셋에서 같은 특성(Feature)을 공유하는 클러스터(통계 조사의 대상 범위(모집단)의 요소를 몇 개 모은 단위체)를 찾는 비지도학습 문제입니다. 예를 들면, Fig. 1 에 표시되어 있는 8개의 데이터를 2개의 클러스터로 나눈다면, 데이터의 분포로 보았을 때 비슷한 포인트를 가지고 있는 데이터끼리 클러스터링하는게 제일 합리적일 것입니다. 따라서, 데이터를 Fig. 1 에 나와있는 빨간 동그라미처럼 2개의 클러스터로 분리시킬 수 있습니다. 물론 이 클러스터링 외에도 **GAN(Generative Adversarial Network, Will be posted later)** 등 다양한 분야에서 사용됩니다.

**즉, 비지도학습은 데이터가 의미하는 바가 알려지지 않은 상태에서 데이터의 분포를 통해 데이터의 특징/구조를 파악하는데 사용됩니다.**

생각보다 강의 내용이 적어서 포스팅이 짧았네요. 다음 포스팅에서는 모델 표현(Model Representation)에 대하여 포스팅하겠습니다.

##### 출처:
- [Andrew Ng - Machine Learning](https://www.coursera.org/learn/machine-learning)
