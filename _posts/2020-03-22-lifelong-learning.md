---
layout: post
title: "평생학습(Lifelong Learning)에 대한 survey"
tags: [DeepLearning]
author: jinhyo
feature-img: /assets/img/pexels/triangular.jpeg 
use_math: true
keywords: __new__, __init__, __call__
---

## 등장배경

AI 연구자들은 인간의 인지 메커니즘을 모방하여 AI의 성능을 높이고자 노력해왔다. 그런 노력 중 하나가 Transfer Learning(전이 학습)이다. 인간은 과거에 학습했던 내용을 기반으로 새로운 내용을 빠르게 습득한다. AI도 과거에 학습했던 내용을 기반으로 비슷한 Task(과제)를 잘 학습할 수 있을까? 라는 아이디어에서 출발한 Transfer Learning은 잘 학습된 모델의 pre-trained weights(사전훈련 가중치)를 새로운 모델의 학습에 활용함으로써 학습 속도가 빠르고 성능이 우수한 모델을 만드는 기법이다. 실제로 강아지를 구분하도록 학습한 AI는 고양이도 잘 구분할 수 있었다. 하지만 새롭게 학습된 모델은, 과거 학습 내용을 잊어버리는 치명적인 문제가 발견된다.

### ▷ Catastrophic forgetting(파괴적 망각)

- 현재 Neural Network(인공신경망)은 Single task(단일 과제)에 대해서는 뛰어난 성능을 보이지만, 다른 종류의 task를 학습하면 이전에 학습했던 task에 대한 성능이 현저하게 떨어지는 문제가 있다. 이를 Catastrophic forgetting이라고 한다. 이 현상은 이전 학습 dataset과 새로운 학습 dataset 사이에 연관성이 있더라도 이전 dataset에 대한 정보를 대량으로 손실한다.

### ▷ Semantic drift(의미 변화)

- 새로 학습하는 과정에서 pre-trained weight가 과도하게 조정될 경우를 Neural Network의 Node(노드)나 weight의 의미가 변했다고 해석할 수 있다. 예를 들어 어떤 Node가 강아지의 귀에 대한 정보를 담고 있었는데, 고양이를 학습한 후 Node가 발바닥에 대한 정보를 처리하도록 바뀌는 것이다. 이를 Semantic drift라고 하며 Catastrophic forgetting과 밀접한 관계가 있다.

## 평생학습(Lifelong Learning)

인간의 뇌는 배경지식을 바탕으로 새로운 것을 배우며, 새로운 것을 배운다고 하여 과거의 지식을 잊어버리지 않는다. Lifelong Learning은 인간의 인지를 모방하여 Catastrophic forgetting과 Semantic drift를 해결하고자하는 메커니즘이다. Lifelong Learning은 다음과 같은 2가지 특징이 있다.

#### ▷ Multi Task Learning(다중 과제 학습)

- Multi task learning은 단일 모델이 여러개의 task를 동시에 학습하는 방법론이다. 서로 다른 task A와 task B가 있다고 가정하자. Task A를 학습하여 모델 A를 만들었는데 task B가 추가되었다고 모델 B를 새로 학습하는 것은 비효율적이다. 우리는 100개의 task를 수행하기 위해서 Single task를 수행하는 모델 100개를 만들 수 없다. 모델 A가 task A와 task B를 모두 수행하도록 학습하는 것이 Multi task learning의 목적이다.

#### ▷ Online Learning(온라인 학습)

- 인간은 같은 내용만 반복해서 학습하지 않는다. 평생에 걸쳐 시시각각 새로운 자극을 받아들이며 그로부터 매순간 학습하고 외부 변화에 적응한다. Online Learning은 연속적으로 데이터를 받고 한번 학습에 사용된 데이터는 버리는 학습법이다. 즉, Single task를 순차적으로 학습해서 Multi task를 수행할 수 있어야하지만,  Multi task를 수행하기 위해서 전체 dataset으로 재학습할 수 없다. 이때 모델이 task A와 task B를 순차적으로 학습하는 과정에서 발생하는 Catastrophic forgetting 문제를 극복해야 한다.

