---
layout: post
title: "[딥러닝] 설명가능 인공지능이란? (A Survey on XAI)"
tags: [DeepLearning]
author: jinhyo
feature-img: /assets/img/pexels/triangular.jpeg 
use_math: true
keywords: XAI, eXplainable AI, Decision Tree, Falling Rule List, Linear Model, LIME, Activation, Weight Visualization, Maximization Optimization, Attribution, Deconvolution, CAM, Grad-CAM, PDP, ICE, MMD-critic 
---

## 1. 등장배경

요즘 가장 각광받고 있는 AI(인공지능)은 단연 Neural Network(신경망)이다. 원래는 AI가 더 큰 의미를 지닌 단어이지만 근래에는 인기에 힘입어 AI가 Neural Network라는 뜻으로 통용되기도 한다. 뇌의 신경세포를 모방한 Neural Network는 서로 복잡하게 연결된 수백만개 이상의 parameter(매개변수)가 비선형으로 상호작용하는 구조로, 사람이 그 많은 parameter를 직접 계산하고 의미를 파악하기는 불가능하고 back propagation(오류역전파법) 덕에 간신히 parameter update만 가능하다. 복잡한 구조 덕에 성능은 기존 기계학습보다도 월등히 높아졌다. 어떤 분야에서는 사람보다도 정확하다. 그러나 사람의 인지 영역을 넘어선 내부 구조 탓에 AI가 왜 그런 결과를 도출했는지는 개발자도 알 수 없다. 그래서 AI를 Black Box라고 부르기도 한다.

#### ▷ 설명의 필요성

- AI는 탁월한 성능으로 우리의 일상 속으로 금방 들어올 것 같았지만 Black Box 구조 탓에 AI의 상용화는 순탄하지 않다. A은행이 최근 신용 대출 가능 여부를 판단하는 AI를 도입했다고 하자. 고객 B씨가 A은행에 대출 신청했는데 대출이 거절된다면, B씨는 본인에게 어떤 문제가 있어서 대출이 안되는지 확인을 요구할 것이다. 그러나 A은행은 최신 AI 알고리즘이 빅데이터를 기반으로 판단했다는 답변밖에 할 수 없다.
- A병원은 최근 암을 진단하는 AI를 도입했다고 하자. B씨는 배탈이 난 것 같아서 병원에 방문했다가 AI로부터 암 판정을 받았다. 어떤 증상 때문에 암이라고 진단했는지는 설명하지 않고 최신 AI 알고리즘이 빅데이터를 기반으로 예측했다고 한다. B씨는 정밀검사를 받아야할지 고민에 빠진다.
- 이렇게 AI가 설명을 못해서 불이익을 당할뻔하는 이야기가 상상 속에만 있는 것은 아니다. 실제 2013~2014년 미국 플로리다주 브로워드 카운티에서 이런 일이 있었다. 약 18,000명의 범죄자를 중심으로 향후 2년 내 재범 가능성을 예측하기 위해 범죄 예측 알고리즘을 이용했는데 흑인의 재범 가능성을 백인보다 45% 높게 예측했다. 흑인의 재범 가능을 왜 더 높게 예측했는지는 알 수 없었다. 그러나 실제 동일 기간의 재범율을 조사한 결과 백인의 재범률이 흑인보다 높았다.

## 2. XAI(eXplainable AI, 설명가능 인공지능)

XAI는 사람이 AI의 동작과 최종결과를 이해하고 올바르게 해석할 수 있고, 결과물이 생성되는 과정을 설명 가능하도록 해주는 기술을 의미한다.

#### ▷ XAI를 둘러싼 다양한 움직임

- 인공지능이 중요작업(mission critical)에 사용될 경우 인공지능의 설명성, 투명성 확보 기술, 기준 정립이 필요하다. 2018년 5월 28일, 의사 결정 이유에 대한 설명을 요구하는 EU의 일반 개인정보보호법(General Data Protection Regulation)이 발효되어 의사 결정 이유를 설명할 수 없는 AI 기술은 향후 의료, 군사 등 중요작업에는 사용하기 어렵게 될 것으로 예상된다.
- XAI는 인터넷의 핵심 기술을 개발한 미국 국방성의 연구개발부서 DARPA(Defense Advanced Research Projects Agency)가 주도하고 있다. 과기정통부도 XAI의 중요성을 인식하고 ‘I-Korea 4.0 실현을 위한 인공지능 R&D 전략(2018.5)’의 R&D 로드맵에 2025년까지 설명가능 학습, 추론 기술 개발 추진 계획을 포함했다.

## 3. 연구 동향

