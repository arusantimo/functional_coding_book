# 변경 가능한 데이터 구조를 가진 언어에서 불변성 유지하기

## Copy-on-write

- 읽기는 데이터가 바뀌지 않기 때문에 다루기 쉬움
- 쓰기 동작은 불변성 원칙에 따라 구현

### Copy-on-write 원칙

1. 복사본 만들기
2. 복사본 변경하기
3. 복사본 리턴하기

> Copy-on-write는 쓰기 동작을 읽기로 바꾼다\
> (데이터를 변경하지 않고 정보를 리턴했기 떄문)

## 쓰기를 하면서 읽기도 하는 동작에 적용

### Solution

1. 읽기/쓰기 함수를 분리하기(best)
2. 두개의 값을 리턴하기

연습문제) Array.pop() 동작을 Copy-on-write 동작으로 변경하기

```javascript
// 1. 읽기 함수와 쓰기 함수 분리
function last_item(array) {
  return array[array.length - 1];
}
function drop_last(array) {
  const new_array = array.slice();
  new_array.pop();
  return new_array;
}

//---2.값 두개를 리턴하는 함수
function pop(array) {
  const new_array = array.slice();
  const last_item = new_array.pop();
  return {
    last: last_item,
    array: new_array,
  };
}
```

연습문제) add_contact 함수를 copy-on-write 버전으로 리팩토링

```javascript
function push(array, elem) {
  const new_array = array.slice();
  new_array.push(elem);
  return new_array;
}
function add_contact(mailing_list, email) {
  return push(mailing_list, email);
}
```

연습문제)arraySet() 함수 만들기 (배열 index 에 값 설정하는 함수)

```javascript
function arraySet(array, index, value) {
  const new_array = array.slice();
  new_array[index] = value;
  return new_array;
}
```

## 액션과 계산

- 변경 가능한 데이터를 읽기 → 액션
  - 읽는 시점마다 결과가 다르기 때문
- 쓰기는 데이터를 변경 가능한 구조로 바꿈
- 데이터에 쓰기가 없다면 ? → 변경 불가능한 데이터
- 불변데이터 읽기 → 계산
- 쓰기를 읽기로 바꾸면 계산이 많아진다!

Q. 구조적 공유란

> 두 개의 중첩된 데이터 구조가 어떤 참조를 공유한다면 구조적 고유(structural sharing)라고 함. 데이터가 바뀌지 않는 불변 데이터 구조라면 구조적 공유는 안전. 메모리를 적게 사용하고, 모든 것을 복사하는 것보다 빠름

- Array.slice()의 경우 얕은 복사를 수행 (참조에 대한 복사본)
- 배열 항목 내 값을 변경하는 경우, 해당 값(객체)의 복사본을 생성하고,
- 변경이 없는 경우에는 같은 참조를 가리키게 된다. → 이것이 구조적 공유

* 배열의 spread operator 는 1차원 배열인 경우에 깊은복사! (그럼 구조적 공유가 안되는건가??)
