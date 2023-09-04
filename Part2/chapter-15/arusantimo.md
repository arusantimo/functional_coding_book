# 타임 라인 그리기

- `사용자의 액션`에 의해 시작됨.  책에서는 `클릭`으로 시작되어짐.
- 순서가 보장되면 `수직`
- 순서가 보장 되지 않으면 `수평`
- `읽기`, `쓰기`로 크게 분류 가능함
- 인자 읽기는 함수 부르기 전에 단계에 실행됨.



#### 비동기 호출은 새로운 타임라인으로 시작 (수평)

자바스크립트는 싱글 스레드 언어로, 비동기프로그램을 지원함.

## 타임 라인그리기 방법. 저자

### 그리는 방법
1. 액션/함수를 박스로 표시
2. 액션의 알수없는 경과 시간을 선으로 표시
3. 경과시간을 알수 없어서 순서보장을 할수 없을때만 선으로 표시
4. 순서 보장된 액션은 하나의 박스로 표시


### 원칙
1. (박스, 라인) 적을수록 쉽다.
2. 짧을 수록 쉽다.
3. 타임라인 끼리 의존성을 줄일 수록 쉽다.
4. 타임라인끼리의 데이터 의존성 규칙을 만들어야한다. (pull, push)
5. 타임라인 전용 객체를 다룬다. (순서를 위한 객체)


### 재사용 하기 더 좋은 코드로 만들기

```JS
function calc_cart_total(cart, callback) {
  var total = 0;
  cost_ajax(cart, (cost) => {
    total += cost;
    shipping_ajax(cart, (shipping) => {
        
      total += shipping; // 조건을 편집할수 있음.      
      
      callback(total);
    });
  });
}

function add_item_to_cart(name, price, quant) {
  cart = add_item(cart, name, price, quant);
  calc_cart_total(cart, update_total_dom);
}

function update_total_dom(total) {
    // 조건
    if (total > 10  && $('#user').isShow) { // 화면상 오류를 피할수  있는 조건
        $('#total').text(total);    
    }
}

```

재사용성도 중요하지만, update_total_dom 내부의 평가 값(조건등)을 콜백실행 시점으로 제어하는 것이 의미가 더 크다.!


## 타임 작성 규칙. 내가

1. 콜스택이 비워지기 전까지는 무조건 테스크큐는 다음 순서다.
2. 가독성 있고, block한 코드 실행을 위해 async await문버을 활용.
3. 테스크큐 우선순위를 활용
4. 하나의 액션에 활용되는 휘발용 객체를 활용하면 좋음.
5. 하나의 큰 코루틴 구조로 타임라인을 설계하면 좋음. (그안에서 활용) async await, generator yeild **제어권

https://github.com/arusantimo/Archive_JJ/wiki/Promise-U-~#but-%EC%BD%9C%EB%B0%B1--%ED%94%84%EB%A1%9C%EB%AF%B8%EC%8A%A4


- 태스크 큐(Task Queue): `setTimeout()`, `setInterval()`과 같은 비동기 함수의 콜백 함수 또는 이벤트 핸들러가 대기하는 곳이다.
- 마이크로 태스크 큐(Micro Task Queue): `Promise()`의 후속 처리 메서드의 콜백 함수나 `MutationObserver()`가 대기하는 곳이다.
- 리퀘스트 애니메이션 프레임 큐(Request Animation Frame Queue): `requestAnimationFrame()`처럼 애니메이션을 업데이트하는 콜백 함수가 대기하는 곳이다.

```js
console.log('처음');

setTimeout(() => {
  console.log('setTimeout - 태스크 큐');
}, 0);

Promise.resolve()
  .then(() => {
    console.log('promise1 - 마이크로 태스크 큐');
  })
  .then(() => {
    console.log('promise2 - 마이크로 태스크 큐');
  });

requestAnimationFrame(() => {
  console.log('requestAnimationFrame - rAF 큐');
});

for (let i = 0; i < 1000; i++) {
  console.log('for문');
}
console.log('마지막');


/*
처음
for문 * 1000
마지막
--
promise1 - 마이크로 태스크 큐
promise2 - 마이크로 태스크 큐
--
setTimeout - 태스크 큐
--
requestAnimationFrame - rAF 큐
 */
```





```js
customElement.prototype.getData = (url) => {
  if (this.cache[url]) {
    this.data = this.cache[url];
    this.dispatchEvent(new Event("load"));
  } else {
    fetch(url)
      .then((result) => result.arrayBuffer())
      .then((data) => {
        this.cache[url] = data;
        this.data = data;
        this.dispatchEvent(new Event("load"));
      });
  }
};

element.addEventListener("load", () => console.log("Loaded data"));
console.log("Fetching data...");
element.getData();
console.log("Data fetched");

/* 1회차
Fetching data...
Data fetched
Loaded data
 */

/*2회차
Fetching data...
Loaded data
Data fetched
 */

queueMicrotask // 활용 하기

customElement.prototype.getData = (url) => {
    if (this.cache[url]) {
        queueMicrotask(() => {
            this.data = this.cache[url];
            this.dispatchEvent(new Event("load"));
        });
    } else {
        fetch(url)
            .then((result) => result.arrayBuffer())
            .then((data) => {
                this.cache[url] = data;
                this.data = data;
                this.dispatchEvent(new Event("load"));
            });
    }
};

```


```js
let urgentCallback = () => console.log("*** 앗! 긴급 콜백을 실행했습니다!");
let callback = () => console.log("일반 타임아웃 콜백을 실행했습니다.");

let doWork = () => {
  let result = 1;

  urgentCallback();

  for (let i = 2; i <= 10; i++) {
    result *= i;
  }
  return result;
};

console.log("주 프로그램 시작");
setTimeout(callback, 0);
console.log(`10!은 ${doWork()}과 같습니다.`);
console.log("주 프로그램 종료");

/*
주 프로그램 시작
*** 앗! 긴급 콜백을 실행했습니다!
10!은 3628800과 같습니다.
주 프로그램 종료
--
일반 타임아웃 콜백을 실행했습니다.
 */


let urgentCallback = () => console.log("*** 앗! 긴급 콜백을 실행했습니다!");
let callback = () => console.log("일반 타임아웃 콜백을 실행했습니다.");

let doWork = () => {
    let result = 1;

    queueMicrotask(urgentCallback);

    for (let i = 2; i <= 10; i++) {
        result *= i;
    }
    return result;
};
console.log("주 프로그램 시작");
setTimeout(callback, 0);
console.log(`10!은 ${doWork()}과 같습니다.`);
console.log("주 프로그램 종료");

/*
주 프로그램 시작
10!은 3628800과 같습니다.
주 프로그램 종료
*** 앗! 긴급 콜백을 실행했습니다!
---
일반 타임아웃 콜백을 실행했습니다.
 */
```
