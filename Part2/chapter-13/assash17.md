# Chapter 13. 함수형 도구 체이닝
> 함수형 도구(filter, map, reduce ...) 등을 조합(chain)해서 문제를 해결해보자.

---

### 요청 사항: 우수 고객(3개 이상 구매)들 각각의 가장 비싼 구매를 구해보기 (318 page)
#### 단계를 나누어 보자
1. 우수 고객(3개 이상 구매) 구하기
   - filter 사용
3. 우수 고객의 구매 목록 중 가장 비싼 구매 구하기
   - map (우수 고객 배열 전체 돌리기 용) + reduce (각 우수 고객의 가장 비싼 구매 찾기 용)
#### map 콜백 안에서 reduce가 수행되므로 코드가 보기 어렵다
 - 책에서 해결한 방법: 객체 배열에서 특정 키값의 max값을 찾는 maxKey함수를 만들어 해결

> 단계를 나누어서 해결한다. 복잡해지면 단순하고 명확하게 할 방법을 찾아보자.

###### 연습문제
---

### 체인을 명확하게 만드는 방법 2가지 (324 page)
 - 단계에 이름 붙이기
 - 콜백에 이름 붙이기

#### 콜백에 이름 붙이기가 재사용성이 더 좋다.
 - why? 더 일반적이고, 더 작은 기능이다.

#### 예제: 한 번만 구매한 고객의 이메일 목록
1. 한 번만 구매한 고객 (filter)
2. 이메일만 추출 (map)

> 고차 함수 (filter, map...)등의 콜백을 함수화 시켜 재사용하자

###### 연습문제
 - hasBigPurchase는 왜 필요한가 isBigPurchase만 사용하면 되지 않나?
---

### 331 page
 - filter, map, reduce등을 여러번 사용할 때 한번 사용하는 것으로 최적화 시킬 수 있다.
 - but, 병목이 생겼을 때만 사용하자 (각각의 단계를 명시하는게 명확하고 읽기 쉽다.)

---

### 기존의 반복문 코드를 함수형 도구(filter, map, reduce ...)로 리팩토링 (332 page)
 - 전체를 이해하고 다시 만드는 방법이 있다.
 - 단서를 찾아 본다.
   - 배열 전체를 반복하는가? (함수형 도구를 사용하기 좋은 단서)
   - 배열화 시켜 사용 (범위값 => 배열화)
   - 책의 예제들은 index가 없으므로, (자바스크립트에는 콜백에 2번째 인자로 존재) index를 배열화 시켜서 사용한다. 

#### 연습문제 339page
1. type을 filter한다. (filter)
2. numberInInventory만 빼낸다. (map)
3. numberInInventory들의 합산을 구한다. (reduce)

---

### 다양한 함수형 도구 (341 page)
 - filter, map, reduce, some, every, forEach, sort, join, find ...
##### Lodash 함수 설명

1. **_.compact**
    - 설명: 배열에서 falsy 값들을 제거합니다.
    ```javascript
    _.compact([0, 1, false, 2, '', 3]);
    // => [1, 2, 3]
    ```

2. **_.difference**
    - 설명: 첫 번째 배열에서 두 번째 배열의 요소를 제거합니다.
    ```javascript
    _.difference([2, 1], [2, 3]);
    // => [1]
    ```

3. **_.differenceBy**
    - 설명: 첫 번째 배열의 요소와 두 번째 배열의 요소를 비교 함수나 속성 이름으로 비교하여 제거합니다.
    ```javascript
    _.differenceBy([2.1, 1.2], [2.3, 3.4], Math.floor);
    // => [1.2]
    ```

4. **_.differenceWith**
    - 설명: comparator 함수를 사용하여 배열의 값들을 비교하고 첫 번째 배열의 값들 중 두 번째 배열의 값들과 다른 값을 반환합니다.
    ```javascript
    const objects = [{ 'x': 1, 'y': 2 }, { 'x': 2, 'y': 2 }];
    _.differenceWith(objects, [{ 'x': 1, 'y': 2 }], _.isEqual);
    // => [{ 'x': 2, 'y': 2 }]
    ```

5. **_.flatten**
    - 설명: 중첩된 배열을 한 레벨로 평탄화합니다.
    ```javascript
    _.flatten([1, [2, [3, [4]], 5]]);
    // => [1, 2, [3, [4]], 5]
    ```

6. **_.flattenDeep**
    - 설명: 배열을 완전히 평탄화합니다.
    ```javascript
    _.flattenDeep([1, [2, [3, [4]], 5]]);
    // => [1, 2, 3, 4, 5]
    ```

7. **_.flattenDepth**
    - 설명: 지정된 깊이까지 배열을 평탄화합니다.
    ```javascript
    _.flattenDepth([1, [2, [3, [4]], 5]], 2);
    // => [1, 2, 3, [4], 5]
    ```

8. **_.intersection**
    - 설명: 모든 배열에 있는 공통 요소를 반환합니다.
    ```javascript
    _.intersection([2, 1], [4, 2], [1, 2]);
    // => [2]
    ```

9. **_.intersectionBy**
    - 설명: 여러 배열의 공통 요소를 반환하는데, 비교하는 값을 반환하는 iteratee를 사용합니다.
    ```javascript
    _.intersectionBy([2.1, 1.2], [4.3, 2.4], Math.floor);
    // => [2.1]
    ```

