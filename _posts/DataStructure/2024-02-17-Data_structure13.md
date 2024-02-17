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

문자열 안에 존재하는 특정 단어를 빠르게 찾아내는 것이 목적

* 본문 : 탐색대상이 되는 문자열
* 패턴 : 검색어를 의미
* 이동 : 본문에서의 탐색위치를 옮기는 것

## 고지식한 검색(Naive Search ,Brute Force Search)

## 카프-라빈 알고리즘 

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

* 


### 카프-라빈 알고리즘 구현 부분