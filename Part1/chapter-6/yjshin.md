# 6. 변경 가능한 데이터 구조를 가진 언어에서 불변성 유지하기

> Copy on write : 불변성   
> 쓰기가 없다면 데이터는 불변형이다.


 ### 읽기와 쓰기
- 읽기 : 데이터를 바꾸지 않고 정보를 꺼내는 것  
  ex) 사용자 정보 확인하기, 장바구니 확인 하기
- 쓰기 : 데이터를 바꾼다.   
  ex) 회원 정보 수정하기, 장바구니 삭제하기 
  
쓰기 동작은 불변성 원칙에 따라 구현해야한다.

### 카피 온 라이트 원칙(패턴)
복사본 만들기 - 복사본 변경하기 - 복사본 리턴하기
```javascript
function setPriceByName(cart, name, price) {
  var cartCopy = cart.slice(); // 복사본 만들기
  for(var i = 0; i < cartCopy.length; i++) {
    if(cartCopy[i].name === name)
      cartCopy[i] = setPrice(cartCopy[i], price); // 복사본 변경하기
  }
  return cartCopy; // 복사본 리턴하기 
}
```


### 쓰기도 하면서 읽기도 하는 동작
```2. 값을 두 개 리턴하기``` 는 첫 번째 요소의 값을 추출하려면 shift() 호출이 불가피 하므로 ``` 1. 함수를 분리하기```를 사용하는 것이 유지보수 및 재활용 측면에서 용이하다.

```javascript
// 읽기도 하면서 쓰기도 하는 예시 
const array = [1, 2, 3, 4, 5]
const first_element = array.shift()

// 1. 함수를 분리하기
function first_element (array) {
    return array[0]
}

function drop_first (array) {
  var array_copy = array.slice();
  array_copy.shift();
  return array_copy
}

// 2. 값을 두 개 리턴하기
function shift(array) {
  var array_copy = array.slice();
  var first = array_copy.shift();
  return {
    first : first,
    array : array_copy
  };
}
```

### 불변 데이터 구조를 읽는 것은 계산이다.
쓰기가 없다면 데이터는 불변형이다.

> __액션과 계산__    
> 액션: __변경 가능한 데이터를 읽는 것__, 부수 효과, 부수 효과가 있는 함수, 순수하지 않은 함수    
> 계산: __변경 불가능한 데이터를 읽는 것__, 순수 함수, 수학 함수



### 바뀔 때마다 복사를 하면 너무 비효율적인 것이 아닌가요?
- GC 는 매우 빠르다.
- 복사를 하더라도 얕은복사를 한다. 메모리를 가리키는 참조 복사에 대한 복사만 한다(=구조적 공유 structural sharing p.134,142)
