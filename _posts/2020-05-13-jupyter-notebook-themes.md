---
layout: post
title: Jupyter Notebook 테마 변경하는 방법
tags: [Python]
author: jinhyo
feature-img: /assets/img/pexels/triangular.jpeg 
use_math: true
keywords: jupyterthemes, dark mode, jupyter notebook
---

jupyter notebook을 하루종일 보고 있자니 눈이 너무 아파서 다크테마를 찾아보았습니다. 테마 바꾸고 나니 눈이 훨씬 편해졌습니다. 다크테마는 뭔가 개발자 허세같아보여서 하기 싫었는데 그게 아니었습니다. 다크테마는 생존문제였습니다.

바쁘신 분들을 위한 요약 :

2장 [jupyterteme 패키지 설치]까지 따라하고 4장 [추천 테마 및 옵션]에서 코드만 복붙하면 됩니다.

스크롤이 길지만 실질적으로는 다음과 같은 코드 2줄이면 끝입니다.

```bash
# jupyterteme 패키지 설치
pip install jupyterthemes

# 추천 테마 및 옵션 적용
jt -t onedork -fs 115 -nfs 125 -tfs 115 -dfs 115 -ofs 115 -cursc r -cellw 80% -lineh 115 -altmd  -kl -T -N

# 끝
```

---

## 목차

