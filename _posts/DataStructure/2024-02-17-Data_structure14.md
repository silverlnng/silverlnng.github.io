---
layout: single
title:  "자료구조Study14_문자열검색 알고리즘"
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
<span style="font-size:150%">패턴과 본문내의 문자열을 한 차례 비교하고 나면 , 다음 단계의 검색에서 사용할 수 있는 '어떤 정보'가 남으며 이 정보를 이용하면 불필요한 비교를 피할수있음을 이용</span> .{: .notice--success}

*

### 접두부 , 접미부 , 경계

## 보이어 무어 알고리즘
