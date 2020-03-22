---
layout: post
title: "docker GPU 사용법"
tags: [Docker]
author: jinhyo
feature-img: /assets/img/pexels/triangular.jpeg 
use_math: false
keywords: [GPU]
---
## 시작하기 전에

본 포스팅은 공식문서를 기반으로 작성하였으며, ubuntu 18.04 LTS에서 정상동작을 확인했습니다.  
작성일자(2020-02-12) 기준으로 설명했기 때문에 에러가 날 경우에는 공식 문서를 확인하기 바랍니다.  
[NVIDIA/nvidia-docker 공식문서](https://github.com/NVIDIA/nvidia-docker)  

### 유의사항

- **nvidia-docker는 linux에서만 지원되며, windows는 docker container에서 gpu를 사용할 수 없다.**  

- **host OS의 준비사항으로 cuda-toolkit은 설치할 필요 없지만, nvidia-driver는 설치되어 있어야 한다.**  

- **본 포스팅의 내용은 19.03 버전 이상에서만 적용되는 예제코드이다.**  
    - nvidia-docker2 패키지와 gpu 사용 방법이 다르므로 반드시 docker 버전을 확인하자. 
    - nvidia-docker2 패키지는 deprecated될 예정이므로 docker 19.03 버전 이상을 설치하는 것을 권장한다.
    - docker를 설치하는 방법은 다른 블로그들에 쉽게 설명되어 있지만, 간혹 19.03 이전 버전을 명시해서 설치하는 경우가 있으니 버전을 반드시 확인하자.
    - docker 버전 확인하는 방법

    ```bash
    docker version
    ```

## GPU 옵션

### 1. GPU 모두 사용하기

- `--gpus` 옵션에 all 

```bash
docker run --gpus all [container_name]
```

### 2. GPU 지정해서 사용하기

- 따옴표 안에 쌍따옴표로 되어 있음에 유의
- device 번호는 0부터 시작한다.
- device 번호는 nvidia-smi에서 보이는 gpu 순서와 동일하다.

```bash
docker run --gpus '"device=0"' [container_name]
```

### 3. GPU 여러개 지정해서 사용하기

- device 번호를 콤마로 구분해서 여러개 지정할 수 있다.

```bash
docker run --gpus '"device=0,1"' [container_name]
```