비교적 짧은 역사에도 불구하고 AI를 설명하기 위해서 많은 기법이 연구되었다. 현재 XAI 기술을 분류하는 기준은 3가지가 있다. 각 기준은 서로 다른 관점을 취하고 있다. 각 관점은 상하관계가 아니며, XAI 분류는 3가지 관점 중 하나에 귀속시키는 것이 아니다.  

|관점|분류|
|:---:|:---:|
|Complexity|Intrinsic  vs  Post-hoc|
|Scop|Global  vs  Local|
|Dependency|Model-specific  vs  Model-agnostic|

### 3.1. Complexity

모델의 복잡성(Complexity)은 설명력과 깊은 연관이 있다. 모델이 복잡할수록, 사람이 해석하기가 더 어려워진다. 반대로 모델이 단순할수록 사람 해석하기는 더 용이하다. 그러나 어려운 문제를 해결하기 위해서는 복잡한 구조가 유리하기 때문에 모델의 복잡성과 해석력은 서로 trade-off 관계가 있다. Neural Network는 설명력이 낮은 대신에 정확도가 월등하게 높아 큰 인기를 얻었고, 설명이 굳이 필요하지 않은 분야로 빠르게 도입 및 확산되고 있다.

#### ▷ 3.1.1. Intrinsic

모델을 쉽게 해석할 수 있는 가장 빠른 방법은 애초에 모델을 해석 가능한 구조로 고안하는 것이다. 의사결정나무(Decision Tree)같은 단순한 모델은 구조만 보더라도 사람이 해석하기 쉽다. 구조가 단순한 모델은 그 자체적으로 이미 해석력을 확보하고 있다고 해서  Intrinsic(본래 갖추어진) 이라는 이름이 붙었다. 또는 투명성(Transparency)를 갖췄다고 표현하기도 한다. Intrinsic의 장점은 “모델이 어떻게 동작하는가?”에 대해서 설명할 수 있다는 것이다. 하지만 trade-off 관계 탓에 Intrinsic 모델은 정확도가 낮다.

#### ○ Decision Tree  

<center><img src="https://user-images.githubusercontent.com/50395556/80390404-5601c180-88e7-11ea-8588-84fcb67cde16.png" title="decision tree"></center>

Decision Tree는 모델 그 자체로서 이미 해석력을 확보하고 있는 대표적인 예시이다. 몇몇 요인들의 값을 기준으로 둘 이상의 가지들이 분기하면서 뻗어나가는 형태이므로 예측 결과를 도출하는 과정과 그 해석이 직관적이라는 장점이 있다. 예측 결과에 해당하는 마디를 따라 올라가면서 의사결정의 근거를 쉽게 확인할 수 있다.

#### ○ Falling Rule List

<center><img src="https://user-images.githubusercontent.com/50395556/80390498-7598ea00-88e7-11ea-8ed0-159bfee2e331.png" title="falling rule list"></center>

IF-ELSE IF-ELSE 규칙으로 정의된 Falling Rule List도 Intrinsic 모델로 분류할 수 있다. 모델이 학습하는 순간, 즉시 모델의 의사 결정 근거가 생긴다.

#### ○ Linear Model

Linear Model(선형 모델)도 Intrinsic으로 분류할 수 있다. Linear Model에서는 독립 변수의 계수가 곧 독립 변수의 중요도를 의미한다. 또는 L1 regularization을 적용하면 일부 독립변수의 계수가 0인 Sparse Linear Model(희소 선형 모델)이 되는데, 이 경우 예측 결과에 영향을 미치는 독립 변수만 추릴 수 있으므로 설명력을 갖게 된다.

#### ▷ 3.1.2. Post-hoc

모델 자체가 설명력을 지니지 않을 경우, 모델의 예측 결과는 사후(post-hoc)에 해석할 수 밖에 없다. 기계학습 및 딥러닝 분야에서 해석 가능한 기법이 대부분 Post-hoc에 속한다. 모델의 정확도가 높으면서 설명력을 내재하고 있으면 가장 좋겠지만, 현실적으로 존재하기 힘들다. 성능이 좋은 복잡한 모델을 사용하고 해석은 Post-hoc으로 하는 방법이 보편적이다. Post-hoc 예시는 이후에 소개하는 관점과 함께 설명한다.

### 3.2. Scope

설명하는 범위에 따라서 해석 기법을 분류할 수 있다. 모든 예측 결과에 대해서 항상 설명력을 갖는 전역적인(Global) 기법과 하나 또는 일부 예측 결과만 설명 가능한 국소적인(Local) 기법으로 나뉜다.

#### ▷ 3.2.1. Global

