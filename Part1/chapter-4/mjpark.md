# 액션에서 계산 빼내기

## 테스트를 쉽게 하려면 + 재사용이 하기 쉽게 하려면

- DOM 업데이트와 비즈니스 규칙은 분리되어야 한다
- 전역변수가 없어야 한다
- 함수가 결과값을 리턴해야 한다

## 액션과 계산, 데이터 구분하기

<code>**전역변수는 변경 가능하기 때문에 액션!<br/> </code>
<code>**액션은 코드 전체로 퍼진다. </code>

## 함수의 입력과 출력

<i>입력과 출력은 명시적이거나 암묵적일 수 있다</i>

- 함수의 인자, 출력 → 명시적
- 암묵적인 입력과 출력이 있으면 → 액션이 된다

<code>암묵적 입력과 출력 => 부수효과</code>

## 계산 추출의 단계

1. 계산 코드를 찾아서 새로운 함수를 만들기
2. 새 함수에 암묵적 입력과 출력 찾기

- 암묵적 입력 : 함수를 부르는 동안 결과에 영향을 줄 수 있는 것
  ex) 함수 밖 변수 읽기, DB읽기
- 암묵적 출력 : 함수 호출의 결과롤 영향을 받는 것
  ex) 공유 객체를 바꾸거나, 웹 요청 보내기

3. 암묵적 입력은 인자로, 암묵적 출력은 리턴값으로 바꾸기

연습문제1) 세금계산 코드에서 계산 추출하기

```javascript
function update_tax_dom() {
  set_tax_dom(shopping_cart_total * 0.1);
}
```

```javascript
function calc_tax(total) {
  return total * 0.1;
}
function update_tax_dom() {
  set_tax_dom(calc_tax(shopping_cart_total));
}
```

연습문제2) 무료배송 확인 코드에서 계산 추출하기

```javascript
function update_shipping_icons() {
  var buy_buttons = get_buy_buttons_dom();
  for (var i = 0; i < buy_buttons.length; i++) {
    var button = buy_buttons[i];
    var item = button.item;
    if (item.price + shopping_cart_total >= 20) {
      button.show_free_shipping_icon();
    } else {
      button.hide_free_shipping_icon();
    }
  }
}
```

```javascript
function check_free_shopping(price, total) {
  return total >= 20;
}

function update_shipping_icons() {
  var buy_buttons = get_buy_buttons_dom();
  for (var i = 0; i < buy_buttons.length; i++) {
    var button = buy_buttons[i];
    var item = button.item;
    if (check_free_shopping(item.price, shopping_cart_total)) {
      button.show_free_shipping_icon();
    } else {
      button.hide_free_shipping_icon();
    }
  }
}
```

---

### 함수형 원칙을 적용하면 액션은 줄어들고 계산은 늘어난다

\*)
https://codesandbox.io/s/ripaegtoring2pan-yesi-forked-cqngcg
