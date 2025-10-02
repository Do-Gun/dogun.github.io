---
layout: page
title: Open-Vocabulary Object Detection on Edge Device
description: "YOLO-World deployment on Jetson Xavier NX for Safety and Security Applications<br><br>University of Seoul, Intelligent Robotics Course<br>Team: Dogun Kim, Gawon Seo"
img: assets/img/ovod/1.gif
importance: 3
category: work
---

## 0. Overview

YOLO-World 기반 Open-Vocabulary Object Detection 시스템을 Jetson Xavier NX에 실시간 배포하여 사이렌 오더 도난 방지, 용의자 추적 등 안전/보안 응용 분야에 적용한 프로젝트입니다.

**Key Features**:
- 사전 정의된 클래스 없이 자유로운 텍스트 입력으로 객체 검출
- 웹 인터페이스를 통한 사용자 친화적 접근
- Fine-tuning으로 작은 객체/밀집 객체 성능 개선

<div class="row">
    <div class="col-sm-6 mt-3 mt-md-0">
        <img src="{{ 'assets/img/ovod/1.gif' | relative_url }}" class="img-fluid rounded z-depth-1" style="height: 400px; width: 100%; object-fit: contain;">
        <div class="caption"><strong>실시간 Open-Vocabulary Detection 데모</strong></div>
    </div>  
    <div class="col-sm-6 mt-3 mt-md-0">
        <img src="{{ 'assets/img/ovod/2.png' | relative_url }}" class="img-fluid rounded z-depth-1" style="height: 400px; width: 100%; object-fit: contain;">
        <div class="caption"><strong>NVIDIA Jetson Xavier NX</strong></div>
    </div>
</div>

---

## 1. Motivation

### Real-world Problems
- **사이렌 오더 도난**: 카페에서 고객 인상착의 기반 추적 필요
- **미아 찾기/용의자 추적**: CCTV 영상에서 특정 인물 검거

### Traditional Object Detection의 한계
- 사전 정의된 클래스(80개 COCO classes)만 검출 가능
- "빨간 옷 입은 사람", "흰 모자 쓴 남성" 등 세부 특징 검출 불가

### Open-Vocabulary Solution
- 임의의 text description으로 zero-shot detection
- "person in blue shirt", "the tallest person" 등 자유로운 표현 가능
- 재학습 없이 즉시 새로운 객체 검출

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        <img src="{{ 'assets/img/ovod/3.png' | relative_url }}" class="img-fluid rounded z-depth-1">
        <div class="caption"><strong>Open-Vocabulary Setting: 사전 정의된 클래스 없이 자유로운 텍스트로 객체 검출</strong></div>
    </div>
</div>

---

## 2. Method

### YOLO-World (CVPR 2024)
- **Architecture**: CLIP + YOLO 결합
- **특징**: 
  - Open-vocabulary zero-shot detection
  - Text 기반 유연한 객체 검색
  - Real-time 추론 속도 (기존 대비 20배 빠름)

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        <img src="{{ 'assets/img/ovod/4.png' | relative_url }}" class="img-fluid rounded z-depth-1">
        <div class="caption"><strong>YOLO-World 모델 아키텍처</strong></div>
    </div>
</div>

### Issues Discovered

- **Issue 1: Closely Packed Objects 검출 실패**
- **Issue 2: Small Object Detection 성능 저하**



<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        <img src="{{ 'assets/img/ovod/5.png' | relative_url }}" class="img-fluid rounded z-depth-1">
        <div class="caption"><strong>초기 모델의 한계: 작은 객체와 밀집된 객체 검출 실패</strong></div>
    </div>
</div>

### Fine-tuning Solution

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        <img src="{{ 'assets/img/ovod/6.png' | relative_url }}" class="img-fluid rounded z-depth-1">
        <div class="caption"><strong>Fine-tuning 전략: 도메인 특화 데이터셋으로 성능 개선</strong></div>
    </div>
</div>

**데이터셋**:
- **SKU-110k**: 밀집 상품 객체 (Issue 1 해결)
- **VisDrone**: 드론 촬영의 작은 사람, 차량 (Issue 2 해결)

**학습 설정**:
- 20 epochs, batch 16, image 640px
- Learning rate: 0.01 → 0.0001 (cosine decay)

---

## 3. Results

### Performance Improvement

#### SKU-110k Dataset
**성능 개선**: mAP@50 0.097 → **0.926** 

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        <img src="{{ 'assets/img/ovod/7.png' | relative_url }}" class="img-fluid rounded z-depth-1">
        <div class="caption"><strong>SKU-110k Fine-tuning 학습 곡선</strong></div>
    </div>
</div>

<div class="row">
    <div class="col-sm-8 mt-3 mt-md-0 mx-auto">
        <img src="{{ 'assets/img/ovod/8.png' | relative_url }}" class="img-fluid rounded z-depth-1">
        <div class="caption"><strong>SKU-110k 정량적 평가: Precision 0.192 → 0.914, Recall 0.011 → 0.872</strong></div>
    </div>
</div>

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        <img src="{{ 'assets/img/ovod/9.png' | relative_url }}" class="img-fluid rounded z-depth-1">
        <div class="caption"><strong>SKU-110k 정성적 평가: Fine-tuning 전후 밀집 객체 검출 성능 비교</strong></div>
    </div>