Multi task learning과 Online Learning을 조합해보자면, Lifelong learning은 순차적으로 들어오는 task(1, 2, … , t-1, t)를 학습하고, t 시점에 모든 task(1, 2, … , t-1, t)에 대하여 성능이 좋은 모델을 만들고자 한다. Lifelong Learning은 연속적으로 학습한다는 점에서 Continual Learning(연속 학습)이라고 부르기도 하며 크게 3가지 접근방법으로 연구가 이루어지고 있다. 다음 장에서 각 접근방법과 대표 알고리즘을 함께 설명한다.

## 연구 동향

### 1. Regularization

Neural Network의 weight을 예전 task의 성능에 기여한 중요도에 따라 weight update(가중치 갱신)를 제한하는 접근법이다. 예전 task의 성능에 중요한 weight일수록  Semantic drift가 발생하지 않도록 하여 Multi task가 가능해진다. Regularization 접근 방법의 대표적인 알고리즘은 Google Deepmind가 발표한 Elastic Weight Consolidation이다.

#### ▷ EWC (Elastic Weight Consolidation)

- Task A를 학습한 정보에서 어떤 weight가 task A의 성능에 중요한지 파악하기 위해서 확률적인 관점에서 접근한다. Task A의 dataset이 주어졌을 때 $\theta$가 나올 조건부확률은 $\theta$의 중요도라고 해석할 수 있다. 확률이 높을수록 중요도가 높다.

$$
\begin{align*}\log p(\theta|D) &= \log p(D|\theta) + \log p(\theta) - \log p(D) \ &= \log p(D_B | \theta) + \log p(\theta | D_A ) - \log p (D_B)\end{align*}
$$  

- EWC는 $F_i$(Fisher information matrix)를 활용하여 weight parameter에 제한을 가하는 Loss function(손실함수)을 사용한다.

$$
\begin{align*}
L(\theta) = L_B (\theta) + \sum_i {\frac{\lambda}{2} F_i (\theta_i - \theta^{*}_{A,i})^2}
\end{align*}
$$

- EWC와 기존 tranfer learning의 차이를 시각화해서 해석해보자.

<center><img src="https://user-images.githubusercontent.com/50395556/77232344-d136d000-6be3-11ea-990d-521b22d25422.png" title="EWC"></center>

- Task A에서 좋은 성능을 발휘하는 weight 범위를 회색, task B에서 좋은 성능을 발휘하는 weight 범위를 크림색으로 시각화했다.
- (no penalty) 규제를 가하지 않고 task B를 학습할 경우 weight는 task A의 최적화 범위를 벗어나 task B에만 최적화된다. Task A를 설명하던 weight에 Semantic drift가 발생해 더 이상 task A를 설명하지 못하게 되므로 Catastrophic forgetting이 발생한다.
- (L2) L2 regularization을 적용할 경우 weight는 task A의 지식을 보존하는데 성공한 것 같으나, 실상 task B에 대한 성능을 포기하는 만큼만 task A의 지식을 보존할 뿐이다. 그림에서처럼 어느 쪽도 만족시키지 못하는 위치에 수렴하게 될 위험이 있다.
- (EWC) EWC는 task A의 성능을 유지하는 한에서 task B의 성능을 최대화하는 weight를 찾는다. 그림에서는 task A와 task B 모두에서 오차가 적은 교집합 부분으로 weight가 갱신되었다.

### 2. Structure

Neural Network의 구조를 동적으로 변경함으로써 새로운 task를 수용할 수 있다. 이 접근법은 Network의 Node 또는 Layer의 개수를 추가하여 새로운 task를 학습할 pararmeter를 확보한다. Structure 접근 방법의 대표적인 알고리즘은 Google Deepmind가 발표한 Progressive Networks이다.

