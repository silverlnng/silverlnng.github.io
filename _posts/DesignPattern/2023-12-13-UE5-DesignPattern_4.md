---
layout: single
title:  "C++_DesignPattern_4_프록시 패턴"
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
   
프록시는 객체의 대리자 또는 대변자 역할을 수행하여, 클라이언트가 직접 접근할 수 없는 대상 객체에 대한 간접적인 접근을 제공합니다. 이를 통해 객체의 생성 및 초기화를 지연하거나, 접근 제어를 추가하거나, 리소스 사용을 최적화할 수 있습니다.

![image](https://github.com/silverlnng/VRFirstProject/assets/112385982/e9fb7029-c0f9-4be6-8fe5-dfab98ee934e)

## 언리얼 엔진에서 리소스 관리를 위한 프록시 패턴
언리얼에서는 맵이나 텍스처와 같은 대규모 리소스를 효율적으로 관리해야 할 때 프록시 패턴을 사용할 수 있습니다.

## ResourceInterface

```cpp
// Fill out your copyright notice in the Description page of Project Settings.

#pragma once

#include "CoreMinimal.h"
#include "UObject/Interface.h"
#include "MyResourceInterface.generated.h"

// This class does not need to be modified.
UINTERFACE(MinimalAPI)
class UMyResourceInterface : public UInterface
{
	GENERATED_BODY()
};

/**
 * 
 */
class THIRDPERSONTEMPLATE_API IMyResourceInterface
{
	GENERATED_BODY()

	// Add interface functions to this class. This is the class that will be inherited to implement this interface.
public:
	virtual void LoadResource () =0;
	virtual void UseResource () =0;
};

```


## ResourceProxy

```cpp
#pragma once

#include "CoreMinimal.h"
#include "MyResourceInterface.h"
#include "MyResurceProxy.generated.h"

/**
 * 
 */
UCLASS()
class THIRDPERSONTEMPLATE_API UMyResourceProxy : public UObject ,public IMyResourceInterface
{
	GENERATED_BODY()

private:
	class UMyConcreteResource* ActualResource; //실제리소스에대한 포인터
public:


	UMyResourceProxy(); 
	virtual void LoadResource() override;
	virtual void UseResource() override;

	~UMyResourceProxy();	//소멸자에서 실제 리소스를 정리 
};


```

```cpp
// Fill out your copyright notice in the Description page of Project Settings.


#include "ProxyPattern/MyResourceProxy.h"
#include "ProxyPattern/MyConcreteResource.h"

UMyResourceProxy::UMyResourceProxy()
{
	ActualResource = nullptr;
}

void UMyResourceProxy::LoadResource()
{
	if(!ActualResource)
	{
		ActualResource = NewObject<UMyConcreteResource>();	//실제 리소스를 생성
		ActualResource->LoadResource();
	}
}

UMyResourceProxy::~UMyResourceProxy()
{
	if(ActualResource)
	{
		ActualResource->ConditionalBeginDestroy();
		ActualResource =nullptr;
	}
}

void UMyResourceProxy::UseResource()    //
{
	if(!ActualResource)
	{
		LoadResource();
	}
	ActualResource->UseResource();
}


```


## 실제 리소스 클래스
UMyConcreteResource

## 리소스를 실제 사용할때 

![image](https://github.com/silverlnng/VRFirstProject/assets/112385982/c30db967-254d-4850-b68c-3048493f96f4)


## UMyResourceProxy , UMyConcreteResource를 UObject 클래스로 만든 이유

* Unreal Engine은 게임 오브젝트들을 UObject 클래스의 하위 클래스로 만들어 관리합니다. 이는 Unreal의 리소스 관리, 직렬화, 메모리 관리 등을 활용하기 위함

* 리소스나 게임 오브젝트들은 UObject를 상속받아 만들어지며, UObject를 상속받는 것은 Unreal의 특정 기능을 활용하기 위한 관습적인 방법

