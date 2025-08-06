# ArenaBattleGAS

[이득우의 언리얼 프로그래밍](https://www.inflearn.com/course/%EC%9D%B4%EB%93%9D%EC%9A%B0-%EC%96%B8%EB%A6%AC%EC%96%BC-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D-part-4/dashboard) 강의의 실습 프로젝트입니다.
강의를 듣고 학습한 내용을 정리해두었습니다.

---

## 목차
  1. [Game Ability System](#GameAbilitySystemGAS)
  2. [Ability System Component](#AbilitySystemComponentASC)
  3. [Gameplay Ability](#GameplayAbility)
  4. [Attribute Set](#AttributeSet)
  5. [Gameplay Effect](#GameplayEffect)

---
## GameAbilitySystem(GAS)    

  - 액터가 사용할 수 있는 능력(Ability)과 액터 간의 상호작용 기능을 제공하는 프레임워크
  - 액터의 기능을 직접 확장하지 않고 모듈화된 능력을 통해 구현
  - 직접적인 로직 구현보다는 데이터를 기반으로 동작하도록 설계됨
  - 언리얼 네트워크 멀티플레이를 지원하도록 설계됨
  - 위 특징들 덕분에 유연성/확장성이 뛰어남


---

## AbilitySystemComponent(ASC)   

```
AABGASPlayerState::AABGASPlayerState()
{
	ASC = CreateDefaultSubobject<UAbilitySystemComponent>(TEXT("ASC"));
	AttributeSet = CreateDefaultSubobject<UABCharacterAttributeSet>(TEXT("AttributeSet"));
}
```
> AABGASPlayerState의 생성자, Multiplay환경을 고려해서 ASC를 캐릭터가 직접 가지고있지 않고 PlayerState에서 생성한다.

   - GAS 프레임워크를 관리, 처리함   
   - ASC를 통해 GAS 프레임워크와 액터가 상호작용   
   - GAS 프레임워크를 위한 코어 역할을 함   

---

## GameplayAbility

```
void AABGASCharacterPlayer::GASInputPressed(int32 InputID){
	FGameplayAbilitySpec* InputSpec = ASC->FindAbilitySpecFromInputID(InputID);
	if (InputSpec){
		InputSpec->InputPressed = true;
		if (InputSpec->IsActive()) ASC->AbilitySpecInputPressed(*InputSpec);
		else ASC->TryActivateAbility(InputSpec->Handle);
	}
}
```
> Player Character의 Input 처리 함수, 입력 ID를 ASC로 넘겨 GameplayAbility를 활성화시키는 역할을 함

   - 액터가 사용(발동) 가능한 능력   
   - 액터에 직접 기능을 추가하는 대신 사용가능한 능력을 분리된 모듈로서 구현   
   - 직접적인 확장 없이 공격, 점프, 스킬 사용 등 기능을 구현할 수 있음   
   - 각 기능들은 모듈화되어있어 재사용성이 높음   

---

## AttributeSet    

```
void UABCharacterAttributeSet::PostGameplayEffectExecute(const FGameplayEffectModCallbackData& Data)
{
	Super::PostGameplayEffectExecute(Data);
	......
	if (GetHealth() <= 0.f && !bOutOfHealth){
		Data.Target.AddLooseGameplayTag(ABTAG_CHARACTER_ISDEAD);
		OnOutOfHealth.Broadcast();
	}
	bOutOfHealth = GetHealth() <= 0.f;
}
```
> GameplayEffect 적용 이후 실행할 로직을 정의하는 PostGameplayEffectExecute, 체력이 0이 되었을 때 Delegate를 이용해 이벤트를 발생시킴

   - GAS에서 사용하는 캐릭터의 능력치 관련 구조체   
   - GameplayEffect에 의해 영향을 받음   
   - AttributeSet에 담긴 각 Attribute에 대해, 변화 이전/이후에 실행할 로직을 정의할 수 있음
   - 또한 GameplayEffect에 의한 영향 이전/이후에 실행할 로직을 정의할 수 있음

---

## GameplayEffect

   - 지속 피해 / 회복, 버프, 디버프 등 일시적/영구적으로 상태를 변화시키는 기능을 함
   - Instant : 상태변화가 딜레이 없이 즉시 적용되고 사라짐
   - Duration : 상태변화가 일정시간동안 적용됨
   - Infinite : 명시적으로 제거할 때까지 상태변화가 영구히 적용됨
   - 주기를 설정할 수 있으며 각 주기마다 어떻게 동작할 지 정의할 수 있음
   - Blueprint를 이용해 다양한 효과를 쉽게 구현할 수도 있음

---






