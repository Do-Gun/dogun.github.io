---
layout: page
title: Multi-Modal Learning for Autonomous Driving with Nighttime Data Augmentation
description: "GAN-based Multimodal Training Data Construction and Learning<br>Creativity Award Winner! <br><br>과학기술정보통신부 / 한국연구재단<br>Team: Dogun Kim, Gawon Seo"
img: assets/img/kdata/1.png
importance: 5
category: work
---

## 0. Overview

2박 3일간 진행된 데이터 사이언스 해커톤에서 생성형 모델을 활용한 데이터 증강 및 멀티모달 학습 프로젝트를 수행하였습니다.

스스로 주제를 선정하고 직접 프로세스를 설계하여 구현했으나, 성능은 기대와 달리 하락하는 결과를 보였습니다.

비록 당시에는 뼈아픈 실패였지만, 부족한 부분을 분명히 인식하게 해 주었고 이론적 기반과 실험 설계 능력을 강화해 나가야겠다는 동기를 부여해준 경험이었습니다.


**K-Data Science Hackathon**

**대회 정보:**
- 주최/주관: 과학기술정보통신부 / 한국연구재단
- 기간: 2박 3일
- 주제: 야간 자율주행 환경에서의 3D Object Detection 성능 향상
- 성과: **창의상 수상**

---

## 1. Problem Statement

### 1.1 Autonomous Driving Commercialization
- 국내 자율주행 상용화가 가속화되는 추세 (예: 강릉시 자율주행 마실버스)  
- 기대 효과: 교통사고 감소(90% 이상 운전자 과실), 고령화 사회 대응, 교통 혼잡 완화  

### 1.2 Sensor Limitations in Nighttime
- **Camera**: 저조도에서 노이즈 증가, 빛 반사에 취약  
- **LiDAR**: 원거리에서 포인트 밀도 감소 → 객체 탐지 성능 저하  

핵심: 두 센서의 상반된 특성을 **서로 보완**할 수 있는 전략 필요  

### 1.3 Nighttime Dataset Scarcity
- 대부분의 데이터셋은 주간 환경 중심  
- 야간 데이터는 고품질 픽셀 단위 라벨 확보가 어려움  

---

## 2. Core Idea

**Two-Stage Approach**

- **Stage 1: Data Augmentation**  
  GAN 기반 **주간 → 야간 이미지 변환**으로 야간 학습 데이터 확보  

- **Stage 2: Sensor Fusion Learning**  
  Camera(2D Image)와 LiDAR(3D Point Cloud)의 상반된 특성을 융합  
  멀티모달 학습을 통해 야간 환경에서 3D Object Detection 성능 개선 시도  

---

## 3. Data Analysis & Preprocessing

### 3.1 Dataset
- **AI Hub**: 도심 및 자동차 전용도로 주간/야간 데이터셋  
- **nuScenes**: Multi-modal 데이터셋, Pre-training 용도로 활용  
- Split: Train : Val : Test = 8 : 1 : 1  

### 3.2 2D Image EDA & Preprocessing
- **CycleGAN Turbo Augmentation**  
  - 기존 CycleGAN 대비 세부 구조 보존 및 추론 속도 개선  
  - 변환 이미지의 Pixel Intensity Histogram 분석 → 실제 야간 데이터 분포와 유사성 확인  

### 3.3 3D Point Cloud EDA & Preprocessing
- **Noise Filtering**: Statistical Outlier Removal 적용  
- **Down Sampling**: 객체별 포인트 밀도 고려 → 희소 객체 손실 최소화  
- **Augmentation**: Z축 기준 Random Rotation 적용  

---

## 4. Modeling

- **Model**: IS-FUSION (CVPR 2024) – Instance & Scene 정보를 모두 활용하는 멀티모달 3D 탐지 모델  
- **Training**: AdamW (lr=1e-4), 20 epochs, CBGS 샘플링 전략, AutoAlignV2 augmentation  
- **Metric**: mAP (mean Average Precision)  

---

## 5. Result

- GAN 증강 데이터로 학습한 모델은 **성능이 오히려 저하**됨  
  - Validation mAP: **68.3 → 57.2**  
- 멀티모달 학습 자체의 가능성은 확인했으나,  
  단순 증강 데이터는 실제 성능 개선으로 이어지지 않음을 경험  

---

## 6. Conclusion

GAN 기반 야간 데이터 증강은 기대와 달리 성능 개선 효과를 보이지 않았습니다. 하지만 이 경험을 통해 **“데이터의 질”이 단순한 양적 확장보다 훨씬 중요하다**는 교훈을 얻을 수 있었습니다.  

짧은 시간 동안 주제 선정, 프로세스 설계, 멀티모달 학습 시도를 직접 해본 과정은 부족함을 깨닫게 했고, 동시에 더 체계적인 학습과 실험 설계의 필요성을 느끼게 해주었습니다.  

또한 대회가 성능보다는 아이디어와 데이터 활용 과정에 중점을 두었기에, **과학기술정보통신부 / 한국연구재단 창의상**을 수상할 수 있었습니다.  

---

## Technical Stack
- **Hardware**: RTX 4090 x 2  
- **Framework**: PyTorch  
- **Model**: IS-FUSION (CVPR 2024)  
- **Generative Model**: CycleGAN Turbo  

---