- Global 기법은 모델의 로직과 관련된 이해를 바탕으로, 모델이 예측하는 모든 결과를 설명한다. 또는 모듈 레벨에서 모델의 한 모듈이 예측 결과에 어떻게 영향을 미치는지 설명하는 범위도 포함한다.
- Intrinsic 모델은 모델의 구조로부터 모든 예측 결과에 대한 설명이 가능하므로 태생적으로 Global 기법에 속한다. (Decision Tree, Falling Rule List 등)
- Global은 이상적인 설명 기법이지만 Post-hoc을 Global로 구현하기는 현실적으로 까다롭다. 설명 측면에서, 모든 예측에 대해서는 일정한 설명력을 갖출 수 있더라도, 개별 예측 결과의 특징을 설명하는 능력은 다소 떨어질 수 있다.

#### ▷ 3.2.2. Local

- Local 기법은 특정한 의사 결정 또는 하나의 예측 결과만 설명한다. 몇몇 개의 예측 결과를 묶어 예측 그룹에 대하여 설명하는 범위도 Local 기법에 포함한다. Global 기법에 대비해서 Local 기법은 설명할 범위가 적어서 비교적 실현성 있고 비용이 적게 든다. 또한, 전반적인 예측 성향은 설명하지 못하더라도 하나 또는 소수의 예측 결과는 완벽에 가깝게 설명할 수 있다.
- 현실적으로 매번 예측할 때마다 설명을 요구하지는 않는다. 이슈가 발생한 예측만 설명하는 것이 현실적이다. 설명이 필요할 때, 적어도 해당 이슈에 대해서는 잘 설명할 수 있다는 점에서 Global 기법보다 실용적이다.

#### ○ LIME(Local Interpretable Model-agnostic Explanation)

LIME은 Scope 관점에서는 Local, Dependency 관점에서는 Model-agnostic, Complexity 관점에서는 Post-hoc으로 분류할 수 있다.입력값을 교란(perturb)해 예측이 어떻게 바뀌는지를 확인함으로써 중요한 입력 변수를 찾을 수 있다는 비교적 쉬운 아이디어다. Model에 대해 영향을 받지 않기 때문에 어떤 알고리즘에도 사용할 수 있는 장점이 있다. 또한 모델의 내부구조가 아무리 복잡하더라도 사람이 직관적으로 해석할 수 있다.  

<center><img src="https://user-images.githubusercontent.com/50395556/80390598-98c39980-88e7-11ea-8943-106ca17c608d.png" title="lime1"></center>

LIME을 이용한 이미지 예측 해석 예시를 보자. 원본 이미지를 해석 가능한 요소로 잘게 쪼갠다. 그리고 일부 요소를 off 하여 교란된 인스턴스 데이터셋을 생성한다. 이미지 예시에서는 아래 그림과 같이 요소를 회색으로 칠하는 것으로 off를 표현한다.  

<center><img src="https://user-images.githubusercontent.com/50395556/80390642-abd66980-88e7-11ea-8d10-872d97c8ec59.png" title="lime2"></center>

교란된 인스턴스 데이터셋에 청개구리가 있는지 예측 확률을 구한다. 그런 다음 인스턴스 데이터세트에 가중치를 적용한 단순 선형회귀 모델을 학습한다. 가중치가 높은, 다시 말해 설명하는 양이 큰 인스턴스를 제외한 나머지를 회색을 칠하고 해석 결과로 제시한다.

### 3.3. Dependency

설명 기법이 특정 종류의 모델에만 적용할 수 있도록 특화되었는지, 혹은 모델에 관계 없이 범용적으로 적용할 수 있는지에 따라 분류할 수 있다.

#### ▷ 3.3.1. Model-specific

특정 종류의 모델만 적용할 수 있는 설명 기법을 Model-specific(모델 특정적)이라 한다. Intrinsic 기법은 모델 자체가 가지고 있는 특성을 이용하므로 타 모델에서 적용할 수 없는 전형적인 Model-specific이다. 특정 모델만 적용할 수 있다는 점에서 모델을 고를 때 선택권이 줄어드는 단점이 있다. CNN 계열에서만 쓸 수 있는 시각화 해석 기법은 모두 Model-specific에 해당한다.

#### ○ Activation

CNN 계열의 모델에서 활성 함수를 거쳐 활성화(Activation)된 feature map 또는 weight 자체를 시각화함으로써 모델을 설명하려는 시도이다. 특정 예측 결과를 설명하는 것이 아니라 Convolution Layer의 weight를 통해서 전역적인 예측 결과를 설명하고자 하므로 Model-specific인 동시에 Global 기법이며, 모델이 만들어진 후에 설명하므로 Post-hoc 기법이기도 하다.

#### ○ Weight Visualization

<center><img src="https://user-images.githubusercontent.com/50395556/80390729-cb6d9200-88e7-11ea-980c-3da8dc2736d6.png" title="weight visualization"></center>

