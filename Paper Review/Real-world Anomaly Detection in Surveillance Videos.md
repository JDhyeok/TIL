# Real-world Anomaly Detection in Surveillance Videos(작성중)

https://arxiv.org/abs/1801.04264

## 0. Abstract

CCTV 영상은 실세계의 다양한 anomalous(이상) 상황을 포착한다. 이 논문에서는 normal(정상), anomalous(이상) 비디오를 둘 다 학습시키는 방법을 제안한다.

이상 데이터를 weakly labeled training 비디오들을 활용해 deep multiple instance ranking framework를 통해 학습하도록 한다.

해당 모델은 anomalous 비디오가 있는 anomalous bag, normal 비디오가 있는 normal bag 두 부분으로 나누어 multiple instance learning을 하고 자동적으로 deep anomaly ranking model을 학습시키고 anomalyh score를 예측한다. 추가적으로 loss function에서 사용하는 sparsity 와 temporal smoothness constraints을 소개한다.


## 1. Introduction
- Motivation and contributions
  
  실생활에서는 normal과 anomalous 행동은 종종 모호하다. 따라서 이런 이벤트들을 더 잘 탐지할 수 있는 시스템이 필요하다.

- Anomaly detection을 위해 weakly-labeled training 비디오를 활용함으로서 MIL을 제안한다.
  
- Anomaly score를 학습하기 위해 sparsity,smoothness constraints 와 함께 MIL ranking loss를 사용하는 것을 제안한다.

- large-scale video 1900 real-world 데이터셋이 있다.

- 현재 state-of-the-art의 anomaly detection 접근법에 상응하는 superior performance를 성취한다.

## 2. Related Work

### Anomaly Detection

### Ranking



## 3. Proposed Anomaly Detection Method

### 3.1 Multiple Instance Learning

### 3.2 Deep MIL Ranking Model

## 4. Dataset

### Previous datasets

### Our dataset

## Experiments