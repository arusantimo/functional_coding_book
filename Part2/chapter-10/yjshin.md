
## 일급함수 1

> ❗     
> 암묵적 인자를 드러내기   
> 함수 본문을 콜백으로 바꾸기

함수 이름에 있는 암묵적 인자    
값을 명시적으로 전달하지 않고 함수 이름의 일부로 전달하고 있다.

 예제 코드
```javascript
// 암묵적 인자 : price 
function setPrice(obj) {
    const newItem = obj['price']
}

// 암묵적 인자 : quantity
function setQuantity(obj) {
    const newItem = obj['quantity']
}
```

### 리팩터링 : 암묵적 인자를 드러내기
price, quantity 와 같은 필드를  일급으로 만들어보자

**일급 값 (first-class value)** 은 언어에 있는 다른 값 처럼 쓸 수 있다.

```javascript 
// 필드명을 일급 값으로 만들었다. 
const fieldsName = ['price', 'quantity']

const translations = {'quantity' : 'number'}

const excludeFields = ['size']

function setFieldByName(obj, field) {
    if (!fieldsName.includes(field)) {
        throw Error('not found')
    }

    // p246 연습문제
    if (excludeFields.includes(field)) {
        throw Error('cannot be run something...')
    }
    
    if (translations.hasOwnProperty(field)) {
        field = translations[field]
    } 

    const newItem = obj[field]
}

const price = setFieldByName({...}, 'price')
const quantity = setFieldByName({...}, 'quantity')
```


일급으로 할 수 있는것
1. 변수에 할당 
2. 함수의 인자로 넘기기
3. 함수의 리턴값으로 받기
4. 배열이나 객체에 담기

### 데이터 지향
데이터를 배열에 할당하는 것과 같은 간단한 작업이라도 성능을 고려하면 프로그래밍 방식에 대해 신중하게 고민해야 한다.
이런 데이터의 성능을 중점으로 설계를 고민하기 시작했고, 그렇게 생겨난 설계 방식 중 하나가 바로 데이터지향설계(Data-Oriented Design, DOD)다.
https://medium.com/monday-9-pm/%EC%B4%88%EB%B3%B4%EA%B0%9C%EB%B0%9C%EC%9E%90-%EB%8D%B0%EC%9D%B4%ED%84%B0%EC%A7%80%ED%96%A5-%EC%84%A4%EA%B3%84-data-oriented-design-%EC%95%8C%EC%95%84%EB%B3%B4%EA%B8%B0-c0bbd36ea9da


### 정적타입 동적타입
정적 타입 : 컴파일할 때 타입 검사를 하는 언어
동적 타입 : 런타임에 타입을 확인하는 언어

### 반복문 예제: 먹고 치우기
리팩터링 단계 (p253~256)
1. 함수 이름에 암묵적 인자를 드러낸다.     
    1. food,dish 제거
   2. operateOnArray()
2. 명시적인 인자를 추가
    1. function (**array**) {}
   2. operateOnArray(arraym, **f**) {}
3. 함수 본문 코드를 인자값으로 수정
   1. food, dish 사용하는 곳을 찾아 array item으로 수정
   2. f(...)
4. 함수를 호출하는 곳을 변경
   1. 적절한 인수값으로 수정
   2. operateOnArray(foods, cookAndEat)


### 함수 본문을 콜백으로 바꾸기(고차함수로 바꾸기)
**고차함수**란 인자로 함수를 받거나 리턴 값으로 함수를 리턴할 수 있는 함수 (12장 자세히..)

```javascript
try {
    // saveUser
} catch (error) {
    logError(error)
}

try {
    // removeUser() // 본문
} catch (error) {
    logError(error)
}

function withLoggin(f) {
    try {
        f()
    } catch (error) {
        logError(error)
    }
}

/*
* 함수 본문을 함수로 감싸서 넘기는 이유
* 실행 시점을 사용자에게 넘길 수 있음
* */
withLoggin(function () { saveUser() })

```