Convolution layer가 보유한 weight(또는 filter)는 2차원 구조를 하고 있으므로 이미지처럼 그 자체로 시각화할 수 있는 특징이 있다. 그러나 위의 그림처럼 각 weight가 어떤 시각적 특징을 추출하는지 사람이 직관적으로 이해하기 어렵다. 학습 중인 CNN 모델의 상태를 점검하는 용도로 사용되는 경우가 많고, 그 이상의 설명을 얻기는 현실적으로 어렵다.

#### ○ Maximization Optimization

<center><img src="https://user-images.githubusercontent.com/50395556/80390816-eb9d5100-88e7-11ea-8fa9-5a32a6e5610a.png" title="maximization optimiation"></center>

활성 함수를 최대한으로 활성화하는 최적의 이미지를 생성함으로써 feature map이 갖는 의미를 파악하는 방법이다. 우선 랜덤 노이즈 이미지(Random Noise Image)를 입력값($X_0$)으로 하고 특정 feature map($f$)을 타겟 출력값으로 한다. 타겟 출력값($f$)을 입력값($X_0$)으로 미분하면 feature map($f$)를 활성화시키는 값($\frac{df}{dX_0}$)을 구할 수 있다. 랜덤 노이즈 이미지($X_0$)에 $\frac{df}{dX_0}$를 더해주면 $X_0$보다 feature map($f$)을 더 많이 활성화하는 $X_T$ 이미지를 얻을 수 있다. 이를 $X_1$이라고 하자. $f$를 활성화하는 값을 구하고 랜덤 노이즈 이미지에 다시 더해주는 과정을 충분한 횟수($T$)만큼 반복하면 를 최대로 활성화시키는  이미지를 얻을 수 있다. 이 이미지는 feature map()을 생성하는 filter가 추출하는 시각적 특징에 대한 정보를 담고 있다. Weight visualization보다 사람이 이해하기 쉬운 형태로 시각화되며 상위 레이어로 갈수록 단순한 패턴에서 복잡한 패턴으로 바뀌는 것을 확인할 수 있다. 또 feature map뿐만 아니라 layer 또는 classifier의 logit을 타겟 출력값으로 삼는 것도 가능하다.

<center><img src="https://user-images.githubusercontent.com/50395556/80390885-0243a800-88e8-11ea-9047-4554a54be917.png" title="maximization optimiation2"></center>

#### ○ Attribution

- CNN의 중간 출력값보다는 이미지가 주어졌을 때 해당 예측 결과를 설명하는 데에 더 집중하는 Local 기법에 속한다. 예를 들어 분류 문제에서 입력값(이미지)을 자전거라고 출력했다면, CNN이 자전거라고 예측하는데 기여(Attribution)한 element(원소)를 찾는다. 이미지 위에 해당 element의 위치를 표시하여 모델이 어디를 보고 자전거라고 예측했는지 직관적으로 이해할 수 있는 시각화 자료가 된다. 이미지에서 집중되는 부분이 두드러지게 표현하는 것을 Saliency map(현저성 맵)이라고 한다.

#### ○ Deconvolution

<center><img src="https://user-images.githubusercontent.com/50395556/80390962-1ab3c280-88e8-11ea-8c30-b9fe3e980645.png" title="deconvolution"></center>

Convolution network를 그대로 반대 방향으로 만들어 이어 붙임으로써 AI의 판단 근거를 역추적하는 개념이다. Convolution 연산을 반대로 수행할 때에 deconvolution 계산을 이용하였다. 단, pooling은 역으로 계산할 수 없기 때문에 pooling 하기 전 위치 정보를 저장해두었다가 다시 사용한다. 위 그림은 Deconvnet의 구조를 표현한 것으로, VGG16 모델을 기반으로 하였다.  

<center><img src="https://user-images.githubusercontent.com/50395556/80391039-2c956580-88e8-11ea-85c9-ee22de611c2a.png" title="deconvolution2"></center>

(a)는 원본 이미지이다. (b)는 마지막 14x14 deconv layer, (c)는 28x28 unpooling layer에서의 activation 결과이다. 이후 (d) ~ (j)는 계속해서 deconv layer와 unpooling layer를 통과한 결과이다. 레이어를 통과할수록 더욱 세밀해지고 있다. 최종적으로 (i)에 이르러서는 자전거의 형상을 거의 완벽하게 묘사한다.  

#### ○ CAM(Class Activation Map)

<center><img src="https://user-images.githubusercontent.com/50395556/80391109-459e1680-88e8-11ea-931e-948edfdc9fb9.png" title="cam"></center>

