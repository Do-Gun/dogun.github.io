---
layout: page
title: Fine-Grained Image Classification Competition
description: "2025 Deep Learning Winter Competition<br>Presidential Award Winner!<br><br>University of Seoul, AICOSS<br>Team: Dogun Kim, Donghoon Kang, Jungeun Kim, Junyong Lim"
img: assets/img/fgic/50.png
importance: 4
category: work
---

## 0. Overview

**Fine-Grained Image Classification Competition**

서울시립대학교에서 개최한 2025 산학협력 실습중심 딥러닝 겨울 부트캠프의 Fine-Grained Image Classification 경진대회입니다. 제한된 환경에서 최선의 결과를 도출하기 위해서는 화려한 최신 기술보다 **문제의 본질 파악, 체계적인 실험 설계, 그리고 긴밀한 팀워크**가 더 중요하다는 것을 깨닫게 해준 귀중한 경험이었습니다.

**제약 조건:**
- 제한된 컴퓨팅 리소스 (팀당 RTX 4090 x 2)
- 시간 제약 (약 3일)
- 주어진 데이터만 사용 가능 (외부 데이터 불가)
- Pre-trained 모델 사용 불가

제한된 리소스와 짧은 시간 내에서 최적의 답을 내야 했기에, 화려한 기법보다는 **기본에 충실한 접근**을 취했습니다. EDA를 통해 문제를 정의하고, 데이터 증강으로 불균형을 해소하며, 통제된 실험 환경에서 모델을 비교했습니다.

**주요 성과:**
- 최종 Macro F1 Score: **0.467** (Private) / **0.448** (Public)
- **서울시립대학교 총장상 수상**

---

## 1. Competition Introduction

300개 클래스(항공기 70, 자동차 100, 새 130)의 Fine-Grained 분류 문제입니다.

<div class="row">
    <div class="col-sm-10 mt-3 mt-md-0 mx-auto">
        <img src="{{ 'assets/img/fgic/1.png' | relative_url }}" class="img-fluid rounded z-depth-1">
        <div class="caption"><strong>Fine-Grained Classification</strong></div>
    </div>
</div>

**대회 특징:**
- 클래스 간 미세한 차이 (예: A300 vs A310)
- 제한된 GPU 시간 및 컴퓨팅 자원
- Pre-trained 모델 사용 불가
- 약 3일의 짧은 대회 기간

---

## 2. EDA & Task Definition

### Data EDA

<div class="row">
    <div class="col-sm-10 mt-3 mt-md-0 mx-auto">
        <img src="{{ 'assets/img/fgic/2.png' | relative_url }}" class="img-fluid rounded z-depth-1">
        <div class="caption"><strong>데이터 구성: Train 9,797장, Test 4,178장</strong></div>
    </div>
</div>

- GT 정보 없는 이상치 제거
- Train: 9,797장, Test: 4,178장
- 같은 클래스가 다양한 각도, 조명, 배경에서 촬영된 이미지

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        <img src="{{ 'assets/img/fgic/3.png' | relative_url }}" class="img-fluid rounded z-depth-1">
        <div class="caption"><strong>다양한 촬영 환경의 동일 클래스 예시</strong></div>
    </div>
</div>

### Data Imbalance

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        <img src="{{ 'assets/img/fgic/4.png' | relative_url }}" class="img-fluid rounded z-depth-1">
        <div class="caption"><strong>클래스별 데이터 분포</strong></div>
    </div>
</div>

- 30개 이하 샘플 클래스: 항공기 0/70, 새 130/130, 자동차 76/100
- 심각한 데이터 부족 및 불균형

### Task Definition
**1. 훈련 데이터 불균형 해결**
**2. 데이터 부족 문제 해결**
**3. Fine-grained 분류 가능한 모델 선택 및 검증 필요**

---

## 3. Data Preprocessing

### Data Augmentation

<div class="row">
    <div class="col-sm-10 mt-3 mt-md-0 mx-auto">
        <img src="{{ 'assets/img/fgic/5.png' | relative_url }}" class="img-fluid rounded z-depth-1">
        <div class="caption"><strong>수평 뒤집기 및 무작위 회전 (±45°)</strong></div>
    </div>
</div>

- 수평 뒤집기
- 무작위 회전 (±45°)
- 각 클래스 최소 300개까지 증강

### Train/Validation Split
- 95:5 비율로 층화 샘플링

### Final Dataset

<div class="row">
    <div class="col-sm-10 mt-3 mt-md-0 mx-auto">
        <img src="{{ 'assets/img/fgic/6.png' | relative_url }}" class="img-fluid rounded z-depth-1">
        <div class="caption"><strong>전처리 전후 데이터 비교</strong></div>
    </div>
</div>

- Training: 89,190장
- Validation: 4,695장
- Test: 4,178장

---

## 4. Model Exploration

### Prior Research: ViT 제외

