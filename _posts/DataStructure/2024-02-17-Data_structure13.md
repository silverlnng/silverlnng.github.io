---
layout: single
title:  "자료구조Study13_문자열검색 알고리즘"
categories: DataStructure
tag: [DataStructure]
toc: true
toc_sticky: true
sidebar:
    nav: "counts"
---

# 문자열 검색
<br>
문자열 안에 존재하는 특정 단어를 빠르게 찾아내는 것이 목적
<br>
* 본문 : 탐색대상이 되는 문자열
* 패턴 : 검색어를 의미
* 이동 : 본문에서의 탐색위치를 옮기는 것
<br>

## 고지식한 검색(Naive Search ,Brute Force Search)

## 카프-라빈 알고리즘 
<br>
* 해시함수를 이용.
* 패턴의 해시값 , 본문안에 있는 하위 문자열의 해시값만 비교 
<br>

![image](https://github.com/silverlnng/DatastructureStudy/assets/112385982/09fa0f4b-f1ca-4ac9-b4dd-1b560efc6dfa)


### 해시 함수의 비용을 줄이기
<br>
![image](https://github.com/silverlnng/DatastructureStudy/assets/112385982/3e83d398-8d3f-46bc-8db4-65daca2d0af5)
<br>
* H1 은 S[0] (제거된 문자) ,S[4] (새로운 문자) 만 이용해서 구할 수 있게 식이 간추려 진다.

* 새로운 해시함수 정의 
<br>
![image](https://github.com/silverlnng/DatastructureStudy/assets/112385982/2eaba2ef-86a3-41f0-8444-9cad8d1ceabf) 

* 이 해시함수로 총 탐색시간이 패턴의 길이와 상관없고 본문의 길이에만 영향을 받게되어 탐색성능이 훨씬 향상
<br>
* 다만 새 해시 함수는 i가 0일 때, 즉 이전 해시 값이 미리 구해지지 않은 경우에는 사용할 수 없으므로 최초의 해시 값올 구할 때는 기존의 해시 함수를 사용.

### 해시값 일정범위 안에 가두기
<br>
* 문자열의 길이가 늘어나면 해시값도 따라 커진다는 문제 -> 해시값을 일정 범위 안에 가두기
<br>
* 해시 함수 결과를 특정 값 ("q") 으로 나눈 나머지를 해시값으로 사용
<br>
![image](https://github.com/silverlnng/DatastructureStudy/assets/112385982/79331f4e-5525-4a2d-95fd-43d0a508b7e3)


### 예시
<br>
본문:ABACCEFABADD
<br>
패턴:CCEFA
<br>
q의 값 : 2147483647(int의 최대값)
<br>
<br>
(1) 패턴의 해시값과 본문[0~4] 의 해시값을 구하기. 맨 처음 단계는 패턴이나 본문 모두 활용할수있는 '이전 해시값' 는 상태로  
![image](https://github.com/silverlnng/DatastructureStudy/assets/112385982/9e4b7de9-6a38-4b3a-a226-47e0b1b01186)
다음의 해시함수 활용.
<br>
![image](https://github.com/silverlnng/DatastructureStudy/assets/112385982/fc548a85-4831-414a-8947-7c908043bedd)
<br>
<br>
(2) 이제는  본문[0~4]의 해시값이 있어서 활용할수 있는 단계. 
![image](https://github.com/silverlnng/DatastructureStudy/assets/112385982/a71c84e1-5071-4360-9666-22085668b567)
다음의 해시함수 사용
<br>
<br>
![image](https://github.com/silverlnng/DatastructureStudy/assets/112385982/ec1c418e-eeb2-413c-b4f9-78d63186632b)


### 카프-라빈 알고리즘 구현 부분