#### ▷ Progressive Networks

- Transfer Learning은 모델이 학습한 사전지식을 weight 초기화 단계에서 통합시키는 반면, Progressive Networks는 새로운 task를 학습할 때 모든 사전지식을 그대로 남겨둔다. 새로운 task를 학습할 때는 Network에 sub network를 추가하여 구조를 변경한다. Sub network는 새로운 task를 학습하는데만 사용되며, 사전지식으로부터 유용한 feature를 추출하여 sub network 학습에 활용한다. 그림을 통해 학습 과정을 살펴보자.

<center><img src="https://user-images.githubusercontent.com/50395556/77232370-fb888d80-6be3-11ea-989b-8e81197cb9b4.png" title="Progressive Network" width=600px></center>

- (1) Task 1을 학습할 때는 Deep Neural Network의 기본 구조를 이용한다.
- (2) Task 2를 학습한다. 모델에 sub network (column)를 추가하고, 기존에 있던 Network의 weight는 고정한다. 그림에서는 고정한 weight는 점선으로 표시하였다. weight를 고정하는 이유는 Catastrophic forgetting을 방지하기 위함이다. 기존 Network의 l번째 hidden layer의 출력은 새로 추가된 sub network의 l+1번째 layer의 추가적인 input으로 사용한다. 기존 weight를 추가된 sub network에 통합하는 과정을 lateral connection(측면 연결)이라고 한다.
- (3) Task 3을 학습한다. (2)에서와 마찬가지로 모델에 sub network를 추가하고 기존 Network의 weight는 고정시킨 후,  lateral connection한다.
- Progressive Networks는 각 task를 학습한 후 새로운 neural network (column)을 추가하는 방법을 통해 Catastrophic forgetting을 방지하였음은 물론, lateral connection을 통해 이전의 학습된 column으로부터 knowledge transfer가 진행되어 효율적인 학습이 가능하다.

### 3. Memory

Regularization 접근법과 Structure 접근법이 Neural Network 모델링 관점에서 바라보는 Lifelong Learning인 반면, Memory 접근법은 생물학적인 기억 메커니즘을 모방하자는 아이디어에서 출발하였다. Memory 접근 방법의 대표적인 알고리즘은 SK T-brain이 발표한 Deep Generative Replay이다.

#### ▷ DGR (Deep Generative Replay)

- DGR은 뇌의 해마를 모방하여 만든 알고리즘이다. 해마는 뇌로 들어온 감각 정보를 단기간 저장하고 있다가 이를 대뇌피질로 보내 장기 기억으로 저장하거나 삭제한다. DGR은 단기 기억과 장기 기억의 상보적 학습 관계를 generator와 solver로 구현하였다.
- Generator는 GAN(Generative Adversarial Network)을 기반으로 한다. Online learning은 학습한 데이터를 저장할 수 없지만, GAN으로 학습했던 데이터와 유사한 데이터를 재현(replay)함으로써 해마가 단기 기억을 저장하는 것과 같은 역할을 한다.
- Solver는 주어진 task를 해결하는 장기 기억에 해당하는 역할을 한다. Solver의 특징은 새로운 task B를 학습할 때, generator가 생성한 이전 task A에 대한 데이터를 동시에 학습하는 것이다. 이는 task A와 task B의 데이터를 모두 학습하는 것과 같은 효과가 발생하여 모델이 Multi task를 수행하도록 한다. Generator와 solver로 구성된 이 모델은 학습뿐만 아니라 다른 모델에 학습된 지식을 전달하는 것도 가능하기 때문에 학자(scholar) 모델이라 불린다.

<center><img src="https://user-images.githubusercontent.com/50395556/77232380-0e9b5d80-6be4-11ea-9e46-39d52f403116.png" title="DGR"></center>

