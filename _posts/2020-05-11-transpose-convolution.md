---
layout: post
title: "CS231n의 Transposed Convolution은 Deconvolution에 가까운 Transposed Convolution이다"
tags: [DeepLearning]
author: jinhyo
feature-img: /assets/img/pexels/triangular.jpeg 
use_math: true
keywords: Transposed Convolution, Deconvolution, Upsampling
---

**일러두기**

본 게시글은 Transposed Convolution에 대하여 개인적으로 공부한 내용을 정리한 것입니다. Transposed Convolution의 다른 이름들이 붙은 이유는 제가 고민해서 도출한 추론이므로 사실과 다를 수 있습니다. Deconvolution과 Tranposed Convolution의 관계 또한 제가 이해한 내용을 바탕으로 썼습니다. Convolution 연산의 수학적 계산은 아직 완전히 이해하지 못하였기 때문에 개념적으로 이해한 내용에 기반했습니다.

이 글에서 다루는 내용
- [Convolution](#Convolution)
- [Upsampling](#Upsampling)
- [Transposed Convolution](#Transposed-Convolution)
- [Deconvolution](#Deconvolution)
- [CS213n의 Transposed Convolution 예제에 대한 의문](#CS213n의-Transposed-Convolution-예제에-대한-의문)
    - [1차원 Transposed Convolution](#1차원-Transposed-Convolution)
    - [Keras Conv2DTranspose](#Keras-Conv2DTranspose)
- [Transposed Convolution 다른 계산 방법](#Transposed-Convolution-다른-계산-방법)
    - [Transposed Convolution with stride](#Transposed-Convolution-with-stride)
    - [손계산 : 2가지 계산방법 비교](#손계산)
- [참고한 문헌 및 웹페이지](#참고한-문헌-및-웹페이지)

---

Transposed Convolution은 Upsampling 기법의 일종이다.  
다른 이름으로는 Deconvolution\*, Upconvolution, Fractionally strided convolution, Backward strided convolution 이라고 불리기도 하는데, 이러한 이명들을 가지고 있는 이유는 연산 과정 때문인 것 같다.  

\* Deconvolution은 엄밀히 말하면 다르지만, 일부 문헌에서 Transposed Convolution을 지칭하는 경우가 있다.

## Convolution

먼저 일반적인 convolution의 연산 과정을 애니메이션으로 살펴보자.

<br><center><img src="https://github.com/vdumoulin/conv_arithmetic/raw/master/gif/no_padding_no_strides.gif" title="convolution gif"></center>  

이미지 출처 : [vdumoulin github](https://github.com/vdumoulin/conv_arithmetic)  

파란색 feature map(4x4)이 input이고, 초록색 feature map(2x2)이 output이다.  
3x3 kernel, strides 1, no padding 으로 연산한 것이다.

## Upsampling

위 애니메이션을 통해서 직관적으로 알 수 있듯이, Convolution 연산은 output의 크기가 input보다 작아지는 특성이 있다. 딥러닝에서 convolution layer를 깊게 쌓을 수록 feature map의 크기가 계속 작아지게 되는데, 반대로 output의 크기가 input보다 커지게 하는 방법을 Upsampling 이라고 한다. Upsampling에는 unpooling과 transposed convolution이 있다.  

Upsampilng이 필요한 예시는 다음과 같다.

<br><center><img src="https://kr.mathworks.com/help/vision/ug/semanticsegmentation_transferlearning.png" title="semantic segmentation"></center>  

이미지 출처 : [MathWorks](https://kr.mathworks.com/help/vision/ug/getting-started-with-semantic-segmentation-using-deep-learning.html)  

semantic semantation은 convolution layer들을 거쳐 feature map의 크기를 줄여나가다가, 다시 feature map의 크기를 키우는 구조이다. input 이미지를 압축된 벡터로 표현했다가 원래의 input 이미지와 동일한 크기로 되돌리는 것인데, 이미지의 픽셀 단위로 예측하기 위해 높은 해상도가 필요하기 때문이다.  

<br><center><img src="https://github.com/zhixuhao/unet/raw/master/img/u-net-architecture.png" title="unet"></center>  

이미지 출처 : [zhixuhao github](https://github.com/zhixuhao/unet)  

또다른 Upsampling의 활용처로는 U-net이 있다. 모델의 중간 결과물들(layer마다 얻어지는 feature map)을 합치는 구조이다. feature map을 합치기 위해서는 서로 모양이 맞아야하므로 Upsampling이 사용된다.  

Upsampling의 필요성에 대하여 간단히 언급하였고, 이 글에서 중요한 것은 Transposed Convolution이다.  

## Transposed Convolution

Convolution 계산을 하면 feature map이 작아진다. 반대로 feature map이 커지게 하려면 어떻게 해야할까? 단순하게 생각해보면 Convolution 계산과정을 역으로 하면 된다.  
~~(조립은 분해의 역순...)~~  

<br><center><img src="https://user-images.githubusercontent.com/50395556/81533378-0f10d300-93a1-11ea-9e69-e775b3c4bdd4.png" title="convolution"></center>  

위 Convolution 그림에서 input의 빨간색 박스안에 있는 원소들이 kernel과 곱해져서 output의 빨간색 원소가 된다. input의 파란색 박스 안에 있는 원소들은 output의 파란색 원소와 대응한다. output의 나머지 원소에 대해서도 마찬가지로 대응하는 원소들을 찾을 수 있다. 그림에서는 나머지 원소에 대한 대응 원소 표시는 생략하였다.

<br><center><img src="https://user-images.githubusercontent.com/50395556/81541105-9401e980-93ad-11ea-87a1-a7676fbd8314.png"></center>  

input과 output의 위치를 바꿔서 생각해보자. 위 그림에서 input의 빨간색 원소는 output의 빨간 박스 안에 있는 원소들과 연관이 있다. 마찬가지로 input의 파란색 원소는 output의 파란색 박스 안에 있는 원소들과 대응한다.

Transposed Convolution을 계산하는 방법은 이렇다. input의 빨간색 원소를 3x3 kernel에 곱해서 output의 대응하는 자리에 집어넣는다. 같은 방법으로 input의 파란색 원소를 3x3 kernel에 곱해서 output의 대응하는 위치에 집어넣는다. 이 때 output에 겹치는 구간(빗금 표시)이 발생하는데, 겹치는 부분의 값은 모두 더해준다. input의 나머지 원소에 대해서도 동일한 방법으로 계산한다.

## Deconvolution

Transposed Convolution을 계산하는 과정이 마치 convolution 연산을 거꾸로 계산하는 것과 같아보인다. 그래서 Deconvolution이라고도 불리는 것 같다. 그러나, 수학적으로 Deconvolution은 정확히 Convolution의 역 연산을 일컫는다. Convolution 연산의 수식은 다음과 같다.

$f*g=h$

$f$는 filter(또는 kernel), \*는 convolution 연산, $g$는 input, $h$는 output이다. $f$(kernel)와 $h$(output)을 알고있는 상태에서 $g$(input)를 구하는 것이 Deconvoluion이다. 

그말인 즉슨, 이미지를 convolution layer를 통과시켜 얻은 feature map을 deconvolution하면 원래의 이미지를 얻을 수 있다는 것이다. (convolution은 적분을 사용하기 때문에 $g$를 완벽히 찾을 수는 없지만 근사할 수 있다.)

여기서 Transposed Convolution과 Deconvolution의 개념적인 차이가 있다. Deconvolution은 Convolution 연산할 때 사용한 kernel과 output을 알고 있어야하며, 역 연산을 통해서 input을 재현하는 목적이라고 볼 수 있다. 그러나 Transposed Convolution에서 사용하는 kernel은 어떤 convolution layer와 공유하는 것이 아니다. Transposed convolution layer가 학습을 통해서 kernel을 찾아간다는 점에서 차이가 있다.

<br><center><img src="https://user-images.githubusercontent.com/50395556/80391039-2c956580-88e8-11ea-85c9-ee22de611c2a.png" title="deconvolution"></center>

Deconvolution으로 feature map 시각화한 예시 / 이미지 출처 : [Learning Deconvolution Network for Semantic Segmentation](https://arxiv.org/abs/1505.04366)

Deconvolution은 feature map을 3차원 이미지로 재구성하여 feature map을 시각화하는 용도로도 사용되고 있다. Activation Function을 거쳤기 때문에 완벽한 이미지로 재구성되는 것은 아니고, 활성화된 부분만 시각화되므로 예측의 근거를 찾는데 활용한다. 시각화할 때는 이전 convolution layer의 kernel을 그대로 이용하며, 학습이 일어나지 않는다. (kernel을 업데이트하지 않는다.) 이쪽이 역 연산이라는 개념에 더욱 부합한다. 그러므로 Deconvolution과 Transposed Convolution은 용어를 구분하여 사용하는 것이 맞다고 생각한다.

## CS213n의 Transposed Convolution 예제에 대한 의문

<br><center><img src="https://user-images.githubusercontent.com/50395556/81540311-6c5e5180-93ac-11ea-92be-d0e279015495.png" title="cs231n transposed convolution"></center>

이미지 출처 : [CS231n](http://cs231n.stanford.edu/)

이 글을 쓰게 된 계기가 바로 이 PPT이다.  
2x2 input에 3x3 kernel, stride 2, padding 1을 적용한 예시에서 output shape이 4x4가 된다. 위에서 찾았던 계산한 방식대로라면 5x5 output이어야 한다고 생각했다. PPT에서 output의 빨간색 박스와 파란색 박스의 일부분이 포함이 안되는 것에 의아함을 느꼈다. 딥러닝에서 대칭성 없이 값을 버리는 것을 본 적이 없었기 때문에 당장에 이해가 안되었다. padding 1 때문이라면 오른쪽과 하단도 1줄씩 버려서 3x3 output이 되어야하는게 아닌가 싶었다. 

<br><center><img src="https://user-images.githubusercontent.com/50395556/81542337-75045700-93af-11ea-89d6-0b35870b4b7d.png" title="transposed convolution stride 2"></center>

위에서 설명했던 계산방식대로 하면 이렇게 되어야 한다고 생각했다. 

<br><center><img src="https://user-images.githubusercontent.com/50395556/81543395-f8727800-93b0-11ea-9ed2-2b41abb17bc2.png" title="convolution stride 2 pad 1"></center>

범인은 padding 1 이었다. Transposed Convolution을 바로 계산하기에 앞서, 주어진 조건을 Convolution에 대입해서 그림으로 그려보았다. padding을 점선으로 표시하였더니 연산된 영역 중에서 왼쪽과 상단은 padding에 해당하였다. 

즉, Transposed Convolution 계산방법대로 했을 때는 5x5 output이 나오는데, Convolution할 때 padding 1이 적용되었었다는 단서가 있으므로, padding에 해당하는 부분을 지우는 것이다.

개인적으로 이 계산 방법은 원래 Convolution 할 때의 input shape을 알고 있다는 전제가 있어야 하기 때문에 Deconvolution적인 관점이라는 생각이 든다. Transposed Convolution을 하기 전에 Convolution하기 전의 원래 크기를 알고 있어야한다는 가정이 딥러닝의 feed forward 개념과 부합하지 않는다고 느껴진다. 

물론 강의 영상에서는 이 방법은 Deconvolution과는 다르며, Deconvolution이라는 용어 사용도 지양하라고 했다. Transposed Convolution은 kernel이 학습을 통해 업데이트되는 점이 차이라고 했다. 그러나 독립된 레이어로서 input과 kernel만으로 output을 내지 않고, 원래의 모양으로 되돌리려하는 느낌이 Deconvolution 관점에 가까운 것 같다.

### 1차원 Transposed Convolution

이어지는 PPT 슬라이드에서는 1차원 데이터로 설명을 보충하였다. 

<br><center><img src="https://user-images.githubusercontent.com/50395556/81567103-c8d66680-93d6-11ea-8aa8-2e4e88e36cbe.png" title="cs231 ppt2"></center>

이미지 출처 : [CS231n](http://cs231n.stanford.edu/)

위는 input(size 2), kernel(size 3), stride 2를 계산하는 그림이다. 앞선 PPT 슬라이드에서 2x2 input이 4x4 input이 되었는데, 2 input은 5 input인 예시를 들어서 좀 더 혼란스러웠다. 여기서는 padding 1을 적용하지 않았기 때문에 shape이 다르다. 계산 방법 자체는 2D나 1D나 같다.

<br><center><img src="https://user-images.githubusercontent.com/50395556/81567231-020ed680-93d7-11ea-8fad-fae7014a13a5.png" title="cs231 ppt3"></center>

이미지 출처 : [CS231n](http://cs231n.stanford.edu/)

다음 장에서는 행렬과 벡터로 연산 과정을 설명했다. 왼쪽 식의 $\vec{a}$가 input이다. 원래는 4차원 벡터 $\vec{a}=[a,b,c,d]^T$인데 padding 1이 더해졌기 때문에 6차원 벡터 $[0, a,b, c, d, 0]^T$라고 쓰였다. kernel은 $\vec{x}=[x,y,z]$인데 slide 2 convolution을 행렬로 표현했다.

우측의 식에서 행렬과 벡터로 쓴 부분을 보면 kernel 부분을 이항한 것이다. 그런데 변수문자가 바뀌어서 좀 혼란스러운데, $[a,b]^T$는 왼쪽식에서 output(우변)을 의미하고 $[ax, ay, az+bx, by, bz, 0]^T$은 왼쪽식에서 padding이 포함된 $\vec{a}$와 대응한다.

앞서 보았던 2D의 예제와 같은 논리라면, 여기서 Trasposed Convolution의 output은 $[ay, az+bx, by, bz]^T$이다. 원래 input이었던 $\vec{a}$와 대조했을 때 $ax$는 0이기 때문이다. 그런데 이번 슬라이드에서는 그러한 표시를 하지 않았다. 더군다나 마지막 차원에 있는 0도 그대로 둬서 마치 Transposed Convolution의 결과가 6차원인 것처럼 보인다. 

여러모로 PPT가 일관성이 부족한 느낌이 들어서 아쉽고 많이 혼란스러웠다.

### Keras Conv2DTranspose

코드로 돌렸을 때 결과가 나오는 것이 정답이라고 생각하고 실험해보았다. 2x2 input, 3x3 kernel, 2x2 stride, 0 padding의 Transposed Convolution 결과는 5x5 output이었다.

<br><center><img src="https://user-images.githubusercontent.com/50395556/81558261-55c5f380-93c8-11ea-90db-6d1bfbabf676.png" title="keras Conv2DTranspose"></center>

keras에서는 padding 옵션이 'valid'와 'same' 뿐이다. cs231n PPT에 나온 것처럼 kernel 3x3, stride 2, padding 1로 2x2 input을 4x4 output으로 만들고 싶었는데, padding을 임의로 바꿀 수가 없었다.

왜 이렇게 구현되었는지까지는 아직 알아내지 못했다. 다만 구글에서 이렇게 만들었으니 이 방법이 업계 표준인가보다 정도로 받아들이기로 했다.

## Transposed Convolution 다른 계산 방법

처음에 Transposed Convolution을 찾아볼 때 가장 많이 봤던 그림은 아래 애니메이션이었다.

<br><center><img src="https://github.com/vdumoulin/conv_arithmetic/raw/master/gif/no_padding_no_strides_transposed.gif" title="transposed convolution gif"></center>  

이미지 출처 : [vdumoulin github](https://github.com/vdumoulin/conv_arithmetic)  

위 애니메이션은 A guide to convolution arithmetic for deep learning이라는 논문에 설명된 Transposed Convolution의 계산방법이다.  
파란색 feature map이 input이고, 초록색 feature map이 output이다.  
2x2 input 주위로 공백을 추가함으로써 원하는 4x4 output을 내는데 성공하였다.

여기서 공백은 padding이 아니라 stride에 따라서 결정된다. (별다른 조건이 없으면 기본적으로 $kernel size - 1$만큼 공백 추가)

## Transposed Convolution with stride

<br><center><img src="https://github.com/vdumoulin/conv_arithmetic/raw/master/gif/no_padding_strides_transposed.gif" title="transposed convolution stride 2 gif"></center>  

이미지 출처 : [vdumoulin github](https://github.com/vdumoulin/conv_arithmetic)  

stride가 있을 때, input의 원소 사이에 공백을 추가한다.  

stride는 convolution 연산할 때 input에서 sliding window가 움직이는 보폭을 의미한다. Transposed Convolution은 Convolution을 거꾸로 하는 개념이라고 했다. stride 2를 거꾸로 생각하면 stride 1/2이 된다. stride가 1/2이라는 말은 2번 움직여야 다음 원소에 도달한다는 말과 같다. 위 이미지에서 input(파란색)의 한 원소에서 다음 원소까지 도달하는데 sliding이 2번 필요하다. 원소간에 공백 때문에 1/2씩 움직이는 개념이 된다.

Fractionally\* strided Convolution(fractionally : 아주 조금)이라는 이름이 붙은 이유는 이 때문인 것 같다.

## 손계산

Transposed Convolution을 처음에 찾아보았을 때 지칭하는 이름도 여러개 있고, 알고보면 다른 개념도 혼재되어 있어서 혼란스러웠다. 그런데 계산하는 방법도 2가지가 있어서 더욱 혼란스러웠다. 계산 원리를 설명하는 글도 많았다. 느낌은 조금 알겠는데 머리로 이해가 잘 안되었다. 그래서 직접 손으로 써서 어떤 차이가 있는지 확인해보았다.

<br><center><img src="https://user-images.githubusercontent.com/50395556/81561047-83fa0200-93cd-11ea-887e-7d9f2d268455.png" title="handwriting1"></center>  

첫번째로 소개했던 계산방법대로 계산을 해보았다. x,y,z,w에 각각 kernel을 곱해서 대응하는 위치에 적었다. 2x2 input, 3x3 kernel, stride 2, padding 0으로 계산했다.

<br><center><img src="https://user-images.githubusercontent.com/50395556/81561081-95dba500-93cd-11ea-88a5-4cc325d59e7d.png" title="handwriting2"></center>  

두번째로 공백을 추가하는 방법을 계산해보았다. 앞선 계산 결과와 패턴이 유사한 느낌이 들었다.  
kernel에 쓴 원소문자를 반전시키면 결과가 같아질 것 같은 느낌이 들었다.

<br><center><img src="https://user-images.githubusercontent.com/50395556/81561129-a5f38480-93cd-11ea-8416-bea8829d3932.png" title="handwriting3"></center>

결국 2가지 방법 모두 결과는 같았다.  
무식하게 직접 해보았지만, 나처럼 무식하게 계산해볼 뻔한 누군가의 소중한 시간을 아끼는데 이 글이 도움이 되었길...

## 참고한 문헌 및 웹페이지

[1] [Vincent Dumoulin, Francesco Visin, A guide to convolution arithmetic for deep learning (arXiv:1603.07285), 2018](https://arxiv.org/abs/1603.07285)  
[2] [Convolution arithmetic(vdumoulin github)](https://github.com/vdumoulin/conv_arithmetic)  
[3] [[논문리뷰] CNN에서의 Deconvolution 이해하기 [1]](https://dambaekday.tistory.com/3)  
[4] [Learning Deconvolution Network for Semantic Segmentation (arXiv:1505.04336)](https://arxiv.org/abs/1505.04366)  
[5] [CS231n Winter 2016: Lecture 13: Segmentation, soft attention, spatial transformers](https://www.youtube.com/watch?v=ByjaPdWXKJ4&t=1526s)  
[6] [딥러닝에서 사용되는 여러 유형의 Convolution 소개](https://zzsza.github.io/data/2018/02/23/introduction-convolution/)