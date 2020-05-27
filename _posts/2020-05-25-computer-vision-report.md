---
layout: post
title:  Computer Vision Report I
date:   2020-05-25 19:00:00
categories: deep learning
tags: deep learning
---

# 딥러닝 기반 객체 인식 기술 동향(ETRI)  

### 1. 과거의 객체 인식 연구 
1.1. 과거의 객체 인식 연구는 SHIFT(Scale Invariant Feature Transform), SURF(Speeded-Up Robust Features), Haar, 4) HOG(Histogram of Oriented Gradients) 등과 같이 객체가 가지는 특징을 설계하고 검출함으로써 객체를 찾아내는 방식으로 진행.  

1.2. 예를 들면 책의 경우, 영상에서는 사다리꼴 형태로 나타나고 꼭짓점에서는 각이 생기는 특징 -> DPM(Deformable Part-based Model)을 이용해 물체를 여러 부분으로 나누어 특징 정보 구성 -> SVM(Support Vector Machine)을 이용해 기계학습  

1.3. 합성곱 신경망(CNN) 등장 이후부터 딥러닝을 이용한 객체 인식 방법이 주류가 됨. 따라서 자연스럽게 CNN의 인식률 향상을 위해 ZFNet, VGG, ResNet, GoogLeNet, DenseNet 등이 등장.  

1.4. CNN을 통해 영상에서 객체가 무엇(what)인지 인식은 성공하였으나, 객체가 어디(where)에 존재하는 지 고민  
  - R-CNN(Region-based CNN)은 where를 찾기 위해 딥러닝 회귀 방법으로 해결한 초기 연구  
  - Fast R-CNN은 R-CNN의 느린 속도를 보완하였지만, 객체의 후보 영역을 찾기 위해 딥러닝을 이용할 수 없었음.  
  - Faster R-CNN은 검출 속도를 향상시키고, 딥러닝만을 이용해 객체 인식 구현 가능  
  - R-FCN은 Faster R-CNN의 영상의 지역적 정보에 의존적이라는 점을 보완  
  - YOLO(You Only Look Once)는 실시간에 가까운 처리 속도 문제와 객체 인식의 모든 과정을 하나의 딥러닝 네트워크로 구성하는 방법을 제안  

### 2. 객체 인식 기술 동향  
2.1. 객체 인식 방법  
  - R-CNN(Region-based CNN)  
       후보 영역(Region Proposal)을 생성하고 이를 기반으로 CNN을 학습시켜 영상 내 객체의 위치를 찾아내는 방법이다.        객체 인식 과정은 크게 네 단계로 이루어진다. 1) 입력된 영상에서 선택적 탐색(Selective Search) 알고리즘을 이용하여 후보 영역들을 생성한다. 2) 생성된 각 후보 영역들을 동일한 크기로 변환하고, CNN을 통해 특징을 추출한다. 3) 추출된 특징을 이용하여 후보 영역 내의 객체를 SVM(Support Vector Machine)을 이용하여 분류한다. 4) 첫 번째 단계에서 생성된 후보 영역의 위치는 정확하지 않기 때문에, 최종적으로 회귀학습을 통해 객체의 영역 박스 위치를 더 정확히 보정한다.  
  
  - Fast R-CNN  
       R-CNN은 CNN, SVM, 회귀의 학습 단계가 모두 분리되어 있고, 수천 개의 후보 영역에서 각각의 CNN을 학습해야 하므로 훈련 시간이 많이 소요된다. 이를 보완하기 위해, 하나의 입력 영상에 대해 하나의 CNN을 학습할 수 있는 Fast R-CNN이 등장하였다.  
       학습된 CNN을 통해 생성된 Feature map을 통합하여 특징을 추출한다. 또한, 분류기의 손실과 영역 박스 회귀의 손실을 합하여 동시에 훈련시킴으로써 훈련 단계를 단순화하였다. 분류기로는 Softmax를 사용한다.  
       
  - Faster R-CNN  
       Fast R-CNN에서 후보 영역을 생성하는 선택적 탐색 알고리즘은 CNN 외부에서 수행되기 때문에, 속도 측면에서 비효율적이며, 알고리즘을 학습시킬 수 없다는 단점이 있다.  
       Faster R-CNN은 선택적 탐색 알고리즘을 사용하지 않고, Feature map을 추출하는 CNN의 마지막 층(Layer)에 후보 영역을 생성하는 별도의 CNN인 영역 제안 네트워크(RPN: Region Proposal Network)를 적용한다.  
       RPN은 R-CNN에서 CNN의 출력인 Feature map을 입력으로 받아 객체의 위치를 추정하여 후보 영역을 출력하는 네트워크이다. CNN에서 추출된 Feature map을 RPN에서 추정된 후보 영역으로 잘라내어 객체를 인식한다. 이렇게 Feature map을 추출하는 CNN 과정과 후보 영역을 생성하는 과정을 일련의 네트워크로 구성함으로써 속도 개선을 시켰다.  
  
  - R-FCN

3. 


## CNN(Convolutional Neural Network) 관련 논문  
### 1. 
### 2. 
### 3. 
### 4.
## 
