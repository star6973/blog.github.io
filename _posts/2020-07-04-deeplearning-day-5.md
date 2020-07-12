---
layout: post
title:  Deep Learning [Day 5]
date:   2020-07-04 10:00:00-19:00:00
categories: DeepLearning
tag: DeepLearning
use_math: true
---

## 8. (GAN))
### 8.1. GAN Theory

Generator 모델은 입력값이 없으므로 아무 의미없는 랜덤값으로 신경망을 만듦.

도둑 -> 입력 10x10, 히든 256, 아웃풋: 28x28(true 데이터의 값과 같아야 하므로)
경찰 -> 입력 28x28, 히든 256, 아웃풋: 1(0 또는 1)

-를 넣어야 minimize


자기 자신의 변수만을 학습 -> 상대방의 학습 모델을 변형시키는 것이 아니라, 자신의 성능을 높이는 것이 중요함.



sample_G: 최종 원하는 출력값

### 8.2. GAN Implementation




### 9. Interpolation



### 10. PCA/LDA

차원 축소 방법
1) PCA: 차원이 축소되더라도 특징이 그대로 유지(아이겐벨류가 가장 긴쪽이 기준을 잡아 그쪽으로 짜부시키기)
2) LDA: 차원이 축소되더라도 분류가 잘되어야 함(분산이 작아야함-똘똘 뭉쳐야함, 거리가 멀어야함-나누기 쉽게)

