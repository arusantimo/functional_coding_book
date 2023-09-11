### 코드를 바꾸었더니 속도는 빨라졌지만 버그가 발생했다. (코드 참조)
 - cost_ajax 콜백에서 shipping_ajax를 빼냈다.
 - coat_ajax의 콜백이 먼저 실행되고 나서 shipping_ajax의 콜백이 실행되어야만 정상적이다
 - 하지만, 콜백의 실행순서는 보장되지 않는다. (어떤 요청에 응답이 먼저 올지 여러 네트워크 상황에 따라 다르기 때문) 484page

### cut 함수로 해결해보자 (코드 참조)
 - 자바스크립트는 단일 스레드로 동작한다. 다른 언어에서는 락이나 다른 기능을 사용해야 한다.
 - 몇번 호출 후 어떤 콜백을 실행 할 것인가.

### JustOnce 함수 (코드 참조)


### 번외 Promise 함수 기능 (https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Promise)
#### Promise.all(iterable)
##### 용도: 여러 개의 Promise를 병렬로 처리하고, 모든 Promise가 성공적으로 완료되면 결과를 반환합니다.
##### 반환 값: 모든 Promise들의 결과 값들을 담은 배열을 반환합니다.
##### 에러 처리: 하나라도 실패하면, 즉시 전체 Promise가 reject 상태가 됩니다.
##### 예시:
```javascript
Promise.all([promise1, promise2, promise3])
  .then(values => {
    console.log(values); // 모든 promise가 성공적으로 완료된 경우, values는 각각의 결과 값들을 담은 배열입니다.
  })
  .catch(error => {
    console.error(error); // 하나라도 실패한 경우, error는 실패한 그 Promise의 에러입니다.
  });
```

#### Promise.allSettled(iterable)
##### 용도: 여러 개의 Promise를 병렬로 처리하고, 모든 Promise가 settle (성공 또는 실패)되면 결과를 반환합니다.
##### 반환 값: 모든 Promise들의 상태와 결과 값을 담은 객체들을 배열로 반환합니다.
##### 에러 처리: 모든 Promise들이 settle되면 (성공이든 실패든) 결과를 반환합니다.
##### 예시:
```javascript
Promise.allSettled([promise1, promise2, promise3])
  .then(results => {
    results.forEach((result, index) => {
      if (result.status === 'fulfilled') {
        console.log(`Promise ${index} 결과: ${result.value}`);
      } else {
        console.error(`Promise ${index} 실패: ${result.reason}`);
      }
    });
  });
```

#### Promise.any(iterable)
##### 용도: 여러 개의 Promise 중 하나라도 성공하면 그 결과를 반환합니다.
##### 반환 값: 첫 번째로 성공한 Promise의 결과 값을 반환합니다.
##### 에러 처리: 모든 Promise가 실패하면, 에러를 반환합니다.
##### 예시:
```javascript
Promise.any([promise1, promise2, promise3])
  .then(value => {
    console.log(value); // 첫 번째로 성공한 Promise의 결과 값입니다.
  })
  .catch(error => {
    console.error(error); // 모든 Promise가 실패한 경우의 에러입니다.
  });
```

#### Promise.race(iterable)
##### 용도: 여러 개의 Promise 중에서 첫 번째로 settle (성공 또는 실패)한 Promise의 결과를 반환합니다.
##### 반환 값: 첫 번째로 settle한 Promise의 결과 또는 에러를 반환합니다.
##### 에러 처리: 첫 번째로 실패한 Promise의 에러를 반환합니다.
##### 예시:
```javascript
Promise.race([promise1, promise2, promise3])
  .then(value => {
    console.log(value); // 첫 번째로 성공한 Promise의 결과 값입니다.
  })
  .catch(error => {
    console.error(error); // 첫 번째로 실패한 Promise의 에러입니다.
  });
```
