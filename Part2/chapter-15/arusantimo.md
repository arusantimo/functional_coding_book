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
5. 타임라인 전용 객체를 다룬다. (순서를 위한 객체) 멀티스레딩에서 차용된듯.


### 재사용 하기 더 좋은 코드로 만들기

```JS
function calc_cart_total(cart, callback) {
  var total = 0;
  cost_ajax(cart, function(cost) {
    total += cost;
    shipping_ajax(cart, function(shipping) {
      total += shipping;
      callback(total);
    });
  });
}

function add_item_to_cart(name, price, quant) {
  cart = add_item(cart, name, price, quant);
  calc_cart_total(cart, update_total_dom);
}
```

재사용성도 중요하지만, update_total_dom 내부의 값을 콜백실행 시점으로 제어하는 것이 의미가 더 크다.!


## 타임 작성 규칙. 내가

1. 콜스택이 비워지기 전까지는 무조건 테스크큐는 다음 순서다.
2. 가독성 있고, block한 코드 실행을 위해 async await문버을 활용.
3. 테스크큐 우선순위를 활용
4. 하나의 액션에 활용되는 휘발용 객체를 활용하면 좋음.

https://github.com/arusantimo/Archive_JJ/wiki/Promise-U-~#but-%EC%BD%9C%EB%B0%B1--%ED%94%84%EB%A1%9C%EB%AF%B8%EC%8A%A4

    