</div>

#### VisDrone Dataset
**성능 개선**: mAP@50 0.052 → **0.426** 

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        <img src="{{ 'assets/img/ovod/10.png' | relative_url }}" class="img-fluid rounded z-depth-1">
        <div class="caption"><strong>VisDrone Fine-tuning 학습 곡선</strong></div>
    </div>
</div>

<div class="row">
    <div class="col-sm-8 mt-3 mt-md-0 mx-auto">
        <img src="{{ 'assets/img/ovod/11.png' | relative_url }}" class="img-fluid rounded z-depth-1">
        <div class="caption"><strong>VisDrone 정량적 평가: Precision 0.071 → 0.527, Recall 0.079 → 0.414</strong></div>
    </div>
</div>

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        <img src="{{ 'assets/img/ovod/12.png' | relative_url }}" class="img-fluid rounded z-depth-1">
        <div class="caption"><strong>VisDrone 정성적 평가: Fine-tuning 전후 작은 객체 검출 성능 비교</strong></div>
    </div>
</div>

#### Final Performance
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        <img src="{{ 'assets/img/ovod/30.png' | relative_url }}" class="img-fluid rounded z-depth-1">
        <div class="caption"><strong> 최종 Fine-tuning 정성 평가</strong></div>
    </div>
</div>

---

## 4. Demo

### Web Demo - Zero-shot Image Detection

<div class="row">
    <div class="col-sm-4 mt-3 mt-md-0">
        <img src="{{ 'assets/img/ovod/13.png' | relative_url }}" class="img-fluid rounded z-depth-1" style="height: 300px; width: 100%; object-fit: contain;">
        <div class="caption"><strong>차량 세부 파츠 검출</strong></div>
    </div>
    <div class="col-sm-4 mt-3 mt-md-0">
        <img src="{{ 'assets/img/ovod/14.png' | relative_url }}" class="img-fluid rounded z-depth-1" style="height: 300px; width: 100%; object-fit: contain;">
        <div class="caption"><strong>동물 세부 구조 검출</strong></div>
    </div>
    <div class="col-sm-4 mt-3 mt-md-0">
        <img src="{{ 'assets/img/ovod/15.png' | relative_url }}" class="img-fluid rounded z-depth-1" style="height: 300px; width: 100%; object-fit: contain;">
        <div class="caption"><strong>인물 특징 검출</strong></div>
    </div>
</div>

### Real-time Demo

<div class="row">
    <div class="col-sm-6 mt-3 mt-md-0">
        <img src="{{ 'assets/img/ovod/1.gif' | relative_url }}" class="img-fluid rounded z-depth-1" style="height: 400px; width: 100%; object-fit: contain;">
        <div class="caption"><strong>실시간 카메라 검출 - 연구실 환경</strong></div>
    </div>
    <div class="col-sm-6 mt-3 mt-md-0">
        <img src="{{ 'assets/img/ovod/16.gif' | relative_url }}" class="img-fluid rounded z-depth-1" style="height: 400px; width: 100%; object-fit: contain;">
        <div class="caption"><strong>다양한 free-form text 입력 처리</strong></div>
    </div>
</div>

Input Text: white desk, red pen, blue pen, black pen, black monitor, blue chair, person in black shirt, white board, person in white shirt, water bottle, cup of coffee, black keyboard, orange book, orange cup, instant cup ramen, red straw


---

## 5. Applications

### Case 1: 사이렌 오더 도난 방지

<div class="row">
    <div class="col-sm-6 mt-3 mt-md-0">
        <img src="{{ 'assets/img/ovod/17.png' | relative_url }}" class="img-fluid rounded z-depth-1" style="height: 400px; width: 100%; object-fit: contain;">
        <div class="caption"><strong>카페 전체 물체 검출</strong></div>
    </div>
    <div class="col-sm-6 mt-3 mt-md-0">
        <img src="{{ 'assets/img/ovod/18.png' | relative_url }}" class="img-fluid rounded z-depth-1" style="height: 400px; width: 100%; object-fit: contain;">
        <div class="caption"><strong>인상착의 기반 추적</strong></div>
    </div>
</div>

### Case 2: 용의자 추적 / 미아 방지

<div class="row">
    <div class="col-sm-6 mt-3 mt-md-0">
        <img src="{{ 'assets/img/ovod/19.png' | relative_url }}" class="img-fluid rounded z-depth-1" style="height: 400px; width: 100%; object-fit: contain;">
        <div class="caption"><strong>원본 이미지</strong></div>
    </div>
    <div class="col-sm-6 mt-3 mt-md-0">
        <img src="{{ 'assets/img/ovod/20.png' | relative_url }}" class="img-fluid rounded z-depth-1" style="height: 400px; width: 100%; object-fit: contain;">
        <div class="caption"><strong>인상착의 기반 인물 검출</strong></div>
    </div>
</div>


---

## 6. Conclusion

본 프로젝트는 Open-Vocabulary Detection의 실제 응용 가능성을 입증하고, Fine-tuning을 통해 작은 객체 및 밀집 객체 검출 성능을 대폭 개선하였습니다. Edge device 배포로 GPU가 없는 실제 CCTV 환경에서도 실용적으로 활용 가능한 시스템을 구축하였습니다.