10. **_.intersectionWith**
    - 설명: comparator 함수를 사용하여 배열의 값들을 비교하고 모든 배열에 있는 공통 값을 반환합니다.
    ```javascript
    const objects = [{ 'x': 1, 'y': 2 }, { 'x': 2, 'y': 2 }];
    _.intersectionWith(objects, [{ 'x': 1, 'y': 2 }], _.isEqual);
    // => [{ 'x': 1, 'y': 2 }]
    ```

11. **_.union**
    - 설명: 여러 배열의 요소를 결합하고 중복 요소를 제거합니다.
    ```javascript
    _.union([2, 1], [4, 2], [1, 2]);
    // => [2, 1, 4]
    ```

12. **_.unionBy**
    - 설명: 여러 배열의 요소를 결합하고 iteratee를 사용하여 중복 요소를 제거합니다.
    ```javascript
    _.unionBy([2.1, 1.2], [4.3, 2.4], Math.floor);
    // => [2.1, 1.2, 4.3]
    ```

13. **_.unionWith**
    - 설명: comparator 함수를 사용하여 배열의 값들을 비교하고 여러 배열의 요소를 결합합니다. comparator가 일치하는 경우 해당 값을 제거합니다.
    ```javascript
    const objects = [{ 'x': 1, 'y': 2 }, { 'x': 2, 'y': 2 }];
    _.unionWith(objects, [{ 'x': 1, 'y': 2 }], _.isEqual);
    // => [{ 'x': 1, 'y': 2 }, { 'x': 2, 'y': 2 }]
    ```

14. **_.uniq**
    - 설명: 배열의 중복 요소를 제거합니다.
    ```javascript
    _.uniq([2, 1, 2]);
    // => [2, 1]
    ```

15. **_.uniqBy**
    - 설명: iteratee를 사용하여 배열의 중복 요소를 제거합니다.
    ```javascript
    _.uniqBy([2.1, 1.2, 2.3], Math.floor);
    // => [2.1, 1.2]
    ```

16. **_.uniqWith**
    - 설명: comparator 함수를 사용하여 배열의 중복 요소를 제거합니다.
    ```javascript
    const objects = [{ 'x': 1, 'y': 2 }, { 'x': 2, 'y': 2 }, { 'x': 1, 'y': 2 }];
    _.uniqWith(objects, _.isEqual);
    // => [{ 'x': 1, 'y': 2 }, { 'x': 2, 'y': 2 }]
    ```

17. **_.shuffle**
    - 설명: 배열의 요소 순서를 무작위로 섞습니다.
    ```javascript
    _.shuffle([1, 2, 3, 4]);
    // => 예) [4, 1, 3, 2] (출력 결과는 무작위로 변할 수 있습니다.)
    ```

18. **_.cloneDeep**
    - 설명: 객체나 배열을 깊게 복사합니다.
    ```javascript
    const objects = [{ 'a': 1 }, { 'b': 2 }];
    const deep = _.cloneDeep(objects);
    console.log(deep[0] === objects[0]);
    // => false
    ```

19. **_.cloneDeepWith**
    - 설명: 객체나 배열을 깊게 복사하며, customizer 함수를 사용하여 복사하는 방법을 지정할 수 있습니다.
    ```javascript
    function customizer(value) {
      if (_.isElement(value)) {
        return value.cloneNode(true);
      }
    }
    const el = _.cloneDeepWith(document.body, customizer);
    console.log(el === document.body);
    // => false
    console.log(el.nodeName);
    // => 'BODY'
    ```

20. **_.debounce**
    - 설명: 연속된 호출을 지연시킨 후 마지막 함수 호출을 실행합니다. 주로 스크롤 이벤트나 입력 검사와 같은 반복적인 이벤트 처리에 사용됩니다.
    ```javascript
    const debouncedSave = _.debounce(function(text) {
      console.log(text);
    }, 1000);

    debouncedSave('unsaved input 1');  // 출력되지 않습니다.
    debouncedSave('unsaved input 2');  // 출력되지 않습니다.
    // 1초 후 'unsaved input 2'가 출력됩니다.
    ```

21. **_.throttle**
    - 설명: 함수 호출을 일정 시간 간격으로 제한합니다. 주로 스크롤 이벤트와 같은 연속적인 이벤트에 사용됩니다.
    ```javascript
    const throttledSave = _.throttle(function(text) {
      console.log(text);
    }, 1000);

    throttledSave('scroll event 1');  // 'scroll event 1'이 바로 출력됩니다.
    throttledSave('scroll event 2');  // 1초 이내에는 출력되지 않습니다.
    ```

---
### reduce (345 page)
 - 모든 항목을 순회하면서 하나의 값으로 만들 수 있는 기능 (총합, 각 요소 빈도수 계산, 배열 평탄화 ...)
 - 책에서는 각 요소의 빈도수 계산을 통해 장바구니를 만들고 있다.
#### event sourcing
 - 이벤트 소싱(Event Sourcing) 이란, 시스템의 상태를 이벤트의 시퀀스로 저장하는 설계 패턴입니다. 즉, 시스템의 상태를 변경하는 모든 동작(이벤트)를 로그로 저장하고, 이 로그를 순차적으로 재생하면 현재의 상태를 얻을 수 있습니다.
 - 예) 은행 계좌
   - 계좌 생성
   - 100원 입금
   - 30원 출금
 - 이러한 방식으로 시스템의 상태를 이벤트 로그를 통해 계산하고, 필요한 시점의 상태를 재생성하는 것이 이벤트 소싱의 핵심 개념입니다.
###### 연습문제
 - 352page roster 함수에 evaluations가 맞나? evaluationsDescending이 맞나 

---

### 정리
 - 복잡한 문제를 작고 명확한 단계로 나누고, 함수형 도구를 조합해 문제를 해결하자.
