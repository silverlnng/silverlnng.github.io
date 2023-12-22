---
layout: single
title:  "C++_DesignPattern_프록시 패턴"
categories: DesignPattern
tag: [UE5]
toc: true
toc_sticky: true
sidebar:
    nav: "counts"
---

# 프록시 패턴

프록시는 접근을 제어하고 관리합니다.

프록시는 자신이 대변하는 객체와 그 객체에 접근하려는 클라이언트 사이에서 여러가지 방식으로 작업을 제어합니다.
   
## UMyConcreteResource를 UObject 클래스로 만든 이유

* Unreal Engine은 게임 오브젝트들을 UObject 클래스의 하위 클래스로 만들어 관리합니다. 이는 Unreal의 리소스 관리, 직렬화, 메모리 관리 등을 활용하기 위함

* 리소스나 게임 오브젝트들은 UObject를 상속받아 만들어지며, UObject를 상속받는 것은 Unreal의 특정 기능을 활용하기 위한 관습적인 방법