CAM은 이미지 분류 모델에서 이미지의 어느 부분을 보고 class를 예측했는지를 시각한다. 기존의 CNN 계열 이미지 분류 모델은 마지막 feature map을 flatten해서 FC(Fully Connected) layer로 변환한 후 classification을 학습한다. 그런데 flatten하게 되면 기존의 feature map이 가지고 있던 공간 정보를 잃게 된다. 그래서 CAM은 flatten 대신에 Global Average Pooling을 사용한다. 마지막 feature map의 각 채널은 1개의 값으로 변환되며 각각의 weight가 곱해져서 output layer에 입력된다. 이 때의 weight는 feature map의 채널에 대한 weight라고 볼 수 있다. 마지막 feature map을 꺼내와서 채널별로 대응하는 weight를 곱한 후 모두 더해주면 모델이 어느 부분을 보고 class를 분류 했는지 알 수 있다.

#### ○ Grad-CAM(Gradient Class Activation Map)

Grad-Cam은 CAM의 단점을 보완하고 발전한 알고리즘이다. CAM은 FC layer 대신에 GAP를 써야만 했기 때문에 모델의 구조를 바꿔야만 하는 단점이 있었다. Grad-CAM은 GAP로 FC layer를 만들고 feature map channel에 대한 weight를 직접 구하는 대신에,  모델 구조는 그대로 두고 feature map channel에 대한 gradient를 구해서 channel별 가중치를 구한다.  

<center><img src="https://user-images.githubusercontent.com/50395556/80391221-68302f80-88e8-11ea-9e32-8a65ae2422e5.png" title="grad cam equation1"></center>

<center><img src="https://user-images.githubusercontent.com/50395556/80391277-78480f00-88e8-11ea-8142-e503598e4a98.png" title="grad cam equation2"></center>

(1) 마지막 convolution layer를 통과한 feature map을 $A^{k} \in R^{\mu \times \upsilon}$ (A:feature map, k:feature, u:width, v:height) 이라고 하고 summation으로 GAP한 것과 같은 값을 구할 수 있다. 모델이 분류했을 때 예측한 확률 값을 y로 하고 feature map $A^k$ 에 대하여 미분하면 $A^k$의 가중치를 알 수 있다.  

(2) 가중치가 음수일 경우는 모델이 최종 예측한 class가 아닌 다른 class라고 예측하는데 중요한 정보임을 의미한다. 다른 class를 예측하는데 중요한 정보에는 관심이 없으므로 기울기가 양수일 때만 시각화할 수 있도록 ReLU를 이용해서 Class Activation Map을 완성한다.

<center><img src="https://user-images.githubusercontent.com/50395556/80391323-872ec180-88e8-11ea-9693-e0060ebea18d.png" title="grad cam"></center>

위는 Visual Question Answering에 Grad-CAM을 적용한 결과이다. CAM과 마찬가지로 모델이 예측할때 집중한 부분을 heatmap 형식으로 시각화한다.

#### ▷ 3.3.2. Model-agnostic

모델의 내부는 사람이 알 수 없으므로, 모델을 설명하기 위해서는 모델 밖에서 근거를 찾아야 한다는 Model-agnostic(모델 불가지론)은 모델의 어떠한 특성도 이용하지 않는다. 따라서 모델에 상관없이 적용 가능한 특징이 있다.

#### ● Surrogate

- 설명 불가능한 모델 대신에 설명 가능한 대리(Surrogate) 모델을 따로 만들어서 설명하는 방법이다. 보통 Surrogate 모델로는 설명이 가능한 Intrinsic 모델을 사용한다.
- 예를 들면 딥러닝 모델을 설명하기 위해서 의사결정나무(Decision Tree)를 사용하는 셈이다. 대리모델은 Original(원본) 모델의 예측을 정답으로 삼아 지도 학습(Supervised Learing)한다. 이로써 Surrogate 모델은 Original 모델에 대한 설명력을 갖는다. 이때 Surrogate 모델은 scope 관점에서 Global 기법에 속한다.
- 앞서 언급했던 LIME도 Local 범위에서의 Surrogate 모델로 볼 수 있다. 주어진 이미지에 대해서 독립변수를 생성하고, 독립변수의 중요도를 설명하는 하나의 모델이기 때문이다.
- Surrogate 모델은 Original 모델을 완벽하게 설명할 수 없는 단점이 있다. 그럼에도 불구, Original 모델에 어떠한 변형도 가하지 않고 높은 정확도를 유지할 수 있다는 점과 간단하게 설명력을 얻을 수 있다는 점에서 많이 사용되는 기법이다.

#### ● Sensitivity Analysis

