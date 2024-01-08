---
layout: single
title:  "자료구조Study8_우선순위 큐"
categories: DataStructure
tag: [DataStructure]
toc: true
toc_sticky: true
sidebar:
    nav: "counts"
---


# 이진 탐색

핵심은 탐색범위를 1/2 씩 줄여나가는 방식에 있다.

## 수행하는 과정

1. 데이터 집합의 중앙에 있는 요소를 선택
2. 중앙 요소의 값과 찾고자 하는 목표값(target)을 비교
3. 목표값이 중앙요소의 값 보다 작으면 중앙을 기준으로 데이터집합의 왼편을 새로운 집합처럼 생각하고 탐색을 수행 ,    크다면 중앙을 기준으로 데이터집합의 오른쪽 편을 새로운 집합처럼 생각하고 탐색을 수행.
4. 찾고자 하는 값을 찾을 때 까지 1~3 번 과정을 반복.

## 성능측정

## 구현
