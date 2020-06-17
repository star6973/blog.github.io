---
layout: post
title:  Deep Learning [Day 1]
date:   2020-06-17 10:00:00-19:00:00
categories: DeepLearning
tag: DeepLearning
---

## 고려대학교 김중헌 교수님
### Curriculum
- Deep Learning Thoery and Software  

1. Machine Learning Overview
  1) AI는 전 분야에 미칠 수 있는 기술
  2) 강화학습은 지도학습/비지도학습의 단일 응답 구조와 다르게 끊임없이 응답하는 구조(바둑의 경우 한 수를 두고 끝나는 것이 아니라 계속해서 인공지능이 생각하면서 진행)
<img>

2. Introduction
  <img>
  
  1) How Deep Learning Works?

    - 딥러닝 프로세스 과정

      a) 모델 만들기(껍데기) - MLP, CNN, RNN, GAN, Customized
         > input -> 4개의 히든레이, 7개의 유닛(MLP) -> output

      b) 훈련하기 -> Input: data, Output: labels
      
      c) 테스팅/추론
  
    - 딥러닝 과정에서 발생되는 문제

      a) Overfitting: 2단계에서 발생되는 문제로, 데이터가 충분하지 않은 경우 발생한다.  
         -> 훈련 결과는 엄청 높게 나왔는데, 테스트 결과는 엄청 낮은 것
         -> 침대를 학습시키고(나의 잠자리 유형) 팔면 망함

  2) Two major Deep Learning Models(CNN, RNN)

    - CNN(Convolutional Neural Network)
      : 기존의 딥러닝은 1차원 구조만 입력이 가능했지만, CNN은 2차원 구조(이미지), 3차원 구조(영상)을 훈련시킬 수 있다.

    - RNN(Recurrent Neural Network)
      : 









