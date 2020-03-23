---
layout: post
title: "딥러닝, 머신러닝 유사한 용어 정리(동의어 사전)"
tags: [DeepLearning]
author: jinhyo
feature-img: /assets/img/pexels/triangular.jpeg 
use_math: false
keywords: [동의어, 용어, terminology]
---
**주의!**

- 인공지능을 공부하다보면 서로 의미는 비슷한데 표현이 달라서 헷갈리는 용어가 많다. 이 표는 초보자를 위한 참고 자료이다.
- 동의어라고 판단한 근거는 작성자의 주관적인 기준이다.
- 용어가 혼용되는 상황이라 구분이 모호하지만, 이해를 돕기 위하여 가능한 용어의 출처를 나누려고 했다.
- 분야별로, 알고리즘별로 엄밀한 정의가 다르므로 100% 같은 의미는 아닐 수 있다.

## 동의어 표

[표 크게 보기](https://www.notion.so/84ba1f220571411985c15868242d4736?v=cd5b28a4103c4669b480d0911b15ae1e)  

<style>
.scrolltable {
    width: 1000px; height:600px;
    display: block;
    overflow: auto;
}
</style>
<table class="scrolltable" border='1px' cellspacing='0'>
    <thead>
        <tr>
        <th>설명</th>
        <th>기계학습</th>
        <th>통계</th>
        <th>Data Science</th>
        <th>?</th>
        <th>정형데이터</th>
        <th>CNN</th>
        </tr>
    </thead>
    <tbody>
        <tr>
        <td>모든 데이터의 집합</td>
        <td></td>
        <td></td>
        <td><strong>dataset</strong>데이터셋<sup><a href="#footnote_1">1</a></sup></td>
        <td></td>
        <td><strong>table</strong>표<sup><a href="#footnote_4">4</a></sup></td>
        <td></td>
        </tr>
        <tr>
        <td>데이터 1개 (정답을 포함하기도 함)</td>
        <td></td>
        <td><strong>sample</strong>표본</td>
        <td><strong>Instance</strong>인스턴스</td>
        <td><strong>data point</strong>데이터 포인트 <br> <strong>example</strong>예 <br> <strong>case</strong>사례 <br> <strong>record</strong>기록 <br> <strong>row</strong>행</td>
        <td></td>
        <td></td>
        </tr>
        <tr>
        <td>데이터 1개를 구성하는 개별 값</td>
        <td></td>
        <td><strong>observation</strong>관측치</td>
        <td></td>
        <td></td>
        <td><strong>value</strong>값</td>
        <td></td>
        </tr>
        <tr>
        <td>정답을 제외한 데이터의 속성을 지칭함</td>
        <td><strong>feature</strong>특징</td>
        <td><strong>independent variable</strong>독립변수</td>
        <td><strong>attribute</strong>속성</td>
        <td><strong>input</strong>입력 <br> <strong>dimension</strong>차원 <br> <strong>feature column</strong>특성 열</td>
        <td></td>
        <td></td>
        </tr>
        <tr>
        <td>모델이 맞추려는 목표</td>
        <td><strong>label</strong><sup><a href="#footnote_2">2</a></sup> <br> <strong>class</strong>클래스<sup><a href="#footnote_2">2</a></sup> : 분류문제에서 <br> <strong>target</strong>목표<sup><a href="#footnote_2">2</a></sup></td>
        <td><strong>dependent variable</strong>종속변수</td>
        <td></td>
        <td></td>
        <td><strong>target column</strong>타겟 열</td>
        <td></td>
        </tr>
        <tr>
        <td>모델이 데이터를 학습하는 과정 <br> / 기계가 파라미터를 찾는 과정</td>
        <td><strong>train</strong>학습</td>
        <td><strong>fit</strong>적합</td>
        <td></td>
        <td><strong>parameter update</strong>파라미터 갱신 <br> <strong>weight update</strong>가중치 갱신</td>
        <td></td>
        <td></td>
        </tr>
        <tr>
        <td>학습한 모델에 입력값을 주고 출력값을 얻는 행위</td>
        <td><strong>predict</strong>예측</td>
        <td></td>
        <td></td>
        <td><strong>inference</strong>추론</td>
        <td></td>
        <td></td>
        </tr>
        <tr>
        <td>학습한 모델에 입력값을 주었을 때 얻은 출력값</td>
        <td><strong>prediction</strong>예측결과</td>
        <td></td>
        <td></td>
        <td><strong>output</strong>출력</td>
        <td></td>
        <td></td>
        </tr>
        <tr>
        <td>모델이 학습으로 얻은 결과</td>
        <td><strong>parameter</strong>매개변수</td>
        <td></td>
        <td></td>
        <td><strong>weight</strong>가중치 : 선형모델에서, 딥러닝에서</td>
        <td></td>
        <td><strong>filter</strong>필터<sup><a href="#footnote_9">9</a></sup> <br> <strong>kernel</strong>커널<sup><a href="#footnote_9">9</a></sup></td>
        </tr>
        <tr>
        <td>실제값과 예측값의 차이</td>
        <td></td>
        <td></td>
        <td></td>
        <td><strong>loss</strong>손실 <br> <strong>cost</strong>비용 <br> <strong>error</strong>오차</td>
        <td></td>
        <td></td>
        </tr>
        <tr>
        <td>실제값과 예측값의 차이를 구하는 함수</td>
        <td></td>
        <td></td>
        <td></td>
        <td><strong>loss function</strong>손실 함수 <br> <strong>cost function</strong>비용 함수 <br> <strong>error function</strong>오차 함수 <br> <strong>objective function</strong>목적 함수<sup><a href="#footnote_8">8</a></sup></td>
        <td></td>
        <td></td>
        </tr>
        <tr>
        <td>다차원 데이터의 차원</td>
        <td></td>
        <td></td>
        <td></td>
        <td><strong>dimension</strong>차원<sup><a href="#footnote_3">3</a></sup></td>
        <td></td>
        <td><strong>feature map</strong>피쳐 맵<sup><a href="#footnote_3">3</a></sup></td>
        </tr>
        <tr>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td><strong>fc layer</strong><sup><a href="#footnote_7">7</a></sup> <br> <strong>fully connected layer</strong>완전 연결층 <br> <strong>dense layer</strong>밀집층<sup><a href="#footnote_5">5</a></sup> <br> <strong>perceptron</strong>퍼셉트론<sup><a href="#footnote_10">10</a></sup> <br> <strong>Linear</strong><sup><a href="#footnote_11">11</a></sup> <br> <strong>head</strong>헤드<sup><a href="#footnote_6">6</a></sup></td>
        <td></td>
        <td></td>
        </tr>
    </tbody>
</table>

### 부연 설명

- <a name="footnote_1">1</a> : Dataset은 Instance의 묶음이며, Instance는 Attributes의 묶음이다.
- <a name="footnote_2">2</a> : label / class / target 의 차이
    - **label**라벨 : 데이터에 붙은 정답
    - **class**클래스 : **classification**분류문제 에서, categorical label 집합에 속하는 값
    - **target**목표 : 맞추려는 대상
- <a name="footnote_3">3</a> : feature map == dimension
    - filter를 이용해서 [convolution](https://developers.google.com/machine-learning/glossary?hl=ko#컨볼루션convolution) 연산한 결과를 feature map이라고 한다. filter 1개당 feature map이 1개 얻어진다. Convolution layer는 filter 여러개로 여러개의 feature map을 얻어서 1개의 다차원 배열로 만든다. 이 때, 각 차원은 별개의 feature map이다. Conv Layer의 output 차원 크기는 filter 수와 동일하다.
- <a name="footnote_4">4</a> : 정형데이터는 2차원으로 구성되어 있으며, 모든 데이터는 column과 row로 구성된 **table**표에 들어간다. pandas를 분석 툴로 많이 사용한다.
- <a name="footnote_5">5</a> : fc 레이어를 그림으로 그리면 노드를 연결하는 선이 빽빽하게(dense) 그려지는 모습을 비유적으로 표현하였다. Keras에서는 이 레이어를 Dense라는 이름으로 구현했다.
- <a name="footnote_6">6</a> : 전통적인 CNN 모델은 보통 feature를 추출하는 Convolution layer를 여러층 쌓고, 마지막에 classification 용도로 fc layer를 쌓는 형태였다. 레이어가 차곡차곡 쌓이는 모양을 그림으로 그리면 맨 위(=head)에 fc layer 놓이게 되는데, 이 마지막 레이어를 비유적으로 head라고 표현한다. 대표적으로 vgg 모델이 그러하다.
- <a name="footnote_7">7</a> : fc는 fully connected의 줄임말이다.
- <a name="footnote_8">8</a> : 기계학습의 목적은 오차함수를 최소화하는 해를 구하는 것과 같으므로 이 함수를 목적 함수라고 부르기도 한다.
- <a name="footnote_9">9</a> : filter == kernel ==weights == parameter
    - CNN에서는 업데이트해야할 대상이 필터, 커널이므로 DNN의 weights와 parameter와 같은 의미로 해석할 수 있다.
- <a name="footnote_10">10</a> : perceptron
- <a name="footnote_11">11</a> : **activation function**활성 함수를 거치지 않은 fc layer를 수식으로 표현하면 **Linear Regression**선형 회기와 모양이 같다. pytorch에서는 fc layer를 Linear라는 이름으로 구현해놓아서 동의어로 포함시켰다.

### 참고한 사이트
[머신러닝 용어: Example, Sample & Data Point](http://taewan.kim/post/sample_example/)  
[데이터 과학 : 더 나은 의사결정을 위한 통찰의 도구](https://books.google.co.kr/books?id=ne61DwAAQBAJ&pg=PT63#v=onepage&q&f=false)  
[머신러닝 용어집 | Google Developers](https://developers.google.com/machine-learning/glossary?hl=ko)  