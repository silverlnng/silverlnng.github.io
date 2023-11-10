---
layout: single
title:  "UE5_TroubleShooting1"
categories: UE5
tag: [UE5]
toc: true
toc_sticky: true
sidebar:
    nav: "counts"
---
   
# 애니메이션 몽타주 Preview 열기 에러
preview 를 작동시키지마자 error가 생기면서 리포터에 몽타주가 사용하고 있는 클래스가 나올수있다.    

![image](https://github.com/silverlnng/UE_ThirdPersonTemplate/assets/112385982/5178f17f-b7b7-43f3-a2f6-5f0e7a99c82c)

그이유는 애니메이션 몽타주 preview 에서도 몽타주에서 사용하고있는 클래스가 동작하고 있기 때문이다 . (빨간박스 처럼 로그가 뜨는걸 확인할수있다.)   

그래서 해당 애니메이션 몽타주 에서 사용하는 UNotifyStateFire 클래스의 함수에서 문법적으로 오류도없고 플레이도 정상작동은 되지만 preview 환경에서는 null값이 나오는경우 에러가 생긴다.      
이러한 경우를 예방하기 위해서 preview 환경에서는 null이 나올 수 있는 경우를 생각해서 null값을 체크를 하는 방어 코드를 작성한다.

![image](https://github.com/silverlnng/UE_ThirdPersonTemplate/assets/112385982/4e5c1e47-561e-4d65-bfe9-25ac268b2e6e)

   


```cpp
void UNotifyStateFire::NotifyBegin(USkeletalMeshComponent* MeshComp, UAnimSequenceBase* Animation, float TotalDuration, const FAnimNotifyEventReference& EventReference)
{
	/Notify가 불려지면 시작 첫(1) 프레임
	GEngine->AddOnScreenDebugMessage(-1, 5.0, FColor::Green, TEXT("NotifyBegin"));
	//GEngine->AddOnScreenDebugMessage() : 블루프린트에서 printString

	tpsPlayer = Cast<ATPSPlayer1>(MeshComp->GetOwner());
	// 여기서 MeshComp는 애니메이션을 플레이하는 SkeletalMeshComponent가 매개변수로 들어온다 
	// 문법적오류도없고 , 플레이에서는 정상적으로 cast되어 작동한다. tpsPlayer에 정상적으로 값이들어가서 null체크를 하지 않고도 tpsPlayer->SpawnBullect(); 를 해도 정상작동.
	// preview 환경에서는 tpsPlayer 찾지를 못한다 .그래서 cast 한다음 tpsPlayer은 null값이다.

	if (tpsPlayer)      // 그래서 null 체크 해주기 
	{
		tpsPlayer->SpawnBullect();
	}
}
```