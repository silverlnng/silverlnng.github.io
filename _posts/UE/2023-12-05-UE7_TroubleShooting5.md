---
layout: single
title:  "UE5_TroubleShooting5"
categories: UE5
tag: [UE5]
toc: true
toc_sticky: true
sidebar:
    nav: "counts"
---
# 마우스 커서 아래 물체 탐색하기 문제
 정확하게 마우스 커서아래의 ground를 선택해야하는데 다른 ground가 선택되는 문제가 있었다.

##

```cpp
void AMainPlayerController::Clicked()
{
	FHitResult HitResult;
	GetHitResultUnderCursor(ECollisionChannel::ECC_Visibility, true
		, HitResult);
	
	clickedCrop = Cast<ACrop>(HitResult.GetActor());
	if(clickedCrop)
	{
		if(clickedCrop->myType==ECropProgressState::seedState)
		{
			target=clickedCrop;
		}
		
		if(clickedCrop->myType==ECropProgressState::cropState)
		{
			target=clickedCrop;
			GEngine->AddOnScreenDebugMessage(1, 30.f, FColor::Orange,"ECropProgressState::cropState");
			
			//인벤토리에 넣기
		}
	}		
}
```
   
GetHitResultUnderCursor 가 탐지하는 ECollisionChannel 을 ECC_Visibility으로 선택한 상태였다.
   
![image](https://github.com/silverlnng/UE_FarmGame/assets/112385982/4a181ee3-169e-4882-bc9b-c3e4192cca52)
   
이렇게 boxComponent가 겹쳐져있는 상태여서 ground mesh를 선택해도 boxComponent가 차지하고있는 대상을 선택하게 된다.

![image](https://github.com/silverlnng/UE_FarmGame/assets/112385982/963fc4fb-d14d-4c3b-b48b-be7c84df98d7)
   
(겹쳐져있고) 선택해야되지 않아야되는 boxComponent 에 대해서 ECC_Visibility에 대한 Traca Response를 ignore 으로 설정   


![image](https://github.com/silverlnng/UE_FarmGame/assets/112385982/c399dfed-5444-4eb4-8dff-dd6cefd0ac9e)
   
실제로 선택해야되는 mesh에 대해서는 ECC_Visibility에 대한 Traca Response을 block으로 설정
   
boxcomponent도 기본적으로 rendering-visible이 true로 설정되어있다.
boxcomponent는 화면에서 그려지지않아 ECC_Visibility에 대해서 반응이 될꺼라 생각을 못했던게 문제였다.   
문제 해결을 위해서 내가 의도한 선택되게 할 물체 와 실제로 (의도와 다르게) 선택되는 물체가 무엇인지를 정확하게 구별하고 어떤 연관성이 있는지 판단하기.
