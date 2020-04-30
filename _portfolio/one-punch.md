---
layout: post
title: "딥러닝을 활용한 독성약재 감별 정보시스템"
img: "assets/img/portfolio/one-punch.png"
date: 30 April 2020
tags: [ortfolio]
---
<div align="center">
  <a href="https://github.com/realblack0/One-Punch">
    <img src="https://user-images.githubusercontent.com/50395556/80699414-e2de9200-8b16-11ea-8e45-112a27b41a9c.png">
  </a>
</div>

# [독약한방 프로젝트](https://github.com/realblack0/One-Punch)

## 프로젝트 개요

일반 한약재와 유사하게 생긴 독성 한약재는 육안으로 구별하기 어렵습니다.  
독성 약재는 숙련된 전문가들이 육안으로 감별 후 유통하고 있으며, 민간인이 독성 약초를 일반 야생 약초로 오인하고 복용하는 사고가 여전히 발생하고 있습니다.  
육안으로 구별하기 힘든 일반 약재와 독성 약재를 CNN 계열 딥러닝으로 분류하고, 약재에 대한 정보를 제공하고자 합니다.

**관련 기사**
> - ['독초 달여 먹는 민간요법'에 사망사고 잇따라…"주의 필요"](https://www.yna.co.kr/view/AKR20190819050400054), 2019-08-19
> - [독성(毒性) 주의 한약재 복용 70대남성 사망](http://www.dailymedi.com/detail.php?number=843884), 2019-06-16

### 프로젝트 기간

2019년 7월 ~ 8월

### 팀원 구성

- 빅데이터 청년 인재 고려대학교 1조
- 한의학 전공, 컴퓨터공학 전공, 통계학 전공, 경영학 전공 등 6명

## UI 소개

### 1. 메인 화면

![First_Screen-compressor](https://user-images.githubusercontent.com/50395556/80694291-4369d100-8b0f-11ea-86ea-45fa9f0b34d3.png)

독약한방 프로젝트는 웹 인터페이스로 개발된 정보시스템입니다.

### 2. 가이드 화면

![Guide-compressor](https://user-images.githubusercontent.com/50395556/80694294-45339480-8b0f-11ea-9406-b9933002cd74.png)

감별할 약재 이미지를 드래그 앤 드롭 방식으로 업로드할 수 있습니다.

### 3. 대시보드

![Main_Page-compressor](https://user-images.githubusercontent.com/50395556/80694299-45cc2b00-8b0f-11ea-88f4-03a6a8013c1a.png)

독성 유무를 감별할 뿐만 아니라 해당 약재의 기원 및 효능, 비슷하게 생긴 약재, 약재 관련 최신 뉴스, 서식지 지도 등 약재 관련 정보를 제공합니다.  
딥러닝의 독성 판단 근거를 이미지로 제공함(**설명가능한 인공지능**)으로써 사용자가 올바른 판단을 할 수 있도록 보조합니다.

## 개발 과정

### 데이터 수집

#### 1. 직접 수집

![data](https://user-images.githubusercontent.com/50395556/80697441-ea506c00-8b13-11ea-9f82-7ff8c8ae6e98.png)

- 독성 약재 : 독성 약재는 시중 유통이 제한되므로 전문가의 도움을 받아 중국에서 공수하여 독성약재 이미지 데이터셋을 구축하였습니다.
- 일반 약재 : 전문가로부터 목록을 받아 약재 구입 후 직접 촬영하였습니다.

#### 2. Data Augmentation

![augmentation](https://user-images.githubusercontent.com/50395556/80697331-c7be5300-8b13-11ea-9597-e241b52b7dbe.png)

Image Data Augmentation 기법으로 데이터셋을 확장하여 약 38,000 장의 학습데이터를 확보하였습니다.

### 모델링

#### 1. Transfer Learning

![mobilenet-v2](https://user-images.githubusercontent.com/50395556/80697189-9776b480-8b13-11ea-8888-ffa7ccd12c25.png)

MobileNet-v1, ResNet50, VGG16, VGG19, LeafNet, DenseNet201 등의 pre-trained model을 기반으로 Transfer Learning을 시도하였습니다.  
성능(precision, recall)과 시간(processing time)을 비교하여 MobileNet-v2를 기반으로 최종모델을 개발하였습니다.

### 2. XAI(설명가능한 딥러닝)

![xai](https://user-images.githubusercontent.com/50395556/80697302-bffeae80-8b13-11ea-87a8-b78d593766a4.png)

설명가능한 딥러닝 기법으로 Feature map visualization, Grad-CAM(Gradient-weighted Class Activation Mapping), LIME(Local Interpretable Model-Agnostic Explanation)을 사용하였습니다.

### 기술 스택

> - Tensorflow
> - Keras
> - Flask
> - SQLAlchemy
> - OpenCV
> - Dash
> - python