1. [아나콘다 터미널에 접속](##1.-아나콘다-터미널에-접속)

2. [jupyter theme 패키지 설치](##jupyter-theme-패키지-설치)

3. [지원하는 테마 적용 (미리보기 이미지)](##지원하는-테마-적용)

    - [onedork](###a.-onedork)
    - [grade3](###b.-grade3)
    - [oceans16](###c.-oceans16)
    - [chesterish](###d.-chesterish)
    - [monokai](###e.-monokai)
    - [solarizedl](###f.-solarizel)
    - [solarizedd](##g.-solarized)

4. [추천 테마 및 옵션](##추천-테마-및-옵션)

    - [현재 사용 중인 테마](###현재-사용-중인-테마)
    - [jupyterthemes 개발자의 추천 테마(dark)](###jupyterthemes-개발자의-추천-테마(dark))
    - [jupyterthemes 개발자의 추천 테마(light)](###jupyterthemes-개발자의-추천-테마(light))

5. [커스터마이즈 옵션](##커스터마이즈-옵션)

6. [기본 테마 되돌리기](##기본-테마-되돌리기)

---

## 1. 아나콘다 터미널에 접속

시작프로그램에서 Anaconda Prompt을 검색해서 터미널 창을 띄웁니다.

또는 [그림 2] 처럼 jupyter notebook에서 Terminal로 접속해도 됩니다. (택 1)

![anaconda prompt icon](https://user-images.githubusercontent.com/50395556/81769185-ec54fa80-9517-11ea-9cf3-a81e4fd4a551.png)

![anaconda prompt](https://user-images.githubusercontent.com/50395556/81769244-13133100-9518-11ea-9eab-d8a0be3a31fe.png)

[그림 1] Anaconda Prompt 앱 아이콘 및 터미널 창

![jupyter notebook terminal open](https://user-images.githubusercontent.com/50395556/81769282-2a521e80-9518-11ea-9e70-c62e746a3775.png)

[그림 2] jupyter notebook 안에서 Terminal 접속

![jupyter notebook terminal](https://user-images.githubusercontent.com/50395556/81769304-3b9b2b00-9518-11ea-997b-0d8adaef9f1b.png)

[그림 3] jupyter notebook 내에서 terminal 접속 화면

## 2. jupyter theme 패키지 설치

다음 명령어로 jupyterthemes를 설치합니다.

jupyter notebook의 테마 7가지를 지원하며, 세부조정도 가능합니다.

```bash
pip install jupyterthemes
```

※ 혹시 버전에러가 발생할 경우 jupyter notebook과 jupyterthemes를 모두 업데이트해주세요.

```bash
# jupyter notebook 최신버전
pip install --upgrade notebook

# jupyter notebook 최신버전
pip install --upgrade jupyterthemes
```

## 3. 지원하는 테마 적용

직접 하나씩 적용해보는 수고를 덜어드리고자, 테마별 적용 화면 캡쳐를 첨부하였습니다.  
이미지를 보고 마음에 드는 테마를 고르시면 됩니다.  
아나콘다 터미널에서 테마 적용 코드를 붙여넣은 뒤 실행하면 즉시 적용됩니다.  
jupyter notebook을 재시작할 필요없이 새로고침만 누르면 됩니다.

### a. onedork

```bash
jt -t onedork
```

- 홈 화면

    ![onedork home](https://user-images.githubusercontent.com/50395556/81769894-ae58d600-9519-11ea-80c7-0c60c4f76f76.png)

- 노트북 화면

    ![onedork notebook](https://user-images.githubusercontent.com/50395556/81769895-af8a0300-9519-11ea-8ff8-ee97b8ed54e9.png)

### b. grade3

```bash
jt -t grade3
```

- 홈 화면

    ![grade3 home](https://user-images.githubusercontent.com/50395556/81769923-bca6f200-9519-11ea-81c6-022e6a3de873.png)

- 노트북 화면

    ![grade3 notebook](https://user-images.githubusercontent.com/50395556/81769927-bdd81f00-9519-11ea-8bf4-f956a804f3b7.png)

### c. oceans16

```bash
jt -t oceans16
```

- 홈 화면

    ![oceans16 home](https://user-images.githubusercontent.com/50395556/81769991-f1b34480-9519-11ea-95d8-0883df0e5b8d.png)

- 노트북 화면

    ![oceans16 notebook](https://user-images.githubusercontent.com/50395556/81769992-f2e47180-9519-11ea-86f0-f85d75983d5c.png)

### d. chesterish

```bash
jt -t chesterish
```

- 홈 화면

    ![chesterish home](https://user-images.githubusercontent.com/50395556/81770039-0b548c00-951a-11ea-9045-72d6313285cd.png)

- 노트북 화면

    ![chesterish notebook](https://user-images.githubusercontent.com/50395556/81770041-0bed2280-951a-11ea-91bf-59e2fbba89a1.png)

### e. monokai

```bash
jt -t monokai
```

- 홈 화면

    ![monokai home](https://user-images.githubusercontent.com/50395556/81770055-1c050200-951a-11ea-83f9-f7bfc77be954.png)

- 노트북 화면

    ![monokai notebook](https://user-images.githubusercontent.com/50395556/81770059-1d362f00-951a-11ea-99b8-f6fa3cbf4ab8.png)

### f. solarizedl

```bash
jt -t solarizedl
```

- 홈 화면

    ![for%20you%204a8fb6751341479287434f4b1c15f588/Untitled%2014.png](for%20you%204a8fb6751341479287434f4b1c15f588/Untitled%2014.png)

- 노트북 화면

    ![for%20you%204a8fb6751341479287434f4b1c15f588/Untitled%2015.png](for%20you%204a8fb6751341479287434f4b1c15f588/Untitled%2015.png)

### g. solarizedd

```bash
jt -t solarizedd
```

- 홈 화면

    ![solarizedl home](https://user-images.githubusercontent.com/50395556/81770087-30e19580-951a-11ea-9ffb-1ee70aa6f747.png)

- 노트북 화면

    ![solarizedl notebook](https://user-images.githubusercontent.com/50395556/81770090-317a2c00-951a-11ea-8801-01d8837c2108.png)

## 4. 추천 테마 및 옵션

### a. 현재 사용중인 테마

저는 이미 기본 테마에 익숙해졌기 때문에 폰트 크기라던지,  메뉴 탭의 위치, 화면 레이아웃 등이 기본테마와 최대한 유사하게 만들어서 사용하고 있습니다.

다크테마는 쓰고 싶지만, 레이아웃이 바뀌는 건 싫고 커스터마이즈는 귀찮다면 추천합니다.

![my theme home](https://user-images.githubusercontent.com/50395556/81770100-46ef5600-951a-11ea-96cd-e681c4059fb9.png)

![my theme notebook](https://user-images.githubusercontent.com/50395556/81770102-48208300-951a-11ea-85d6-6cc8ee021f3a.png)

```bash
jt -t onedork -fs 115 -nfs 125 -tfs 115 -dfs 115 -ofs 115 -cursc r -cellw 80% -lineh 115 -altmd  -kl -T -N
```

옵션 설명 적어드립니다. 커스텀에 참고하세요.

-fs 115 : 코드 폰트 사이즈

-nfs 125 : 노트북 메뉴 폰트 사이즈

-tfs 115 : 마크다운 폰트 사이즈

-dfs 115 :  pandas DataFrame 폰트 크기 

-ofs 115 : Output 영역 폰트 크기

-cursc r : cursor 색 red (onedork theme에서는 red가 가장 눈에 띄는 듯)

-cellw 80% : 셀 가로 길이 80% (숫자 클수록 화면에 꽉참)

-lineh 115 : 코드 줄 간격

-altmd : 마크다운 셀의 배경을 투명하게 하는 옵션 

-kl : 커널 로고 표시 (노트북 화면 우상단에 python 로고)

-T :  메뉴탭 아래에 툴바 표시 (저장, 셀 추가/삭제/이동, 커널 중단/재실행 등)

-N : 노트북 화면에서 파일명 표시

### b. jupyterthemes 개발자의 추천 테마 (dark)

jupyter themes 주인장의 추천 테마는 다음과 같습니다.

```bash
# dark
jt -t onedork -fs 95 -altp -tfs 11 -nfs 115 -cellw 88% -T
```

![developer theme ligth notebook](https://user-images.githubusercontent.com/50395556/81770140-5e2e4380-951a-11ea-9f9c-0c2acf36a60d.png)

![developer theme light home](https://user-images.githubusercontent.com/50395556/81770141-5f5f7080-951a-11ea-8f0b-0e5cc9da37ea.png)

### c. jupyterthemes 개발자의 추천 테마 (light)

```bash
# light
jt -t grade3 -fs 95 -altp -tfs 11 -nfs 115 -cellw 88% -T
```

![debeloper theme home](https://user-images.githubusercontent.com/50395556/81770155-6a1a0580-951a-11ea-8555-df38754069ce.png)

![developer theme notebook](https://user-images.githubusercontent.com/50395556/81770156-6b4b3280-951a-11ea-8d2e-635ea5edc1f8.png)

## 5. 커스터마이즈 옵션

jupyterthemes 공식문서에서 발췌한 내용입니다.

**Command Line Usage**

```bash
jt  [-h] [-l] [-t THEME] [-f MONOFONT] [-fs MONOSIZE] [-nf NBFONT]
    [-nfs NBFONTSIZE] [-tf TCFONT] [-tfs TCFONTSIZE] [-dfs DFFONTSIZE]
    [-m MARGINS] [-cursw CURSORWIDTH] [-cursc CURSORCOLOR] [-vim]
    [-cellw CELLWIDTH] [-lineh LINEHEIGHT] [-altp] [-altmd] [-altout]
    [-P] [-T] [-N] [-r] [-dfonts]

```

#### Description of Command Line options

| cl options            |   arg   |  default   |
| :-------------------- | :-----: | :--------: |
| Usage help            |   -h    |     --     |
| List Themes           |   -l    |     --     |
| Theme Name to Install |   -t    |     --     |
| Code Font             |   -f    |     --     |
| Code Font-Size        |   -fs   |     11     |
| Notebook Font         |   -nf   |     --     |
| Notebook Font Size    |  -nfs   |     13     |
| Text/MD Cell Font     |   -tf   |     --     |
| Text/MD Cell Fontsize |  -tfs   |     13     |
| Pandas DF Fontsize    |  -dfs   |     9      |
| Output Area Fontsize  |  -ofs   |    8.5     |
| Mathjax Fontsize (%)  | -mathfs |    100     |
| Intro Page Margins    |   -m    |    auto    |
| Cell Width            | -cellw  |    980     |
| Line Height           | -lineh  |    170     |
| Cursor Width          | -cursw  |     2      |
| Cursor Color          | -cursc  |     --     |
| Alt Prompt Layout     |  -altp  |     --     |
| Alt Markdown BG Color | -altmd  |     --     |
| Alt Output BG Color   | -altout |     --     |
| Style Vim NBExt*      |  -vim   |     --     |
| Toolbar Visible       |   -T    |     --     |
| Name & Logo Visible   |   -N    |     --     |
| Kernel Logo Visible   |   -kl   |     --     |
| Reset Default Theme   |   -r    |     --     |
| Force Default Fonts   | -dfonts |     --     |

옵션 설명 참고: [dunovank/jupyter-themes](https://github.com/dunovank/jupyter-themes)

## 6. 기본 테마 되돌리기

세팅에 망했다고 울지마세요. 한방에 원상복구 시켜드립니다.

```bash
# 기본 세팅으로 되돌리기(restore)
jt -r
```
