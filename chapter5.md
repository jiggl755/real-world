## TDD로 새로운 설계문제를 풀어나가는 방법

### TDD : 테스트코드를 먼저 만든 후 이에 맞춰 코드를 구현

- 필요한 요구사항에 집중하고 개선
- 코드를 올바르게 조직
- TDD 주기에 따라 요구사항 구현을 반복하며 요구사항을 만족시켰다는 사실을 개런티할수있으며 버그 발생 범위를 줄일 수 있다
- 필요하지않은 테스트를 구현하는 일을 줄일수있다

### TDD 주기

1. 실패하는 테스트 구현
2. 모든 테스트 실행
3. 기능이 동작하도록 구현
4. 모든 테스트 실행

※ 실생활에서는 코드의 리팩토링이 필수이니 테스트 실행 이후 리팩토링을 실시한다


---

## 유닛테스트를 구현하는데 유용한 모킹 기법

### 모킹 : run() 이 실행되었을 때 이를 확인하는 기법 

- 비지니스 규칙에 액션을 추가할 때마다 run()이 실행되었는지 확인
1. 목생성
2. 메서드가 호출되었는지 확인

mock() 메서드에 Action객체를 인수로 전달하면 유닛 테스트로 Action의 목 객체를 만들수있음

---

## 최신 자바 기능

### 조건 추가하기
만약 조건이 테스트시 들어가야한다면 하드코딩으로박으면 기능코드가 독립적으로 돌아갈수없다
Facts 라는 새 클래스를 만들자

### 지역변수 형식 추론 : 컴파일러가 정적 형식을 자동으로 추론해 결정하는 기능

> Map<String, String> map = new HashMap<String, String>();

을

> Map<String, String> map = new HashMap<>();

로 사용 가능 다이아몬드 연산자라는 기능으로 자바7에 추가 
앞에 형식이 지정되어있으면 제네릭의 형식 파람을 생략가능
자바10에서는 이게 지역변수까지 확장 적용

> Facts env = new Facts();
> 
> BusuinessRuleEngine businessRuleEngine = new BusinessRuleEngin(env);

> var env = new Facts();
>
> var businessRuleEngine = new BusinessRuleEngin(env);

var 라는 키워드를 사용했으나 뒤에 들어가는 형식을 통해 env화 businessRuleEngine 이 각각 무슨 형식인지 알수있다
var는 final과 다르다 다시 재선언할수있다.

### swich문

- 기존 스위치문
```
swtich(dealStage){
    case LEAD : 
        foreacastedAmount = amount * 0.2;
        break;
    case EVALUATING :
        foreacastedAmount = amount * 0.5;
        break;
    case INTERESTED : 
        foreacastedAmount = amount * 0.8;
        break;
    case CLOSE :
        foreacastedAmount = amount;
        break;
}
```
- break를 빼먹으면 폴스루 모드로 실행되며 버그가될수있다

- 새로운 스위치문
```
var foreacastedAmount = amount * switch (dealStage){
    case LEAD -> 0.2;
    case EVALUATING -> 0.5;
    case INTERETED -> 0.8;
    case CLOSE -> 1;
}
```
- 가독성 올라감
- 소모검사도 이루어진다 enum에 switch를 사용하면 자바 컴파일러가 모든 enum 값을 switch에서 소모했는지 확인

---

## 빌더패턴과 인터페이스 분리원칙으로 사용자 친화적인 API 개발 방법.
실제 액션을 수행하지 않고도 각 액션과 관련된 조건을 기록해야한다 how?

### 인터페이스 분리원칙 ISP
- 어떤 클래스도 사용하지 않는 메서드에 의존성을 갖지 않아야한다 >> 불필요한 결합을 만들지 않기 위해
- ISP는 SRR과 달리 설계가아닌 사용자 인터페이스에 초점을 둔다
- 인터페이스가 커지면 결국 사용자는 사용하지 않는 기능을 갖게되며 이는 불필요한 결합도를 가지게된다

?? 필요한 이상의 기능을 가진 인터페이스와 결합하여 인터페이스의 이불에 의존하게되었다는건가?

### 플루언트 API 
- 특정문제를 더 직관적으로 해결할 수 있도록 특정 도메인에 맞춰진 API를 가리킨다. 
- 메서드 체이닝을 이용하면 복잡한 연산도 지정할수있다. 
- 대표적으로 Stream

### 도메인 모델링
- 어떤조건이 주어졌을때(when) 이런 작업을 한다(then)

- 조건 : 어떤 팩트에 적용할 조건
- 액션 : 실행할 연산이나 코드 집합
- 규칙 : 조건과 액션을 합친것, 조건이 참일때만 액션 실행

### 빌더패턴
- 객체를 반드는 방법을 재공
- 생성자의 파라미터를 분해해서 각각의 파라미터를 받는 여러 메서드로 분리


---
### 비지니스 규칙

- 팩트 : 규칙이 확인할수있는 정보
- 액션 : 수행하려는 동작
- 조건 : 액션을 언제 발생시킬지 지정 
- 규칙 : 실행하려는 비지니스 규칙을 지정
---

