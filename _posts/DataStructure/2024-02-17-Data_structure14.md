---
layout: single
title:  "자료구조Study14_문자열검색 알고리즘_KMP,보이어"
categories: DataStructure
tag: [DataStructure]
toc: true
toc_sticky: true
sidebar:
    nav: "counts"
---

# 문자열 검색

문자열 안에 존재하는 특정 단어를 빠르게 찾아내는 것이 목적

* 본문 : 탐색대상이 되는 문자열
* 패턴 : 검색어를 의미
* 이동 : 본문에서의 탐색위치를 옮기는 것

## KMP 알고리즘
<br>
<span style="font-size:150%">패턴과 본문내의 문자열을 한 차례 비교하고 나면 , 다음 단계의 검색에서 사용할 수 있는 '어떤 정보'가 남으며 이 정보를 이용하면 불필요한 비교를 피할수있음을 이용</span>

*

### 접두부 , 접미부 , 경계
* 접두부 : 문자열의 머리부분
* 접미부 : 문자열의 꼬리부분
    * 빈 문자열도 접두부, 접미부가 될 수 있다.
* 경계 : 서로 일치하는 접두부와 접미부
<br>

|BAABABAA|
![image](https://github.com/silverlnng/DatastructureStudy/assets/112385982/f6314d41-a908-44b6-b2d7-19b9431f4c36)
<br>
| 접두부 | 접미부 |
| :--: | :---: | 
|      |       | 
|  B   |  A    | 
|  BA  | AA    |
|  BAA | BAA   | 
|  BAAB | ABAA   | 
|  BAABA | BABAA   | 
|  BAABAB | ABABAA   |
|  BAABABA | AABABAA   | 
<br>

### 예시
본문:BAABAABAB
패턴:BAABAB
<br>
(1) 본문과 일치했던 패턴의 문자열까지 보기<br>
-> 0~4번 문자열 (BAABA) 까지는 서로 일치. 5번 문자에서 불일치
![image](https://github.com/silverlnng/DatastructureStudy/assets/112385982/11c938d6-be4c-46b1-a733-39f0d2d295af)

<br>
(2) 일치했던 패턴의 문자열까지 경계를 가지고 있는지 확인하기<br>
->
![image](https://github.com/silverlnng/DatastructureStudy/assets/112385982/5165077a-9224-43a3-91fe-1c3932b22cd9)
<br>

(3) 본문의 접미부 = 패턴의 접두부를 활용하기
-> 본문과 패턴의 [0~4] 는 서로 일치하기 때문에 경계도 서로 같다 .  
<br>

(4) 패턴을 일치하는 부분문자열 길이 에서 경계의 길이 만큼 뺀것만큼 이동시키고 불일치가 발견된 곳에서 검색을 재개
->일치하는 부분 문자열 BAABA 의 길이 (5) 에서 경계의 길이 (2)를 빼만큼 이동시키고 불일치가 발견된 5번 부터 검색을 재개
<br>
![image](https://github.com/silverlnng/DatastructureStudy/assets/112385982/7292746c-0f2b-4d86-bb54-a36cd02bbe67)
<br>

* 

* 본문의 길이가 N일 때, 최대 N번만큼만 비교를 수행하면 본문 내에서 패턴과 일치하는 문자열의 위치를 알아낼 수 있습니다

### 경계 정보를 미리 계산하기
BAABABAA 패턴

(1) 

이동거리 = 일치 접두부의 길이 - 최대 경계 너비

## KMP 알고리즘 구현부분

## 보이어 무어 알고리즘


