---
layout: single
title:  "UE5_TroubleShooting2"
categories: UE5
tag: [UE5]
toc: true
toc_sticky: true
sidebar:
    nav: "counts"
---

# GameModeBase c++작성시 Tick() 호출 문제
   
   
   
https://docs.unrealengine.com/5.2/en-US/API/Runtime/Engine/GameFramework/AGameMode/Tick/
   
공식 사이트에서 확인 해보면 "Function called every frame on this Actor. Override this function to implement custom logic to be executed every frame. Note that Tick is disabled by default, and you will need to check PrimaryActorTick.bCanEverTick is set to true to enable it. "      
으로 적혀있는걸 확인할 수 있다.
   
그래서    virtual void Tick(float DeltaSeconds) override; 함수를 제대로 사용하기 위해서는    
   
PrimaryActorTick.bStartWithTickEnabled = true;
PrimaryActorTick.bCanEverTick = true;
   
설정이 필수다.



