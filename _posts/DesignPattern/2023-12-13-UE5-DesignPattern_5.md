---
layout: single
title:  "C++_DesignPattern_5_옵저버 패턴"
categories: DesignPattern
tag: [UE5]
toc: true
toc_sticky: true
sidebar:
    nav: "counts"
---

# 옵저버 패턴
* 이벤트 기반 시스템에서 유용한 패턴으로, 상태 변화를 감지하고 이를 관찰하는 데 사용
* Unreal Engine의 이벤트 시스템에서 각종 상호작용을 처리하는 데 이 패턴을 활용
* 객체간의 일대다 의존성을 정의하는 디자인 패턴 .
* 클래스간의 의존성을 줄인다 = 디커플링 , 리스크 헷지

## 옵저버 패턴의 장점
   
* 느슨한 결합
   * 주체와 옵저버간의 느슨한 결합을 제공하여 객체간의 상호작용을 줄여준다 . 주체가 변경되어도 옵저버 객체에는 영향을 안주게 된다
* 확장성
   * 새로운 옵저버를 추가하는게 쉽다.
* 모듈성
   * 주체와 옵저버 둘다 독립된 모듈로 존재 . 재사용성이 용이하다

## 게임에서 구현 예시

* 이벤트 시스템
    * 플레이어의 행동이나 게임 내부 상태 변화 등의 이벤트가 발생했을 때, 해당 이벤트를 구독하는 옵저버들에게 알리는 시스템을 구현할 수 있습니다. 예를 들어, 플레이어가 아이템을 획득하면 이를 감지하여 UI를 업데이트하거나 다른 NPC의 상태를 변경하는 등의 작업을 수행할 수 있습니다.
       
* UI 업데이트
    * 게임 내부 상태가 변경되었을 때 UI를 업데이트하는데 옵저버 패턴을 활용할 수 있습니다. 예를 들어, 플레이어의 체력이 변경될 때 UI 상의 체력 바를 업데이트하는 등의 작업을 수행할 수 있습니다.
       
* AI 동기화
    * 게임 내의 AI 캐릭터들이 특정 상태에 도달하거나 특정 이벤트를 감지할 때, 다른 AI 캐릭터들의 상태를 업데이트하는데 옵저버 패턴을 활용할 수 있습니다. 예를 들어, 한 적 AI가 공격을 시작하면 주변의 다른 적 AI들도 이를 감지하여 동기화된 공격 동작을 수행할 수 있습니다.

   
## 옵저버 인터페이스
   
```cpp
// Fill out your copyright notice in the Description page of Project Settings.

#pragma once

#include "CoreMinimal.h"
#include "UObject/Interface.h"
#include "ObserverInterface.generated.h"

// This class does not need to be modified.
UINTERFACE(MinimalAPI)
class UObserverInterface : public UInterface
{
	GENERATED_BODY()
};

/**
 * 
 */
class THIRDPERSONTEMPLATE_API IObserverInterface
{
	GENERATED_BODY()

	// Add interface functions to this class. This is the class that will be inherited to implement this interface.
public:
	virtual  void NotifyAttack() = 0;
	//옵저버에게 공격 알림을 전달하는 함수
};

```
   

## 옵저버를 등록하는 쪽
   
```cpp
// Fill out your copyright notice in the Description page of Project Settings.

#pragma once

#include "CoreMinimal.h"
#include "Enemy.h"
#include "ObserverPattern/ObserverInterface.h"
#include "Goblin.generated.h"

/**
 * 
 */
UCLASS()
class THIRDPERSONTEMPLATE_API AGoblin : public AEnemy , public IObserverInterface
{
	GENERATED_BODY()
public:
	AGoblin();
	//AEnemy 클래스의 가상함수를 오버라이드 하여 구현
	virtual void Attack_Implementation() override;
	virtual void Defend_Implementation() override;

	//IObserverInterface 의 가상함수를 구현
	virtual void NotifyAttack() override;

	//옵저버 등록 및 알림전달 함수
	void RegisterObserver(IObserverInterface* Observer);
	
	void UnRegisterObserver(IObserverInterface* Observer);
private:
    // 옵저버들의 목록
	TArray<IObserverInterface*> observers;
};

```   
```cpp
// Fill out your copyright notice in the Description page of Project Settings.


#include "Goblin.h"

AGoblin::AGoblin()
{
}

void AGoblin::Attack_Implementation()
{
	//고블린의 공격기능 구현
	UE_LOG(LogTemp,Warning,TEXT("Goblin is attacking"));

	// 공격능력을 사용하면 옵저버에게 알림을 보내야한다
    // 옵저버 목록을 for문으로 모두 조회 
	for(IObserverInterface* observer : observers)
	{
		if(observer)
		{
			observer->NotifyAttack();
		}
	}
}

void AGoblin::Defend_Implementation()
{
	// 고블린 적의 방어 기능 구현 
	UE_LOG(LogTemp,Warning,TEXT("Goblin is defending"));
}

void AGoblin::NotifyAttack()
{
	
}

void AGoblin::RegisterObserver(IObserverInterface* Observer)
{
	// 옵저버 등록
	observers.AddUnique(Observer);
}

void AGoblin::UnRegisterObserver(IObserverInterface* Observer)
{
	//옵저버 제거
	observers.Remove(Observer);
}

```   