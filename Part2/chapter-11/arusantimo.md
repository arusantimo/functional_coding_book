# 일급 함수 2

## 카피온 라이트 리팩토링 (배열, 객체)

분리 1단계

1. 중복 코드를 감지 한다.
2. 중복 코드중 변경점을 체크한다.
3. 변경점을 값이나 함수 객체등 1급 객체로 분리할수 있는지 체크한다.
4. 변경점을 함수 (로직)으로 분리할수 있게 하고, 값으로써 활용해야 한다면, 값의 평가 지점을 분리한다. (콜백으로 분리)

```javascript
function arraySet(array, idx, value) {
  var copy = array.slice(); // 앞부분 ---> 공통
  copy[idx] = value; // (로직) * 변경점
  return copy; // 뒷부분 ---> 공통
}

function push(array, elem) {
  var copy = array.slice(); // 앞부분 ---> 공통
  copy.push(elem); // 본문 (로직) * 변경점
  return copy; // 뒷부분 ---> 공통
}
```


중복 코드 감지 하여 함수로 분리
```javascript
function arraySet(array, idx, value) {
  return withArrayCopy(array, (copy) => {
    copy[idx] = value;
  });
}

function withArrayCopy(array, modify) {
  var copy = array.slice();
  modify(copy); 
  return copy;
}
```

빼낸 함수에서 변경 로직 분리 하기 (인자로)

```javascript
function withObjectCopy(object, modify) {
  var copy = Object.assign({}, object);
  modify(copy);
  return copy;
}

만약 외부의 선언된 변수의 값을 넘겨주면 callbySharing(Reference, Pointer) 

function objectSet(object, key, value ==> callbyValue) { // 함수 컨텍스트에 종속된 새로운 값을 위한 메모리 공간 확보
  return withObjectCopy(object, (copy) => {
    copy[key] = value; value ==> callbyName
  });
}

function objectDelete(object, key) {
  return withObjectCopy(object, (copy) => {
    delete copy[key];
  });
}
```


####  콜백 함수를 화살표 함수로 하면 좋은 이유.

불필요한 this 참조 aguments나 프로토타입 선언이 없어서 일반 익명함수에 비해서 가볍게 사용할수 있다.  
하지만 가장 중요한것은 이것을 es6스팩에 넣고자 할때 함수를 값으로 사용하기 위해서 만들어져 졌고, 그래서 축약형 문법을 만들었다.  
제작한 사람의 의도는 함수형 언어에서의 함수를 값으로 사용할때 쓰는 문법을 사용하고자 만들었고, 스팩또한 그렇게 제한했음.


## (고차 함수)

> 고차함수 인자를 함수로 받거나, 출력을 함수로 한다. 

### 함수를 인자로 받는 고차 함수
```javascript
Array.prototype.customMap = function (func) {
    const result = [];
    let index = -1;
    for (const value of this) {
        index++;
        result.push(
            func(value, index, this) // 이  값은 함수가 실행 되는 시점에 평가 된다.
        );
    }    
    return result;
};
```

### 함수를 리턴하는 고차 함수

책에 나온 방밥은 좋은 방법이 아님.

try catch 에서 동일한 타입의 객체(함수 or 데이터)를 리턴하지 않기 때문에 실행하는 시점 마다 다른 형태의 값이 함수에서 나오게 된다.
만약 고차함수화 할것이라면, 이렇게 했어햐함.

```javascript
function wrapLogging(f) {
  return function(arg) {
    try {
        f(arg);
    } catch (error) {
        logToSnapErrors(error);
        throw new RuntimeException(error);
    } finally {
        f(arg); // 이걸 했어야한다.
    }
  }
}
```

개인 의견
> 고차함수는 고급 추상화 기법으로 재사용성을 높여준다. 
> 하지만 가장 중요한 점은 중복 코드와 분리코드를 평가시점을 다르게 해준다는 것이다.
> 예를 들어 중복 코드가 비용이 많은 드는 코드라고 해보자. 그런 평가를 매번 해줘야 하는것 보다는, 평가 시점을 함수 실행 이후 시점으로 
> 미뤄 두고 꼭 해야 하는 시점에만 하게 한다면 성능 향상에 도움을 줄수 있다. 
> 하지만 콜바이밸류의 경우 언제 실행 될지 함수 실행 시점 전에는 알수 없기 때문에 이런 최적화를 할수 없다.
> 
> 그렇기 때문에 1급 함수의 가장 큰 장점 중하나가 `런타임에 생성이 가능하다.` 인 것이다.
>
> 프론트엔드에서도 이런 방법으로 사용하면 좋은것이 이벤트 핸들러 에서 평가 시점을 조절할수있다. 이벤트 핸들러에 값을 전달하는 방식으로 값을 평가할때 함수가 실행되어 진 다음 시점으로 미뭐야지 정상적인 값 평가가 가능하다.