- 도식을 통해 학습 과정을 살펴보자. (a) Task 1부터 task N까지 존재할때 DGR은 순차적으로 학습한다. Task 1을 학습할 때 generator는 데이터를 재현하도록 학습하고 solver는 task 1에 대해서 학습한다. Task 1을 학습한 상태를 scholar 1이라고 하자.
- (b) Task 2부터 단기 기억이 활용된다. 먼저 Task 2의 데이터를 $x$, 정답을 $y$라고 하고, Generator가 task 1의 데이터를 재현(replay)한 것을 $y'$라고 하자. Generator는 $x$와 $x'$를 동시에 재현하도록 학습한다.
- (c) $x'$을 solver에 입력하여 얻은 결과를 $y'$이라고 하자. solver는 아직 task 2를 학습하기 전이므로 $x'$와 $y'$은 task 1의 데이터를 재현한 것과 그에 대한 예측이다. $x$, $y$, $x'$, $y'$ 을 모두 사용하여 task 1과 task 2를 동시에 수행할 수 있도록 solver를 학습한다.
- 위 과정을 반복하여 새로운 task를 학습하면서도 Catastrophic forgetting을 막을 수 있다. DGR의 성능은 generator가 생성한 데이터 대신 실제 이전 task의 데이터로 학습했을 때와 거의 비슷하였다. 다음 그림은 generator가 MNIST와 SVHN을 학습하고 동시에 재현한 예시이다.

<center><img src="https://user-images.githubusercontent.com/50395556/77232391-2246c400-6be4-11ea-817e-e2dfeef09636.png" title="DGR2"></center>

### 4. 혼합형

상기한 접근 방법 외에도 많은 연구가 이루어지고 있다. 그 중 Regularization과 Structure 접근법을 혼합하여 성능을 개선한 사례로서 KAIST의 Dynamically Expandable Network를 소개한다.

#### ▷ DEN (Dynamically Expandable Network)

<center><img src="https://user-images.githubusercontent.com/50395556/77232426-325ea380-6be4-11ea-8306-71c89bdd2d64.png" title="DEN" width=600px></center>

- 최초, task A에 대한 학습은 L1 regularization을 이용하여 weight가 sparse(희소)하게 학습한다. L1 regularization은 weight가 정확하게 0으로 떨어지도록 유도하는 특성이 있다. DEN은 이 특성을 현재 task에 중요한 weight parameter를 분별하는 용도로 사용한다. 가중치가 정확하게 0일 경우 모델에서 해당 weight parameter를 삭제한다. 위 그림에서는 가중치가 0인 weight를 점선으로 표시하였다. 이후 다음과 같은 3단계를 통해서 새로운 task를 학습한다.

<center><img src="https://user-images.githubusercontent.com/50395556/77232441-3d193880-6be4-11ea-96cd-ff7262a75d80.png" title="DEN2" width=600px></center>

- (1) Selective retraining 단계는 task B를 학습하는데 중요한 node를 탐색하고 weight를 갱신한다. 먼저 Layer 1까지의 weight는 고정하고 L1 regularization으로 학습하면 task B를 학습하는데 중요한 Layer 2의 node를 찾을 수 있다. 그림에서는 노란색 node에 해당한다. 이 node의 연결선을 따라가면 task B를 학습하는데 중요한 Layer 1의 node를 찾을 수 있다. 계속해서 연결선을 따라가면 task B를 학습하는데 중요한 모든 node와 weight를 찾을 수 있다. 탐색과정이 끝나면 L2 regualrization으로 학습하며 중요한 node와 weight를 미세조정한다.
- (2) Dynamic network expansion 단계는 새로운 task를 학습하는데 모델의 capacity가 부족할 경우 Network를 확장한다. Selective retraining 결과, 새로운 task에 대한 loss가 임계치(threshold) 이상일 경우는 모델의 capacity가 부족하다고 판단한다. 레이어별로 임의의 개수만큼 node를 추가한다. Group sparsity regularization을 이용해 추가된 k개의 node 중 필요 없는 node는 제거하고, 제거되지 않은 node도 sparse하게 만든다.
- (3) Network split/duplication 단계는 Semantic drift를 확인하고 해소한다. Task B를 학습하기 전 weight와 학습 후 weight 사이의 L2 distance를 산출한다. L2 distance가 임계치를 넘으면 Semantic drift가 발생했다고 판단하고 해당 node를 복제하여 레이어에 추가한다. 그림에서는 회색 node가 임계치를 넘었고, 흰색 node가 그렇지 않은 경우이다. 회색 node를 복제하여 노란색 node를 만들고 각 레이어에 추가하였다. 최종적으로 회색 node가 기존 값을 유지하도록 regularization으로 규제하며 모델이 재학습하도록 한다.
- DEN의 학습과정은 이전 task에 관련된 weight는 최대한 유지하면서 새로운 task를 학습하기 때문에 Catastrophic forgetting을 예방한다. 또한 Progressive Networks보다 capacity를 효율적으로 관리할 수 있는 장점이 있다.

