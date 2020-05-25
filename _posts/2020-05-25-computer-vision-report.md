---
layout: post
title:  Computer Vision Report I
date:   2020-05-25 19:00:00
categories: deep learning
tags: deep learning
---

# 딥러닝 기반 객체 인식 기술 동향(ETRI)  

과거의 객체 인식 연구 - SHIFT(Scale Invariant Feature Transform)
                    - SURF(Speeded-Up Robust Features)
                    - Haar
                    - HOG(Histogram of Oriented Gradients)

- 객체가 가지는 특징을 설계하고 검출  
- 책의 경우, 영상에서는 사다리꼴 형태로 나타나고 꼭짓점에서는 각이 생기는 특징 -> DPM(Deformable Part-based Model)을 이용해 물체를 여러 부분으로 나누어 특징 정보 구성 -> SVM(Support Vector Machine)을 이용해 기계학습  
- 합성곱 신경망(CNN) 등장 이후부터 딥러닝 이용  
- CNN의 인식률 향상을 위해 ZFNet, VGG, ResNet, GoogLeNet, DenseNet 등이 등장  
- CNN을 통해 영상에서 객체가 무엇(what)인지 인식은 성공하였으나, 객체가 어디(where)에 존재하는 지 고민  
  + R-CNN(Region-based CNN)은 where를 찾기 위해 딥러닝 회귀 방법으로 해결한 초기 연구  
  + Fast R-CNN은 R-CNN의 느린 속도를 보완하였지만, 객체의 후보 영역을 찾기 위해 딥러닝을 이용할 수 없었음.  
  + Faster R-CNN은 검출 속도를 향상시키고, 딥러닝만을 이용해 객체 인식 구현 가능  
  + R-FCN은 Faster R-CNN의 영상의 지역적 정보에 의존적이라는 점을 보완  
- YOLO(You Only Look Once)는 실시간에 가까운 처리 속도 문제와 객체 인식의 모든 과정을 하나의 딥러닝 네트워크로 구성하는 방법을 제안했다.  




## CNN(Convolutional Neural Network) 관련 논문  
### 1. 
### 2. 
### 3. 
### 4.
## 