- 입력 변수를 의도적으로 교란하거나 데이터를 변형 시뮬레이션했을 때 모델의 예측이 안정적으로 유지되는지를 확인하는 기법이다. 입력 변수의 변화에 따른 예측의 변화를 측정하여 변수의 중요도를 해석한다. 이때 입력 변수의 변화는 변수를 제외하거나 추가하는 식의 부가적 변형이다. 유의할 점은, Sensitivity Analysis의 목적은 입력 변수와 예측 간의 함수 관계를 설명하려는 것이 아니라는 점이다. 변수의 중요도를 판별하여 모델이 예측하는데 중요한 변수를 찾아 해석의 인사이트를 얻거나, 중요하지 않은 변수를 제거하는데 사용한다.
- 대표적인 기법으로는 앞서 설명했었던 LIME이 있다. LIME은 원본 이미지를 잘게 쪼개 여러 개의 변수로 만든 다음 입력값을 교란했다. 입력값 교란에 따른 예측 결과의 변화가 가장 큰 변수를 중요도가 높은 변수로 해석한다.

#### ● Partial Dependence

- 중요한 변수를 찾는 것은 주요 동인을 발견하는 데에는 도움이 되지만, 입력 변수와 예측 간의 관계를 설명하지는 않는다. 부분 의존성(Partial Dependence)은 소수의 입력 변수와 예측 사이의 함수 관계를 도출한다. 비 부가적인 방식으로, 입력 변수의 범위 조절에 따른 예측의 변화를 측정하여 모델의 예측이 입력 변수에 어떻게 의존하는지 파악한다.

#### ○ PDP(Partial Dependence Plot)

PDP는 모델의 예측이 단일 입력에 어떻게 의존하는지를 보여주는 1-way 그래프이다. 예측이 관심 입력 변수의 값에 부분적으로 영향을 받는 것인지 나타낸다. PDP를 그리는 과정은 간단하다. 관심변수를 제외한 관측치의 모든 변수의 값을 고정하고 특정 범위 내에서 관심변수를 변화시켜가며 예측값을 구한다. 임의의 관심변수 값 1개에 대하여 관측치 개수만큼의 예측을 얻게 되는데, 이를 평균 내어 출력한다.  

<center><img src="https://user-images.githubusercontent.com/50395556/80391689-091eea80-88e9-11ea-903d-b2779aa88cea.png" title="pdp" width="800px"></center>

PDP는 계단 함수, 곡선, 선형 등과 같은 관계 유형을 보여준다. 위는 Boston Housing Dataset을 PDP 그린 예시 그래프이다. RM(주택 1가구당 평균 방의 개수)을 보면 방 개수가 늘어날수록 집 값이 올라가는 경향이 있다는 것을 알 수 있다. LSTAT(모집단 내의 하위계층 비율)을 보면 하위계층 비율과 집 값은 음의 계단함수 관계임을 알 수 있다.단, PDP는 관심 변수가 다른 변수와 상호작용하지 않을 때에만 유효하다.

#### ○ ICE(Individual Conditional Expectation)

PDP는 모델의 작동원리에 대한 대략적인 뷰를 제공하는 Global 기법인 반면 ICE는 개별 관측의 수준까지 상세하게 탐색할 수 있으므로 Local 기법의 특성도 갖는다. 평균값을 내는 PDP를 분리하므로 모델 변수들 간의 상호작용을 식별하는데도 도움이 된다.  

<center><img src="https://user-images.githubusercontent.com/50395556/80391912-5d29cf00-88e9-11ea-9d0c-cf26bed48c1e.png" title="ice" width="800px"></center>

왼쪽 그래프는 PDP이다. 평평하기 때문에 X1은 예측 결과와 관계가 없는 것으로 보인다. 그러나 오른쪽 ICE 그래프를 보면 X1이 실제로는 예측 결과와 관련이 있음을 알 수 있다. 다만 평균을 구했기 때문에 PDP가 평평하게 보였던 것이다.  ICE는 많은 입력 변수 사이에서 강한 관계가 있을 때 특히 유용하다.

#### ● Example Based

- 예시 기반(Example Based) 설명은 데이터셋 중에서 특정한 예시를 뽑아 모델을 설명하는 기법이다. 직접적으로 예시를 들어서 설명하므로 사람이 직관적으로 이해하기 쉬운 장점이 있다. 모델 전체를 설명하는 Global 기법이기도 하다.

#### ○ MMD-critic(Maximum Mean Discrepancy-critic)

최대 평균 불일치(Maximum Mean Discrepancy, MMD)-critic은 비지도학습 알고리즘으로 prototype과 criticism을 자동으로 찾는 기법이다. Prototype은 데이터셋의 범주를 가장 잘 설명하는 대표적인 예시이다. 그러나 prototype은 지나치게 일반화를 적용할 우려가 있다. 이를 방지하기 위해서 prototype으로 설명할 수 없는 관측치를 선별해 criticism으로 사용한다. 즉, criticism은 prototype으로 대표되는 범주와 같은 카테고리에 속하지만, prototype이 대표하지 못하는 예외 데이터이다. Example based 기법은 prototype과 criticism을 이용해서 관측치의 주요 특징을 파악하고 이를 통해 모델을 설명한다.다음 예시를 통해 더 알아보자.