## 활용 방안

<center><img src="https://user-images.githubusercontent.com/50395556/77232453-4dc9ae80-6be4-11ea-88f6-47541b9909d7.png" title="Lifelong Learning usage" width=600px></center>

데이터는 끊임없이 증가하고 있다. 데이터가 충분히 많아짐에 따라 클래스도 다양해지고 있다. 시장 수요 또는 연구 목적에 따라서 기존에 분류했던 클래스가 더 세분화되는 경우도 있고, 아예 새로운 클래스가 추가되기도 한다. 클래스가 추가될 때마다 모델을 새로 구축하는 방법으로는 급증하는 데이터의 속도를 따라갈 수도 없을뿐더러 모델을 교체할 때마다 비용도 발생한다. Lifelong Learning은 모델을 교체하지 않고 새로운 클래스를 추가할 수 있고 데이터의 변화에도 실시간 대응할 수 있다.

<center><img src="https://user-images.githubusercontent.com/50395556/77232456-50c49f00-6be4-11ea-9ac9-d28255114584.png" title="Lifelong Learning usage2" width="600px"></center>

Lifelong Learning은 분류 문제 뿐만 아니라 여러 분야에 적용할 수 있다. 위 그림은 입력된 이미지 내에서 물체를 찾아 픽셀단위로 추출하는 Image semantic segmentation의 예시이다. Sky의 존재 유무를 식별하는 task를 학습한 AI가 Building의 존재 유무도 식별하도록 학습할 수 있다. 이러한 방식으로 task를 지속적으로 추가해나간다면 Multi task를 수행하는 모델로 발전할 수 있다. 

## 이미지 출처 및 참고

[1]  James Kirkpatrick, Razvan Pascanu, Neil Rabinowitz, Joel Veness, Guillaume Desjardins, Andrei A. Rusu, Kieran Milan, John Quan, Tiago Ramalho, Agnieszka Grabska-Barwinska, Demis Hassabis, Claudia Clopath, Dharshan Kumaran, Raia Hadsell. Overcoming catastrophic forgetting in neural networks, arXiv:1612.00796,  2016
[2]  Andrei A. Rusu, Neil C. Rabinowitz, Guillaume Desjardins, Hubert Soyer, James Kirkpatrick, Koray Kavukcuoglu, Razvan Pascanu, Raia Hadsell. Progressive Neural Networks, arXiv:1606.04671, 2016
[3]  Hanul Shin, Jung Kwon Lee, Jaehong Kim, Jiwon Kim. Continual Learning with Deep Generative Replay, arXiv:1705.08690, 2017
[4]  Jaehong Yoon, Eunho Yang, Jeongtae Lee, Sung Ju Hwang. LIFELONG LEARNING WITH DYNAMICALLY EXPANDABLE NETWORKS, arXiv:1708.01547, 2017
[5]  http://dmqm.korea.ac.kr/activity/seminar/266
[6]  https://tv.naver.com/v/3941879