대회 특성상 대규모 데이터 사전 학습이 필요한 ViT는 실험에서 제외하고, 이미지 특성에 따른 inductive bias를 줄 수 있는 CNN 계열 모델 실험을 수립했습니다.

### Model Validation Process

<div class="row">
    <div class="col-sm-8 mt-3 mt-md-0 mx-auto">
        <img src="{{ 'assets/img/fgic/7.png' | relative_url }}" class="img-fluid rounded z-depth-1">
        <div class="caption"><strong>모델 검증 프로세스</strong></div>
    </div>
</div>

**하이퍼파라미터 고정:**
- Optimizer: AdamW
- Scheduler: CosineAnnealingLR
- Learning Rate: 1e-3
- Weight Decay: 1e-4

### Model Selection & Experiments

<div class="row">
    <div class="col-sm-8 mt-3 mt-md-0 mx-auto">
        <img src="{{ 'assets/img/fgic/8.png' | relative_url }}" class="img-fluid rounded z-depth-1">
        <div class="caption"><strong>실험 모델 목록</strong></div>
    </div>
</div>

### Initial Results (10 Epoch)

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        <img src="{{ 'assets/img/fgic/9.png' | relative_url }}" class="img-fluid rounded z-depth-1">
        <div class="caption"><strong>6개 모델 학습 곡선</strong></div>
    </div>
</div>

<div class="row">
    <div class="col-sm-8 mt-3 mt-md-0 mx-auto">
        <img src="{{ 'assets/img/fgic/10.png' | relative_url }}" class="img-fluid rounded z-depth-1">
        <div class="caption"><strong>10 Epoch 성능 비교</strong></div>
    </div>
</div>

EfficientNet-B4, B5 추가 학습 진행

---

## 5. Result

### Additional Training

<div class="row">
    <div class="col-sm-8 mt-3 mt-md-0 mx-auto">
        <img src="{{ 'assets/img/fgic/11.png' | relative_url }}" class="img-fluid rounded z-depth-1">
        <div class="caption"><strong>Epoch별 성능 변화</strong></div>
    </div>
</div>

### Final Score

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        <img src="{{ 'assets/img/fgic/12.png' | relative_url }}" class="img-fluid rounded z-depth-1">
        <div class="caption"><strong>최종 Kaggle 리더보드</strong></div>
    </div>
</div>

- Private: **0.467** / Public: **0.448** (1st)
- Presidential Award Winner!

<div class="row">
    <div class="col-sm-6 mt-3 mt-md-0">
        <img src="{{ 'assets/img/fgic/14.png' | relative_url }}" class="img-fluid rounded z-depth-1" style="height: 400px; width: 100%; object-fit: contain;">
        <div class="caption"><strong>Presidential Award</strong></div>
    </div>
    <div class="col-sm-6 mt-3 mt-md-0">
        <img src="{{ 'assets/img/fgic/15.png' | relative_url }}" class="img-fluid rounded z-depth-1" style="height: 400px; width: 100%; object-fit: contain;">
        <div class="caption"><strong>My Team!!</strong></div>
    </div>
</div>

---

## 6. Analysis

### Why EfficientNet?

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        <img src="{{ 'assets/img/fgic/13.png' | relative_url }}" class="img-fluid rounded z-depth-1">
        <div class="caption"><strong>EfficientNet Compound Scaling</strong></div>
    </div>
</div>

- Compound Scaling으로 효율적 성능 (네트워크 depth, width, resolution을 통해 가장 높은 정확도를 찾음)
- 제한된 리소스 환경에 최적

### Future Improvements
- Ensemble Learning 추가 분석
- K-Fold Cross Validation
- Hyperparameter Tuning
- Image Resize 개선 (Zero-Padding)

---

## Technical Stack
- **Hardware**: RTX 4090 x 2
- **Framework**: PyTorch
- **Model**: EfficientNet-B5
- **Optimization**: AdamW + CosineAnnealingLR

---

## Conclusion

본 프로젝트에서 EfficientNet-B5 모델로 Macro F1 Score 0.467 (Private)을 달성하여 서울시립대학교 총장상을 수상했습니다.

제한된 리소스(RTX 4090 x 2)와 짧은 시간(약 3일) 내에서 최적의 답을 내야 했기에, 화려한 기법보다는 **기본에 충실한 접근**을 취했습니다. EDA를 통해 문제를 정의하고, 데이터 증강으로 불균형을 해소하며, 통제된 실험 환경에서 모델을 비교했습니다.

특히 팀원들과의 긴밀한 소통을 통해 역할을 분담하고, **최대한 많은 실험을 빠르게 시도**하는 것이 핵심이었습니다. ViT 제외 결정, 검증 프로세스 수립, 하이퍼파라미터 고정 등 매 단계마다 팀 논의를 거쳐 효율적으로 실험 범위를 좁혀갔습니다.

제한된 환경에서의 경진대회는 최신 기술보다 **문제 정의, 체계적 실험, 팀워크**가 얼마나 중요한지 깨닫게 해준 귀중한 경험이었습니다.
