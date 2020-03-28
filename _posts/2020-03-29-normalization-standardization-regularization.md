---
layout: post
title: "[딥러닝] 정규화? 표준화? Normalization? Standardization? Regularization?"
tags: [MachineLearning]
author: jinhyo
feature-img: /assets/img/pexels/triangular.jpeg 
use_math: true
keywords: [Normalization, Standardization, Regularization, 정규화, 표준화]
---

딥러닝을 공부하다 보면 "정규화" 라는 용어를 참 자주 접하게 된다.  
그런데 애석하게도 Normalization, Standardization, Regularization 이 세 용어가 모두 한국어로 정규화라고 번역된다.  
이 세가지 용어가 다름을 알고 난 뒤로부터 가능한 딥러닝 용어들을 한글이 아닌 영어로 쓰려고 하고 있다.  
매번 헷갈리는 Normalization, Standardization, Regularization의 차이에 대해서 간략히 정리해둔다.  

## Normalization

- 값의 범위(scale)를 0~1 사이의 값으로 바꾸는 것
- 학습 전에 scaling하는 것
    - 머신러닝에서 scale이 큰 feature의 영향이 비대해지는 것을 방지
    - 딥러닝에서 Local Minima에 빠질 위험 감소(학습 속도 향상)
- scikit-learn에서 `MinMaxScaler`

$$
\begin{align*}
{x-x_{min} \over x_{max}-x_{min}}
\end{align*}
$$  

## Standardization

- 값의 범위(scale)를 평균 0, 분산 1이 되도록 변환
- 학습 전에 scaling하는 것
    - 머신러닝에서 scale이 큰 feature의 영향이 비대해지는 것을 방지
    - 딥러닝에서 Local Minima에 빠질 위험 감소(학습 속도 향상)
- 정규분포를 표준정규분포로 변환하는 것과 같음
    - Z-score(표준 점수)
    - -1 ~ 1 사이에 68%가 있고, -2 ~ 2 사이에 95%가 있고, -3 ~ 3 사이에 99%가 있음
    - -3 ~ 3의 범위를 벗어나면 outlier일 확률이 높음
- 표준화로 번역하기도 함
- scikit-learn에서 `StandardScaler`

$$
\begin{align*}
\frac {x-\mu} {\sigma} \space \space \space (\mu:평균, \sigma: 표준편차)
\end{align*}
$$  

## Regularization

- weight를 조정하는데 규제(제약)를 거는 기법
- Overfitting을 막기위해 사용함
- L1 regularization, L2 regularizaion 등의 종류가 있음
    - L1: LASSO(라쏘), 마름모
    - L2: Lidge(릿지), 원

$$
\begin{align*}
Loss = \frac {1} {n} \sum_{i=1}^{n} \{ (y -\hat{y})^2 + \frac {\lambda} {2} |w|^2 \}
\end{align*}
$$

### 참고한 자료
1. [머신 러닝 - Normalization, Standardization, Regularization 비교](https://blog.naver.com/PostView.nhn?blogId=qbxlvnf11&logNo=221476122182&categoryNo=52&parentCategoryNo=0&viewDate=&currentPage=1&postListTopCurrentPage=1&from=postView)  

2. [What is the difference between normalization, standardization, and regularization for data?](https://www.quora.com/What-is-the-difference-between-normalization-standardization-and-regularization-for-data)
