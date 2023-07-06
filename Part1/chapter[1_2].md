# 챕터  


- 함수형 사고란 무엇인가.
- 다른 책과 차이점은 무엇인가.
- 함수형 프로그래머가 코드를 바라보는 방법은?
- 이책을 게속 볼것인가.

## 함수형 프로그래밍이란  


> `수학함수`(`순수함수`)를 사용하고 `부수효과`를 줄이는(피하는) 것을 목표로 한다.

`부수효과`는 함수가 계산하여 계산된 새로운 값을 내보내는 것 이외의 다른 모든 일이다.   
메일 보내기, 로그인하기, 서버의 값 받아오기, 전역 상태 바꾸기, 외부 IO 접근 등등 있다.

## 현실의 프로그래밍에서의 문제점  

우리는 개발하다보면 무수히 많은 부수효과를 만들어 내면서 코딩을 해야만 한다,  
우리가 하는것은 일반 수학식 처럼 하나의 결과만을 만들어 내는것이 아니기 때문.!  

1. 부수효과는 필수 불가결하다.
2. 함수형 프로그래밍은 부수효과를 핸드링하기 위함이다.
3. 의외를 함수형 프로그래밍은 실용적이다.

이책은 함수형 프로그램의 학술적이고, 난해하고 추상적인 개념을 배제함.  
총 3가지 요소로 설명함
- `액션`
- `계산`
- `데이터`

```js
user = {
  "name": "arusnatimo"
} // [데이타]사람에 대한 정보 

sendEmail(to, from, title, cotnent) // [액션] 이메일 보내기

sum(a, b) // [계산] 인자로 넘어온 두개의 수를 더한 값을 내보내기

saveUserToDB(user) // [액션] 데이터 베이스에 유저 정보 저장

cutFirstName(fullName) // [계산] 인자로 넘어온 이름을 잘라서 내보내기

getUserInfo(userID) // [액션] 유저 아이디로 유저의 정보를 조회하여 내보내기 ** 이건 외부정보에 의존적인 함수이기 때문에 액션이라고 할수 있음.
```

### 계산.  
어떤 상황이던 같은 인자값이면 같은 결과를 내보내야 한다. (순수함수)
함수 실행에 따라 반환값 이외에 어떤 영향도 끼칠수 없어야 한다. (테스트가 무조건 되어야 한다.)
멱등성을 보장한다.

### 액션  
액션을 요청하는 시점과 상황에 따라 같은 인자를 넘겨줘도 액션의 결과가  같지 않다.  
외부의 상황과 실행 횟수에 의존적이다.(메세지 10번 보내는것 !== 1번 보내는것)   
(테스트가 권장되나, 필수가 아님, 의존성 설계가 어렵기 때문, 통과하기 위한 임의의 상태를 만들어 내는 테스트를 위한 테스트가 되어 버리기도함.)

### 데이터  
그냥 데이터임. 자료구조 의미적으로는 사실에 대한기록  
실행 전 후 에대한 추적이 필요 없기때문에 파악하기 쉽다.

---

우선 코드를 작성하거나 분석함에 있어서 가장 처음에 할것이 이 세개로 분류하는것임.

그리고 핸들링 하기 좋은 방향은 액션보다 계산이 많으면 결과를 예측하기 쉽다. 그리고 계산보다 데이터는 구조가 보이기 때문에 계산보다 예측하기 쉽다.

어떤 기능에 대해서 액션과 데이터 계산을 구분해본다.

1.사용자가 인풋에 포커싱한다. -> `액션` 인풋(이벤트타겟)과 브라우저(사용자) 상태(리스너) 에 의존적이다.  
2.메세지를 수신한다. -> `액션` 수신자 송신자 상태에 의존적이다.  
3.전송된 메세지와 수신자 목옥을 받아 필수 수신자 리스트를 반환한다. -> `계산` 수산자 목로과, 전송된 메세지가 인자로 전달되어 그 상태를 받아 처리하기 때문에 항상 같은 결과를 내보낸다.  
4.메세지를 송신한다. -> `액션` 같은 수신자에게 몇번 보낸는지를 체크해야 하는 의존성이있기 때문에 송신의 갯수나 형태가 의존적이라서, 액션이다.

## 요소마다 우리가 배워야 할것

### 액션

- 외부 요인(시간, 외부 상태)에 영향에서도 안전하게 액션(상태 변경, 메세지 발송등)을 처리하는 방법
- 액션의 순서를 보장하는 방법
- 액션의 횟수를 컨트롤 하는 방법
- `시간` `타이밍`에 의존적이 때문에 다루기 어렵다. 바꿔 말해 실행 지점의 외부 상태

### 계산

- 계산의 효율성 (로지칼 지식)
- 정확성
- 테스트 전략

### 데이터

- 효율적인 처리(조회, 변경)를 위한 데이터 구조 전략
- 데이터 보관 기술
- 데이터 구조를 통한 소프트웨어 스팩정리

## 뭐가 좋아지나 어디에 주로 쓰나 

주로 분산 시스템(멀티스레드), 비동기 프로그래밍, 메타 프로그래밍등

복잡한 소프트웨어에 쓰임.  주로 하나의 액션으로 파생되는 다양한 액션과 상태변화가 있을때, 순서가 섞이거나, 상태가 섞여서 원하는 결과를 못 만들어낼때 개선할 수 있다.  
한마디로 시간에 따라 바뀌는 값을 모델링 할수 있다.

데이터와 계산은 실행 순서, 외부 상태, 실행 시간, 횟수등 외부 요소와 무관하게 항상 같은 결과를 가져다 준다.
그렇기 때문에 최대한 두개의 요소를 사용해서 문제를 해결하려고 전략을 구상해야한다.  

액션은 소프트웨어에 꼭 필요한 요소이지만 버그를 만들어내는 원인(외부 요소에 의존적)이 되기때문에  
최대한 격리 시키는 전략을 짜야한다.  








