# ArenaBattleGAS

[이득우의 언리얼 프로그래밍](https://www.inflearn.com/course/%EC%9D%B4%EB%93%9D%EC%9A%B0-%EC%96%B8%EB%A6%AC%EC%96%BC-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D-part-4/dashboard) 강의의 실습 프로젝트입니다.
강의를 듣고 학습한 내용을 정리해두었습니다.

---

## 목차
  1. [Game Ability System](#GameAbilitySystem(GAS))
  2. [Ability System Component](#AbilitySystemComponent(ASC))

---
## GameAbilitySystem(GAS)   



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

---

   - Character - Notify, Character - UI, Character - Item 등 다양한 객체간 통신을 위해 사용   