<center><img src="https://user-images.githubusercontent.com/50395556/80391956-703c9f00-88e9-11ea-868a-bca061b7be19.png" title="mmd critic1"></center>

위 그림을 보면 총 4개의 범주가 있다. 범주로 묶인 관측치 중에서 범주에 대한 대표성을 띠는 관측치를 prototype으로 선별한다. 주의할 점은 prototype은 범주를 일반화하는 임의의 관측치를 생성한 것이 아니라, 실제 존재하는 관측치 중에서 선택하는 것이다.

<center><img src="https://user-images.githubusercontent.com/50395556/80391980-7cc0f780-88e9-11ea-8e2c-722199734bdd.png" title="mmd critic2"></center>

범주 내에서 protoype으로 일반화할 수 없는 예외 관측치를 criticism으로 선별한다. protoype과 마찬가지로 실제 존재하는 관측치 중에서 선택한다.

## 4. 설명에 대한 평가

XAI가 설명을 적절하게 했는지 성능에 대한 평가하기는 어렵다. 좋은 설명이라는 것은 주관적인 특징이 있기 때문이다. 설명의 성능을 평가하는 것이 모호한 분야인 탓인지 XAI에 대한 전체 연구 중에서도 성능 평가에 관한 연구는 고작 5%에 불과하다. 하지만 아무리 주관적인 영역이라 할지라도 성능 평가를 간과해서는 안 된다. 사람이 주관적으로 “설명이 좋다”라고 느끼는 기본 특징에 대해서 심리, 철학, 인지 과학적인 관점에서 연구한 내용을 소개한다. 더불어 설명 성능을 정량적으로 평가하려는 시도도 함께 소개한다.

#### ▷ 인간 친화적인 설명의 특징

- **Contrastive(대조적)** : 사람은 A라는 예측에 대하여 집중하여 궁금하기보다는 왜 B 대신에 A라고 예측했는지 궁금해하는 성향이 있다. 따라서 다른 예측 결과인 B와 A를 비교하며 예측 결과 차이를 만들어낸 핵심 요인을 설명하는 것이 좋다.
- **Selective(선택적)** : 사람은 어느 결과를 발생시킨 요인을 모든 곳에서 찾으려고 하지 않으며, 자신에게 친숙하거나 잘 알고있는 영역 내에서 한 두가지의 핵심 요인을 찾으려는 성향이 있다. 따라서 가능한 모든 이유를 설명하기보다는 제공하는 설명의 양을 한 두개 정도로 한정하는 것이 좋다.
- **Social(사회적)** : 설명은 다른 사람에게 지식을 전달하는 대화의 일종이다. 사람은 청자의 수준에 맞는 용어, 청자가 가진 배경지식 등을 고려하여 설명할 때 좋은 설명이라고 느낀다. 개발자의 관점에서 벗어나 AI 서비스르 이용하는 유저가 이해할 수 있도록 설명을 설계하려는 노력이 필요하다.

#### ▷ 설명에 대한 정량적 평가 방법

- **Application-grounded** : 애플리케이션을 사용하는 최종 사용자(End-user)가 평가하도록 하는 방법이다. 대부분의 경우 도메인 전문가를 대상으로 한다. End-task의 맥락에서 설명의 질을 평가한다.
- **Human-grounded** : Application-grounded 방법을 조금 간단하게 하는 방법이다. 전문가 보다는 일반인이 평가하도록 한다. 설명의 질을 일반적인 관점에서 평가하기에 좋다.
- **Functionally-grounded** : 사람이 개입하지 않는 평가 방법이다. Human-grounded 방법을 통해서 이미 유의미한 평가 지표를 도출한 바 있을 때, 해당 지표를 기준으로 모델을 평가한다.

## 5. 활용 방안

#### ▷ 중요작업에 활용

범죄 위험성 판단, 인사 평가, 군사 작전, 의료 분야 등 의사 결정이 중대한 영향을 미치는 분야에서는 아직 AI를 적용되지 못한 분야이다. XAI로 의사 결정 이유를 설명할 수 있게 된다면 AI에게 중대한 의사결정을 맡길 수 있을 것이다.  
법적, 윤리적 이슈가 발생할 소지가 있는 분야에서도 XAI의 역할이 중요할 것이다. 예를 들어 자율주행자동차의 교통사고, 수술 AI의 의료사고, AI 군사 드론의 민간인 공격 사고는 책임의 소재는 여전히 뜨거운 논쟁거리이다. XAI가 논란을 종결지을 수는 없겠지만 사회적 합의에 이르기 위해서는 반드시 짚고 가야 할 문제일 것이다.

#### ▷ 연구에 활용

<center><img src="https://user-images.githubusercontent.com/50395556/80392023-8e0a0400-88e9-11ea-8c7a-a8d1e4a65b5f.png" title="cbam"></center>

AI를 해석할 수 없어서 답답한 사람은 사용자뿐만이 아니다. XAI는 AI 연구에도 활용할 수 있다. Black Box 내부를 들여다보고 AI의 예측 결과를 설명할 수 있다는 말은 모델의 성능이 낮을 때는 왜 낮은지, 성능이 높을 때는 왜 높은지를 설명할 수 있다는 말과 같기 때문이다. 위 사진은 AI 알고리즘 연구개발 단계에서 기존 알고리즘 대비 성능 개선 정도를 확인하는 용도로 XAI가 사용된 사례이다.

## 이미지 출처 및 참고
[1]  [Amina Adadi and Mohammed Berrada, Peeking Inside the Black-Box: A Survey on Explainable Artificial Intelligence (XAI), 10.1109/ACCESS.2018.2870052, IEEE, 2018](https://ieeexplore.ieee.org/document/8466590)  
[2]  [Diogo V. Carvalho, Eduardo M. Pereira 1, aime S. Cardoso. Machine Learning Interpretability: A Survey on Methods and Metrics., 2019](https://doi.org/10.3390/electronics8080832)  
[3]  [Riccardo Guidotti, Anna Monreale, Salvatore Ruggieri, Franco Turini, Dino Pedreschi, Fosca Giannotti. A Survey of Methods for Explaining Black Box Models, arXiv:1802.01933, 2018](https://arxiv.org/abs/1802.01933)  
[4]  [Fulton Wang, Cynthia Rudin. Falling Rule Lists, arXiv:1411.5899, 2014](https://arxiv.org/abs/1411.5899)  
[5]  [Hyeonwoo Noh, Seunghoon Hong, Bohyung Han. Learning Deconvolution Network for Semantic Segmentation, arXiv:1505.04366, 2015](https://arxiv.org/abs/1505.04366)  
[6]  [Bolei Zhou, Aditya Khosla, Agata Lapedriza, Aude Oliva, Antonio Torralba. Learning Deep Features for Discriminative Localization, arXiv:1512.04150, 2015](https://arxiv.org/abs/1512.04150)  
[7]  [Ramprasaath R. Selvaraju, Michael Cogswell, Abhishek Das, Ramakrishna Vedantam, Devi Parikh, Dhruv Batra. Grad-CAM: Visual Explanations from Deep Networks via Gradient-based Localization, arXiv:1610.02391, 2016](https://arxiv.org/abs/1610.02391)  
[8]  [Sanghyun Woo, Jongchan Park, Joon-Young Lee, In So Kweon. CBAM: Convolutional Block Attention Module, arXiv:1807.06521, 2018](https://arxiv.org/abs/1807.06521)  
[9]  한국교육학술정보원, 2019 학술정보 글로벌 동향, Vol. 10. IT 기반 분야, 2019  
[10]  금융보안원, 설명 가능한 인공지능(eXplainable AI, XAI) 소개, 2018  
[11]  oreilly, [Local Interpretable Model-Agnostic Explanations (LIME): An Introduction](https://www.oreilly.com/learning/introduction-to-local-interpretable-model-agnostic-explanations-lime)  
[12]  oreilly, [Ideas on interpreting machine learning](https://www.oreilly.com/radar/ideas-on-interpreting-machine-learning/)  
[13]  SAS Blog Insight, [머신러닝, 데이터 세트를 이해하고 해석하는 방법](https://www.sas.com/ko_kr/solutions/ai-mic/blog/machine-learning-data-set.html)  
[14]  SAS Korea 블로그, [머신러닝 해석력 시리즈 3탄: 부분의존성(PD) & 개별조건부기대치(ICE) 플롯 정복하기!](https://blogsaskorea.com/116)  
[15]  SUALAB Research Blog, [Interpretable Machine Learning 개요: (1) 머신러닝 모델에 대한 해석력 확보를 위한 방법](http://research.sualab.com/introduction/2019/08/30/interpretable-machine-learning-overview-1.html)  
[16]  SUALAB Research Blog, [Interpretable Machine Learning 개요: (2) 이미지 인식 문제에서의 딥러닝 모델의 주요 해석 방법](https://research.sualab.com/introduction/2019/10/23/interpretable-machine-learning-overview-2.html)
